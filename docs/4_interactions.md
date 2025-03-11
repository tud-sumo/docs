# Interacting with the Simulation

## Testing Values

Several functions exist to check values in the simulation, primarily to check wether objects exist and get their type or status. All of these are within the `Simulation` class, and are:

### Vehicles
| Function                  | Return Value                                                                            |
|---------------------------|-----------------------------------------------------------------------------------------|
| `vehicle_exists(ID)`      | Whether or not a vehicle with the given ID exists in the simulation.                    |
| `vehicle_loaded(ID)`      | Whether or not a vehicle has been scheduled to enter (or is already in) the simulation. |
| `vehicle_to_depart(ID)`   | Whether or not a vehicle has been scheduled to enter (but is not in) the simulation.    |
| `vehicle_type_exists(ID)` | Whether or not a vehicle type with the given ID exists.                                 |
| `route_exists(ID)`        | Route edges if the ID exists, else returns None.                                        |

### Other Objects
| Function                      | Return Value                                               |
|-------------------------------|------------------------------------------------------------|
| `detector_exists(ID)`         | Detector type if the ID exists, else returns None.         |
| `controller_exists(ID)`       | Whether a controller with the ID exists.                   |
| `event_exists(ID)`            | Event status if the ID exists, else returns None.          |
| `geometry_exists(ID)`         | Geometry type if ID exists (lane/edge), else returns None. |
| `tracked_edge_exists(ID)`     | Whether a tracked edge with the ID exists.                 |
| `junction_exists(ID)`         | Whether a junction with the ID exists.                     |
| `tracked_junction_exists(ID)` | Whether a tracked junction with the ID exists.             |

## Getting Values

### Object IDs

The most basic getter functions in the `Simulation` class return the IDs of objects within the simulation. These are:

|                Function                |                                      Return Value                                                           |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------|
| `get_vehicle_ids(vehicle_types)`       | All vehicle IDs, or those of specific type(s).                                                              |
| `get_detector_ids(detector_types)`     | All detector IDs, or those of specific type(s) - '_multientryexit_' or '_inductionloop_'.         |
| `get_controller_ids(controller_types)` | All controller IDs, or those of specific type(s) - '_VSLController_' or '_RGController_'.         |
| `get_event_ids(event_statuses)`        | All event IDs, or those of specific status(es) - '_scheduled_', '_active_' or '_completed_'. |
| `get_geometry_ids(geometry_types)`     | All controller IDs, or those of specific type(s) - '_edge_' or '_lane_'.                          |
| `get_tracked_edge_ids()`               | All tracked edge IDs.                                                                                       |
| `get_junction_ids()`                   | All junction IDs.                                                                                           |
| `get_tracked_junction_ids()`           | All tracked junction IDs.                                                                                   |

### Other Data

All data collected throughout the simulation is stored in the `sim_data` dictionary, however, its data (and other dynamic vehicle data) can be fetched using the `Simulation.get_[x]()` functions below.

  - `get_no_vehicles()`:
    - Returns the number of vehicles in the last step of the simulation.<br><br>
  - `get_no_waiting()`:
    - Returns the number of waiting vehicles in the last step of the simulation.<br><br>
  - `get_tts()`:
    - Returns the Total Time Spent (TTS) by vehicles in the simulation during the last step.<br><br>
  - `get_delay()`:
    - Returns total vehicle delay during the last simulation step (calculated as the number of vehicles where speed < 0.1m/s<sup>2</sup>, multiplied by the simulation step length).<br><br>
  - `get_interval_network_data(data_keys, n_steps, interval_end, get_avg)`:
    - Returns aggregated, network-wide vehicle data during the time range (`curr_step - n_steps - interval_end`, `curr_step - interval_end`). Supported data keys are '_no\_vehicles_', '_no_waiting_', '_tts_' and '_delay_'.
    - By default, all returned values are totalled over the interval. `get_avg` denotes whether to return the step average delay instead of the total.
    - If multiple data keys are given, each dataset is returned in a dictionary separated by its key.<br><br>
  - `get_vehicle_data(vehicle_ids)`:
    - Returns a dictionary containing all information on a vehicle. This is; '_type_', '_longitude_', '_latitude_', '_altitude_', '_heading_', '_speed_', '_acceleration_', '_stopped_', '_length_', '_departure_', '_destination_' and '_origin_'.
    - TUD-SUMO stores static information (route origin, vehicle length etc.) to avoid unnecessary repeated calls to TraCI. This is automatically done when calling `get_vehicle_data()` on a vehicle for the first time.
    - `vehicle_ids` can be a single vehicle ID (string), or a list of IDs (list/tuple). If multiple IDs are given, a dictionary is returned with each vehicle's data stored under its ID.<br><br>
  - `get_all_vehicle_data(vehicle_types, all_dynamic_data):`
    - Returns the total number of vehicles in the simulation, the total number of waiting vehicles, and if `all_dynamic_data == True`, the static & dynamic data for all vehicles.
    - If `all_dynamic_data == False`, an empty dictionary is returned as the last variable.<br><br>
  - `get_last_step_detector_vehicles(detector_ids, vehicle_types, flatten)`:
    - Returns the IDs of all vehicles who passed over the specified detector(s) in the last simulation step.
    - `detector_ids` and `vehicle_types` can either be a single value (string) or list of values (list/tuple). If multiple detector IDs are given, IDs for all detectors can either be returned in a single list (`flatten = True`), or the IDs can be returned in a dictionary with lists of IDs separated by detector (`flatten = False`).<br><br>
  - `get_last_step_geometry_vehicles(geometry_ids, vehicle_types, flatten)`:
    - Returns the IDs of all vehicles on the specified geometry in the last simulation step.
    - `geometry_ids` and `vehicle_types` can either be a single value (string) or list of values (list/tuple). If multiple geometry IDs are given, IDs for all geometries can either be returned in a single list (`flatten = True`), or the IDs can be returned in a dictionary with lists of IDs separated by geometry (`flatten = False`).<br><br>
- `get_interval_detector_data(detector_ids, data_keys, n_steps, interval_end, avg_step_vals, avg_det_vals, sum_counts)`:
    - Returns data collected by one detector or multiple detectors during the time range (`curr_step - n_steps - interval_end`, `curr_step - interval_end`).
    - If multiple detector IDs are given, data is returned in a dictionary separated by detector (and data keys). If `avg_det_vals == True`, data is averaged (step-wise) for all detectors (ie. `{"det_1": [1, 2, 3], "det_2": [3, 2, 1]}` is averaged to `[2, 2, 2]`).
    - If `avg_step_vals == True`, data is averaged across all steps (ie. `{"det_1": [1, 2, 3], "det_2": [3, 2, 1]}` is averaged to `{"det_1": 2, "det_2": 2}`).
    - If multiple detector IDs are given and `avg_step_vals == avg_det_vals == True`, a single averaged value is returned (for all data keys) (ie. `{"det_1": [1, 2, 3], "det_2": [3, 2, 1]}` is averaged to `2`).
    - `data_keys` can either be a single value (string) or a list of values (list/tuple). The valid keys are '_no\_vehicles_', '_no\_unique\_vehicles_', '_flow_', '_density_', '_speeds_' and '_occupancies_', although '_occupancies_' is only valid for induction loop detectors.
    - '_no\_vehicles_' returns the vehicle counts by the detector at each step, meaning vehicles may be counted multiple times across different steps. '_no\_unique\_vehicles_' instead returns the number of unique vehicles that passed over the detector during the specified interval, which is generally much lower than raw vehicle counts.
    - '_flow_' and '_density_' always return the averaged value, instead of values for each step in the interval. These are calculated using the number of unique vehicles and average speeds.
    - If multiple data keys are given, each dataset is returned in a dictionary separated by its key (and detectors), such as `{"det_1": {"speeds": ..., "flows": ...}, "det_2": ...}`.
    - `sum_counts` denotes whether to return the sum of vehicle counts ('_no\_vehicles_' and '_no\_unique\_vehicles_') throughout the interval instead of their average values. `sum_counts` will override `avg_step_vals`.

To query routes and paths in the network, use the functions below.

  - `is_valid_path(edge_ids)`:
    - Tests whether a list of edges is a valid path. This is determined by checking whether each edge connects to the subsequent edge in the list.
    - Valid paths can be added as a route using the `Simulation.add_route()` function.<br><br>
  - `get_path_travel_time(edge_ids, curr_tt, unit)`:
    - Calculate the travel time of a path (list of edges). If `curr_tt` is true, the travel time is calculated using current mean speed on edges. If `curr_tt` is false, the travel time is calculated using free-flow speed.<br><br>
  - `get_path_edges(origin, destination, curr_optimal)`:
    - Uses the A* algorithm to find the optimal route between two edges (`origin` to `destination`).
    - If `curr_optimal` is true, the route is based on current conditions, using current mean speed on edges to find travel time. If `curr_optimal` is false, the route is calculated based on free-flow travel time.

### Advanced Getters

Vehicles, detectors and geometries all use a `Simulation.get_[x]_vals()` function, which allow you to very easily get a set of values for multiple objects. All 3 use the same structure, with the same type of parameters:

  1. `vehicle_ids`/`detector_ids`/`geometry_ids`: A single ID (string) or list of IDs (list/tuple)
  2. `data_keys`: A single data key (string) or list of data keys (list/tuple)

If one data key is given, the raw value is returned, otherwise, a dictionary is returned containing the values separated by data key. Similarly, if one object ID is given, the raw data value/dictionary is returned, otherwise, a dictionary is returned containing the data separated by vehicle ID. For example:

```python
>>> my_sim.get_vehicle_vals("vehicle_1", "speed")
25

>>>  my_sim.get_vehicle_vals("vehicle_1", ("speed", "acceleration"))
{"speed": 25, "acceleration": -1}

>>>  my_sim.get_vehicle_vals(("vehicle_1", "vehicle_2"), "speed")
{"vehicle_1": 25, "vehicle_2": 30}

>>>  my_sim.get_vehicle_vals(("vehicle_1", "vehicle_2"), ("speed", "acceleration"))
{"vehicle_1": {"speed": 25, "acceleration": -1}, "vehicle_2": {"speed": 30, "acceleration": 2}}
```

Subscriptions and static vehicle data are used whenever possible to reduce TraCI calls. The valid data keys for each function are listed below.

  - `get_vehicle_vals()`:
    - '_type_': Vehicle type
    - '_length_': Vehicle length
    - '_speed_': Current vehicle speed
    - '_is_stopped_': Bool denoting whether the vehicle is stopped
    - '_max_speed_': Vehicle maximum speed
    - '_acceleration_': Current vehicle acceleration
    - '_position_': Current vehicle coordinates
    - '_altitude_': Current vehicle altitude
    - '_heading_': Current vehicle heading
    - '_departure_': Vehicle departure time
    - '_edge_id_': Current vehicle's edge ID
    - '_lane_id_': Current vehicle's lane ID
    - '_lane_idx_': Index of the vehicle's current lane
    - '_origin_': Departure edge ID of the vehicle
    - '_destination_': Current destination edge ID of the vehicle
    - '_route_id_': Current vehicle route ID
    - '_route_idx_': The index of the vehicle's edge on its route
    - '_route_edges_': The list of edges that the vehicle's route consists of
    - '_allowed_speed_': Current vehicle's maximum allowed speed, based on its location & type
    - '_leader_id_': Returns the ID of the leading vehicle (returns `None` if no leading vehicle within 100m)
    - '_leader_dist_': Returns the distance to the leading vehicle (returns `None` if no leading vehicle within 100m)
  - `get_detector_vals()`:
    - '_type_': Detector type ('mutlientryexit' or 'inductionloop')
    - '_position_': Detector coordinates
    - '_vehicle_count_': Number of vehicles that passed over the detector in the last step
    - '_vehicle_ids_': IDs of vehicles that passed over the detector in the last step
    - '_lsm_speed_': Average speed of vehicles that passed over the detector in the last step
    - '_halting_no_': Number of halting vehicles in the detector area _(multi-entry-exit only)_
    - '_lsm_occupancy_': Average occupancy during the last step _(induction loop only)_
    - '_last_detection_': Time since last detection _(induction loop only)_
    - '_avg_vehicle_length_': Average length of vehicles that passed over the detector in the last step _(induction loop only)_
  - `get_geometry_vals()`:
    - '_vehicle_count_': Number of vehicles on the edge/lane
    - '_vehicle_ids_': IDs of vehicles on the edge/lane
    - '_vehicle_speed_': Average speed of vehicles on the edge/lane
    - '_avg_vehicle_length_': Average length of vehicles on the edge/lane in the last step.
    - '_halting_no_': Number of halting vehicles on the edge/lane
    - '_vehicle_occupancy_': Vehicle occupancy of edge/lane
    - '_curr_travel_time_': Estimated travel time (calculated using length and average speed)
    - '_ff_travel_time_': Estimated free-flow travel time (calculated using length and maximum speed)
    - '_emissions_': CO, CO<sub>2</sub>, HC, PMx and NOx emissions, stored in a dictionary
    - '_length_': Length of the edge/lane
    - '_max_speed_': Maxmimum speed (edge max_speed is the average of its lanes)
    - '_connected_edges_': Dictionary containing 'incoming' and 'outgoing' edges _(edge only)_
    - '_incoming_edges_': Incoming edges _(edge only)_
    - '_outgoing_edges_': Outgoing edges _(edge only)_
    - '_street_name_': Street name, defined in SUMO _(edge only)_
    - '_n_lanes_': Number of lanes in the edge _(edge only)_
    - '_lane_ids_': IDs of all lanes in the egde _(edge only)_
    - '_edge_id_': Edge ID for the lane _(lane only)_
    - '_n_links_': Number of linked lanes _(lane only)_
    - '_allowed_': List containing allowed vehicle types _(lane only)_
    - '_disallowed_': List containing all prohibited vehicle types _(lane only)_
    - '_left_lc_': Bool denoting whether left lane changes are allowed _(lane only)_
    - '_right_lc_': Bool denoting whether right lane changes are allowed _(lane only)_

## Setting Values

Both vehicle and geometries (edges/lanes) also allow for dynamically setting variables with the `Simulation.set_vehicle_vals()` and `Simulation.set_geometry_vals()` functions. Both use the same types of parameters:

  1. `vehicle_ids`/`geometry_ids`: A single ID (string) or list of IDs (list/tuple). If multiple IDs are given, the values are set for all objects.
  2. `data_keys`: Unlike the `get_[x]_vals()` functions, data_keys are used as arguments. There is no limit on the number of data keys given.

For example:

```python
>>> my_sim.set_geometry_vals("edge_1", max_speed=40)
>>> my_sim.set_vehicle_vals(("vehicle_1", "vehicle_2"), max_speed=20, highlight="#FF0000")
```

The valid data keys and their accepted data type are listed below. Note that the keys aim to be the same as those in the `get_[x]_vals()` functions (whenever possible).

  - `set_vehicle_vals()`:
    - `type`: Changes the vehicle's type to another pre-existing type (string)
    - `colour`: Changes the vehicle's colour (either valid hex code or list of rgb(a) values)
    - `highlight`: Sets vehicle highlighting (boolean)
    - `speed`: Sets a new speed value for the vehicle (integer or float)
    - `max_speed`: Sets a new maximum speed for the vehicle (integer or float)
    - `acceleration`: Sets a new acceleration for a given duration (list/tuple containing speed value and duration)
    - `lane_idx`: Sets a vehicle to try and change lane index for a given duration (list/tuple containing lane index and duration)
    - `destination`: Sets a new destination by edge ID (string)
    - `route_id`: Sets the vehicle to another route by its ID (string)
    - `route_edges`: Sets the vehicle to a new route by edge IDs (list of strings)
    - `speed_safety_checks`: Indefinitely sets whether speed/acceleration safety constraints are followed when setting speed - can be boolean to turn on/off all checks or [bitset](https://sumo.dlr.de/docs/TraCI/Change_Vehicle_State.html#lane_change_mode_0xb6) (boolean/int)
    - `lc_safety_checks`: Indefinitely sets whether lane changing safety constraints are followed when changing lane - can be boolean to turn on/off all checks or [bitset](https://sumo.dlr.de/docs/TraCI/Change_Vehicle_State.html#speed_mode_0xb3) (boolean/int)
  - `set_geometry_vals()`:
    - `max_speed`: Set a new maximum speed/speed limit (lane or for all contained lanes) (integer or float)
    - `allowed`: Set a new list of allowed vehicle types, with an empty list allowing all (list of strings) _(lane only)_
    - `disallowed`: Set a new list of prohibited vehicle types (list of strings) _(lane only)_
    - `left_lc`: Sets the list of vehicle types that are allowed to change to the left lane (list of strings) _(lane only)_
    - `right_lc`: Sets the list of vehicle types that are allowed to change to the right lane (list of strings) _(lane only)_

## Subscriptions

Subscriptions are a useful tool in TraCI that aim to reduce runtime by limiting the amount of API calls. This can be particularly effective when data needs to be collected at each simulation step. TUD-SUMO automatically adds vehicle subscriptions for basic variables including position, speed and acceleration as these are already used within the automatic data collection and are necessary for calculating vehicle delay at each step. Tracked edges will also automatically subcribe to vehicle IDs, as this is needed to collect step vehicle data.

It is possible to disable automatic vehicle subscriptions by setting `automatic_subscriptions = False` in `Simulation.start()`. However, this is most likely unncessary and not recommened, particularly for applications where a lot of data needs to be repeatedly collected from the simulation.

Otherwise, depending on the use case, it may be necessary to subscribe/unsubscribe to other variables. This can be done using the functions below. These use the same structure as the `get_[x]_vals()` functions with two parameters:

  1. `vehicle_ids`/`detector_ids`/`geometry_ids`: A single ID (string) or list of IDs (list/tuple). Note that individual objects have their own set of subscriptions.
  2. `data_key`: A single data key (string) or list of data keys (list/tuple).

The valid data keys for each object type and respective function are listed below. Note that individual objects have their own set of subscriptions, so if you would like to subscribe to collect all vehicles' edge ID at each step, you must call `add_vehicle_subscriptions()` for all vehicle IDs.

  - `add_vehicle_subscriptions()`/`remove_vehicle_subscriptions()`:
    - '_speed_'
    - '_is_stopped_'
    - '_max_speed_'
    - '_acceleration_'
    - '_position_'
    - '_altitude_'
    - '_heading_'
    - '_edge_id_'
    - '_lane_idx_'
    - '_route_id_'
    - '_route_idx_'

  - `add_detector_subscriptions()`/`remove_detector_subscriptions()`:
    - '_vehicle_count_'
    - '_vehicle_ids_'
    - '_lsm_speed_'
    - '_halting_no_'
    - '_lsm_speed_'
    - '_last_detection_'

  - `add_geometry_subscriptions()`/`remove_geometry_subscriptions()`:
    - '_vehicle_count_'
    - '_vehicle_ids_'
    - '_vehicle_speed_'
    - '_halting_no_'
    - '_occupancy_'

## Adding/removing Vehicles

Vehicles can be added to or removed from the simulation using the `Simulation.add_vehicle()` and `Simulation.remove_vehicles()` functions respectively. These functions and their parameters are detailed below.

  - `add_vehicle()`:
    - `vehicle_id`: (Unique) ID for the new vehicle.
    - `vehicle_type`: Vehicle type.
    - `routing`: Denotes how the vehicle will route through the network (either route ID or (2x1) list of edge IDs for an OD pair).
    - `initial_speed`: Initial speed of the vehicle at insertion, defaults to maximum (either '_max_', '_random_' or a number > 0).
    - `origin_lane`: Lane for insertion at origin, defaults to best (either '_random_', '_free_', '_allowed_', '_best_', '_first_' or lane index).
  - `remove_vehicles()`:
    - `vehicle_ids`: A single vehicle ID (string) or list of IDs (list/tuple).

!!! Warning

    Removing vehicles causes a SUMO error to be printed to the console, an example of which is shown below. This has no impacts on the simulation and can be safely ignored. 

    ```
    Error: Answered with error to command 0xa4: Vehicle 'vehicle_0' is not known.
    Error! Vehicle 'vehicle_0' is not known.
    ```


When repeatedly adding new vehicles, it may be useful to create a new route that vehicles can be assigned to. This can be done using `Simulation.add_route()`, which is detailed below.

  - `add_route()`:
    - `routing`: List of edge IDs. If 2 disconnected edges are given, vehicles calculate an optimal path at insertion.
    - `route_id`: (Unique) route ID. If this is not given, the ID is generated using the origin and destination edge IDs.
    - `assert_new_id`: If true, an error is thrown for duplicate route IDs.

## Custom Vehicle Functions

`Simulation.add_vehicle_in_functions()` and `Simulation.add_vehicle_out_functions()` can be used to easily add custom functions that are called with each vehicle that enters or leaves the simulation. An example of this is shown below. Multiple functions can be added and are called in the order in which they are added.

If certain parameters are included, their values will be automatically generated with each vehicle. These are:

  - `simulation`: The simulation object itself.
  - `curr_step`: The current simulation step.
  - `vehicle_id`: Each vehicle ID.
  - `route_id`: Route the vehicle will travel on.
  - `vehicle_type`: Vehicle type ID.
  - `departure`: Departure time of the vehicle.
  - `origin`: Origin egde ID of the vehicle.
  - `destination`: Destination edge ID of the vehicle.

```python
def recolour_vehicle(simulation, vehicle_id, new_colour="#FF0000"):
    simulation.set_vehicle_vals(vehicle_id, colour=new_colour)

new_vehicles = []
def register_vehicle(vehicle_id, curr_step, arr):
    print(vehicle_id, "entered simulation at step", curr_step)
    arr.append(vehicle_id)

def exit_vehicle(vehicle_id, curr_step):
    print(vehicle_id, "left simulation at step", curr_step)

my_sim.add_vehicle_in_functions(functions=[recolour_vehicle, register_vehicle],
                                parameters={"register_vehicle": {"arr": new_vehicles}})
my_sim.add_vehicle_out_functions(exit_vehicle)

while my_sim.is_running():
    my_sim.step_through()
```

Running this example would print the message below to the console. The `arr` list would also contain `['car_1', 'car_2', 'car_3']`.

```
>>> run_sim.py
car_1 entered simulation at step 0
car_2 entered simulation at step 2
car_3 entered simulation at step 4
car_1 left simulation at step 10
car_2 left simulation at step 12
car_2 left simulation at step 14
```

Only `simulation`, `curr_step` and `vehicle_id` can be used for both vehicle in **and** out functions. Trip-related variables cannot be used in vehicle out functions. Extra parameters can be used by passing in a parameters dictionary as below. These can later be updated using `Simulation.update_vehicle_function_parameters()`. Note that it is not necessary to include a `new_colour` parameter in the dictionary, as this already has a default value.

These functions can be removed using `Simulation.remove_vehicle_in_functions()` and `Simulation.remove_vehicle_out_functions()`.