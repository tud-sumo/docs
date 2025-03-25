# Basic Simulations

## SUMO Scenarios

Before anything can be simulated, all the necessary SUMO scenario files are required. This will typically include a '_.sumocfg_', '_.neteditcfg_', '_.net.xml_', '_.rou.xml_' and '_.add.xml_'. The simplest way to create a scenario and these files is using netedit, about which there is more information [here](https://sumo.dlr.de/docs/Netedit/index.html).

## Initialising the Simulation

All simulations in TUD-SUMO are created and run using the `Simulation` class, which is initialised as below. A scenario name and description are optional, but can be useful when running multiple similar simulations.

```python
from tud_sumo.simulation import Simulation

my_sim = Simulation(scenario_name="example", scenario_desc="Example simulation.")
```

To then start the simulation and create the connection to SUMO through TraCI, use `Simulation.start()`. This can very easily be done using the '_.sumocfg_' file created by netedit, which links all to all other files, however, the `net_file`, `route_file`, `add_file` and `gui_file` parameters can also be used to give each file individually. Whether or not to use the GUI is also set here, although note this cannot be changed throughout the simulation.

```python
my_sim.start("example_scenario.sumocfg",
             gui=True,
             get_individual_vehicle_data=False,
             units="metric",
             suppress_pbar=False,
             seed=1
            )
```

The `get_individual_vehicle_data` is an important parameter denoting whether to collect and save dynamic information for all vehicles (ie. position, speed, acceleration etc.) at each step. This can be useful for small scenarios where this data may be required and computation time is less of an issue, such as single intersections, however, this should be set to false for large scenarios, such as large motorway networks.

3 unit settings are supported '_metric_' (km/kmph), '_imperial_' (mi/mph) and '_UK_' (km/mph). All data collected and saved are in these units, and this setting cannot be changed later.

The `seed` parameter is optional and affects both the SUMO simulation and the `Simulation` class. Random seeds in SUMO primarily affects how vehicles are added into the simulation (more information can be found [here](https://sumo.dlr.de/docs/Simulation/Randomness.html)). The seed can be set to random either by not using the parameter or by setting it to '_random_'.

By default, a progress bar is automatically created when simulating more than 10 steps at a time. This can be skipped by setting `suppress_pbar = False`. 

Tracked junctions/edges, controllers etc. can be initialised at this point. These objects can be added individually, or if all of their parameters are saved in a dictionary or '_.json_' or '_.pkl_' file, these can be read using the `Simulation.load_objects()` function. This dictionary can be created manually, or can be saved with `Simulation.save_objects()` for ease of use. An example of this resulting file can be found [here](https://github.com/DAIMoNDLab/tud-sumo-examples/blob/main/a20_example/objects.json), otherwise, more information on the objects themselves can be found in their respective sections.

```python
# Save object initialisation parameters
my_sim.save_objects("objects.pkl")

# Initialise objects using the same parameters
my_sim.load_objects("objects.pkl")
```

The following objects can be included in this dictionary:

  - '_edges_': List of edge IDs for tracking.
  - '_junctions_': [Tracked junction](6_tracked_objs.md/#tracked-junctions) initialisation parameters.
  - '_phases_': Traffic light [phase dictionary](7_traffic_control.md/#traffic-signal-control).
  - '_controllers_': Dictionary containing [VSL](7_traffic_control.md/#variable-speed-limits) and/or [RG](7_traffic_control.md/#dynamic-route-guidance) controller parameters.
  - '_events_': Dictionary containing [scheduled event](8_events.md/#scheduled-events) parameters (previous dynamic events are saved in the resulting file).
  - '_demand_': Dictionary containing a [demand profile](4_demand.md) table. These are made by creating a demand profile and saving using `Simulation.save_objects()`. 
  - '_routes_': A dictionary containing new routes by their ID and a (2x1) array representing the origin and destination.

## Running the Simulation

The simulation is run using the `Simulation.step_through()` function. When no parameters are given, the simulation will run through one step by default. Otherwise, using `n_steps` will run the simulation for a specific number of steps, `end_step` will run the simulation until a specific step and `n_seconds` will run the simulation for a specific amount of time (in seconds).

```python
# Run 1 step
my_sim.step_through()

# Run for 100 steps
my_sim.step_through(100)

# Run until step 200
my_sim.step_through(end_step=200)

# Run for 100 seconds (where step length = 0.5)
my_sim.step_through(n_seconds=200)
```

A control loop can, therefore, be created as below. A progress bar is automatically created when simulating for 10 or more steps in one call of `Simulation.step_through()`. In order to create a progress bar that is consistent between separate calls, use the `pbar_max_steps` parameter and set this to the total length of the simulation.

```python
n, sim_dur = 100, 2500
while my_sim.curr_step < sim_dur: # or use my_sim.curr_time

    # Step through n steps.
    my_sim.step_through(n_steps=n, pbar_max_steps=sim_dur)

    # Perform control
    # ...
```

To include a warmup period where no data is collected, set `keep_data = True`.

```python
warmup_length = 120
my_sim.step_through(n_seconds=warmup_length, keep_data=True)
```

## Ending the Simulation

A simulation can be run until it is finished as below. The `Simulation.is_running()` function returns false once the simulation is over, which is determined as happening once all defined vehicles have finished their run through the simulation. It is then best to end the simulation using `Simulation.end()`, which closes the connection to TraCI.

```python
while my_sim.is_running():
    my_sim.step_through()

my_sim.end()
```

## Automatic Data Collection

One of the major advantages of TUD-SUMO is the automatic data collection. This involves automatically collecting all basic simulation information into a `sim_data` dictionary. An example of this can be seen in examples directory in the main TUD-SUMO repository, however, the main structure is as follows:

```JSON
{
    "data":
        {
            "detectors": {},
            "junctions": {},
            "edges": {},
            "controllers": {},
            "vehicles": {},
            "demand": {},
            "trips": {},
            "events": {},
            "all_vehicles": {}
        },
    "start": 0,
    "end": 1000,
    "step_len": 1.0,
    "units": "METRIC",
    "seed": 10,
    "sim_start": "08/07/2024, 13:00:00",
    "sim_end": "08/07/2024, 13:00:10"
}
```

Detectors will automatically collect vehicle speeds and counts for each time step, as well as the IDs of each vehicle that passed over it, whilst occupancies are also collected for mutli-entry-exit detectors. These will be stored under '_data/detectors/{detector_id}_', with '_type_', '_position_', '_speeds_', '_vehicle_counts_', '_vehicle_ids_' and '_occupancies_'.

Vehicle data will include the number of vehicles at each step, '_twt_' (No. waiting vehicles * step length), '_tts_' (N. vehicles * step length), '_delay_' (calculated based on vehicle speed and ideal free-flow speed) and '_to_depart_' (the number of vehicles waiting to be inserted at each time step).

Demand is only included when dynamically adding demand, ie. not when demand is solely defined in the '_.rou.xml_' file. This dictionary will contain two objects; '_headers_' and a list of demand profile tables. Each table in '_profiles_' will contain a (9 x n) sized array with information on flows added through demand profiles, with its headers listed under '_headers_'.

Trip data will contain data for incomplete and completed trips. This is stored under '_data/trips_' and then either '_incomplete_' or '_completed_'. Each trip will store the '_route_id_', '_vehicle_type_', '_departure_', '_arrival_' (or removal), '_origin_' and '_destination_'.

Junction, edge, controller and event data are only included when necessary. The `all_vehicles` data will contain all the individual vehicle data at each time step, so it is only included when `get_individual_vehicle_data` is set to true when starting the simulation.

The `sim_data` dictionary can be reset at any point using the `Simulation.reset_data()` function.

```python
my_sim.reset_data()
```

## Saving & Summarising Data

All the data collected throughout the simulation can be saved at any point using the `Simulation.save_data()` function. This will save the `sim_data` dictionary as a file in the specified directory. Either JSON or pickle files are supported, simply denoted by a '_.json_' or '_.pkl_' extension in the filename.

```python
my_sim.save_data("data/example_data.json")
my_sim.save_data("data/example_data.pkl")
```

All data saved by a simulation or a simulation data file can be summarised using the `Simulation.print_summary()` or `print_summary()` functions. This will print a summary of the collected data (ie. number of vehicles, TTS, controllers, events etc.), as well as some information about the simulation itself (ie. scenario name/description, runtime, seed etc.). This summary can be saved to a '_.txt_' file using the `save_file` parameter. An example summary is shown below.

```python
my_sim.print_summary(save_file="data/example_summary.txt")

# Print summary without creating a Simulation object
from tud_sumo.simulation import print_summary
print_summary("data/example_data.pkl")
```

```
 *============================================================*
 |                      TUD-SUMO v3.2.2                       | 
 *============================================================*
 |                          A20_ITCS                          | 
 *============================================================*
 |                        Description:                        | 
 |   Example traffic controllers, with 2 ramp meters, 1 VSL   | 
 |        controller and 1 route guidance controller.         | 
 *============================================================*
 |                 Simulation Run: 25/03/2025                 | 
 |               17:13:43 - 17:13:58 (0:00:15)                | 
 *------------------------------------------------------------*
 | Number of Steps:                               500 (0-500) | 
 | Step Length:                                          1.0s | 
 | Avg. Step Duration:                                  0.03s | 
 | Units Type:                              Metric (km, km/h) | 
 | Seed:                                                    1 | 
 *============================================================*
 |                            Data                            | 
 *============================================================*
 |                        Vehicle Data                        | 
 *------------------------------------------------------------*
 |                       No. Vehicles:                        | 
 | Average:                                            736.24 | 
 | Peak:                                                 1211 | 
 | Final:                                                1211 | 
 | Overall TTS:                                       368118s | 
 * ---------------------------------------------------------- *
 |                   No. Waiting Vehicles:                    | 
 | Average:                                             23.43 | 
 | Peak:                                                   69 | 
 | Final:                                                  55 | 
 | Overall TWT:                                        11713s | 
 * ---------------------------------------------------------- *
 |                       Vehicle Delay:                       | 
 | Average:                                           392.71s | 
 | Peak:                                              788.69s | 
 | Final:                                              783.1s | 
 | Cumulative Delay:                               196352.73s | 
 * ---------------------------------------------------------- *
 |                    Vehicles to Depart:                     | 
 | Average:                                           1063.27 | 
 | Peak:                                                 2227 | 
 | Final:                                                2227 | 
 * ---------------------------------------------------------- *
 | Individual Data:                                        No | 
 *------------------------------------------------------------*
 |                         Trip Data                          | 
 *------------------------------------------------------------*
 | Incomplete Trips:                            1213 (65.18%) | 
 | Completed Trips:                               648 (34.8%) | 
 *------------------------------------------------------------*
 |                         Detectors                          | 
 *------------------------------------------------------------*
 |               Induction Loop Detectors: (15)               | 
 |       a13_ramp_inflow, cw_down_occ_0, cw_down_occ_1,       | 
 |  cw_down_occ_2, cw_down_occ_3, cw_up_occ_0, cw_up_occ_1,   | 
 |      cw_up_occ_2, rerouter_2, utsc_e_in, utsc_e_out,       | 
 |      utsc_n_in_1, utsc_n_in_2, utsc_w_in, utsc_w_out       | 
 |                                                            | 
 |              Multi-Entry-Exit Detectors: (7)               | 
 |    a13_ramp_queue, a13_rm_downstream, a13_rm_upstream,     | 
 |      cw_ramp_inflow, cw_ramp_queue, cw_rm_downstream,      | 
 |                       cw_rm_upstream                       | 
 *------------------------------------------------------------*
 |                       Tracked Edges                        | 
 *------------------------------------------------------------*
 | 126730026, 1191885773, 1191885771, 126730171, 1191885772,  | 
 |         948542172, 70944365, 308977078, 1192621075         | 
 *------------------------------------------------------------*
 |                     Tracked Junctions                      | 
 *------------------------------------------------------------*
 |                     utsc (Signalised)                      | 
 |           crooswijk_meter (Signalised, Metered)            | 
 |              a13_meter (Signalised, Metered)               | 
 *------------------------------------------------------------*
 |                        Controllers                         | 
 *------------------------------------------------------------*
 |                    Route Guidance: (1)                     | 
 |                          rerouter                          | 
 |                                                            | 
 |                 Variable Speed Limits: (1)                 | 
 |                            vsl                             | 
 *------------------------------------------------------------*
 |                    Event IDs & Statuses                    | 
 *------------------------------------------------------------*
 |                 Active: incident_response                  | 
 |             Completed: incident_1, bottleneck              | 
 *------------------------------------------------------------*
```

The structure of a simulation data file can also be printed using the `Simulation.print_sim_data_struct()` or `print_sim_data_struct()` functions. This will print the structure of the `sim_data` dictionary as a tree, allowing you to see the exact keys and data types used in the data. An example of the output of `print_sim_data_struct()` is shown below.

Note that the dimensions of array are only displayed up to 2D. If an array has a higher dimension, this is denoted by a '_+_'. If the array is inhomogeneous (ie. its dimensions are inconsistent), this is denoted by a '_*_' and the array's maximum size is shown.

```python
my_sim.print_sim_data_struct()

# Print structure without creating a Simulation object
from tud_sumo.simulation import print_sim_data_struct
print_sim_data_struct("data/example_data.pkl")
```

```
A20_ITCS:
  ├─- scenario_name: str
  ├─- scenario_desc: str
  ├─- tuds_version: str
  ├─- data:
  |     ├─- detectors:
  |     |     ├─- a13_ramp_queue:
  |     |     |     ├─- type: str
  |     |     |     ├─- position:
  |     |     |     |     ├─- entry_lanes: tuple (1x1)
  |     |     |     |     ├─- exit_lanes: tuple (1x1)
  |     |     |     |     ├─- entry_positions: tuple (1x1)
  |     |     |     |     └─- exit_positions: tuple (1x1)
  |     |     |     ├─- speeds: list (1x500)
  |     |     |     ├─- vehicle_counts: list (1x500)
  |     |     |     └─- vehicle_ids: list (500x28*)
  .     .     .
  .     .     .
  .     .     .
  |     ├─- vehicles:
  |     |     ├─- no_vehicles: list (1x500)
  |     |     ├─- no_waiting: list (1x500)
  |     |     ├─- tts: list (1x500)
  |     |     ├─- twt: list (1x500)
  |     |     ├─- delay: list (1x500)
  |     |     └─- to_depart: list (1x500)
  |     └─- trips:
  |           ├─- incomplete: dict (1x1213)
  |           └─- completed: dict (1x648)
  ├─- start: int
  ├─- end: int
  ├─- step_len: float
  ├─- units: str
  ├─- seed: int
  ├─- sim_start: str
  └─- sim_end: str
```