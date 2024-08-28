# Tracked Objects

Tracked objects are a useful way to include extra objects in the automatic data collection. Whilst all data from every detector is automatically included, specific scenarios may contain a large number of edges or junctions. Therefore, important edges or junctions can be tracked to automatically collect more specific information, such as inflow or average speeds. Tracked objects are also supported within the [plotter](8_plotting.md) class, so that phase signal diagrams or space-time diagrams can easily be plotted with significantly less effort.

Currently, only edges and junctions can be tracked, although this may change in the future.

## Tracked Junctions

There are two types of tracked junctions; regular junctions and metered junctions. Most intersections will only need to be tracked as a regular junction, whilst metered junctions are a subclass of junction that include some extra tracking for motorway on-ramp metering.

!!! tip
    Note that traffic light/ramp meter control itself is not discussed here. **An adaptive traffic signal or ramp meter does not have to be tracked** and not all tracked junctions require some control. Tracking a junction with an adaptive traffic signal or ramp meter simply adds a level of data collection, however, **it is recommended** and makes evaluation of the controllers much easier.

All parameters to create a tracked junction are given as a dictionary and is done using the `Simulation.add_tracked_junctions()` function. This can be in code, or from a '_.json_' or '_.pkl_' file. Tracked junctions can also be included in the object parameters file when calling `Simulation.load_objects()`, under '_junctions_'. A tracked junction can be initialised at 3 different levels, depending on the amount of data collection required.

  1. Signal tracking only:
    - This is the lowest level of tracking, where only signal phases and average green/red times are tracked. Therefore, this level is only useful when the specified junction has a traffic light.
    - In this case, the only required parameters are the junction ID or a list of junction IDs. It is recommended to give junctions and their traffic lights the same ID in SUMO in order to avoid confusion.

  2. Inflow/outflow tracking:
    - Traffic-flow related variables can be tracked by tracked junctions, however, it is necessary to define the detectors used to do so.
    - Here, the input for `Simulation.add_tracked_junctions()` is a dictionary, where each junction has a '_flow_params_' value. This is a dictionary containing '_inflow_detectors_', '_outflow_detectors_' and '_vehicle_types_'. '_inflow_detectors_' and '_outflow_detectors_' are lists containing the detectors that will be used to register vehicles entering and exiting the junction, whilst '_vehicle_types_' is an optional parameter that lists the vehicle types that should be registered (defaults to all).

  3. Metered junctions:
    - Metered junctions adds options for tracking queuing variables; queue length, queue delay and spillback. This is primarily designed for use in ramp metering systems.
    - Here, the input for `Simulation.add_tracked_junctions()` is a dictionary, where each junction has a 'meter_params' value. This is a dictionary containing '_min_rate_', '_max_rate_', '_init_rate_', '_queue_detector_' and '_ramp_edges_'. These are detailed below, however, flow can still be tracked as above.
    - '_min_rate_' is a required parameter denoting the minimum allowed metering rate.
    - '_max_rate_' is a required parameter denoting the maximum allowed metering rate.
    - '_init_rate_' denotes the initial metering rate set after initialisation.
    - '_queue_detector_' is the ID of a **multi-entry-exit detector** that should be placed on the on-ramp. The entry detector should be placed at the start of the on-ramp, and the exit detector just before the traffic light. If given, queue length and delay at each step is tracked automatically.
    - If you would like to track vehicle spillback, provide the list of edge IDs that make up the ramp under the '_ramp_edges_' parameter. This can also be used instead of '_queue_detector_' to track queue length and delay, however, this approach is much slower. Tracking spillback in this way will only work when vehicles are inserted directly onto the on-ramp (ie. no urban roads connect to the on-ramp), and requires checking all unloaded vehicles of their insertion edge which can be slow.

Examples of these different levels are shown below.

```python
# Level 1: Only signal phases & green/red times are tracked
my_sim.add_tracked_junctions(["traffic_signal_1", "traffic_signal_2"])

# Level 2: Track inflow/outflow (& signal phases & green/red times)
my_sim.add_tracked_junctions({"traffic_signal_3":
                                {"flow_params":
                                    {"inflow_detectors":  ["det_in_1", "det_in_2"],
                                     "outflow_detectors": ["det_out_1", "det_out_2"],
                                     "vehicle_types": ["cars", "lorries", "motorcycles", "vans"]}
                                }
                            })

# Level 3: Track metering (& inflow/outflow & signal phases & green/red times)
my_sim.add_tracked_junctions({"ramp_meter":
                                {"meter_params":
                                    {"min_rate": 200,
                                    "max_rate": 2000,
                                    "queue_detector": "ramp_queue"},
                                "flow_params":
                                    {"inflow_detectors": ["ramp_inflow", "ramp_upstream"],
                                    "outflow_detectors": ["ramp_downstream"]}
                                }
                            })
```

All data from tracked junctions is stored in the `sim_data` dictionary under '_data/junctions/{junction_id}_'. This will contain the junction's '_position_', '_incoming_edges_', '_outgoing_edges_', '_init_time_' and '_curr_time_' (referring to the start and end time of the data collection). Phase data is then stored under '_tl_', whilst flow and metering data are under '_flow_' and '_meter_' respectively.

Data collection for tracked junctions can be reset using the function below, however, data collection is also reset when using `Simulation.reset_data()`.

```python
my_sim.tracked_junctions["traffic_signal_1"].reset()
```

## Tracked Edges

Tracked edges are very simple to initialise, but include a lot of useful data collection. Edges are tracked using only their ID with the `Simulation.add_tracked_edges()` function.

```python
# Add all edges as a tracked edge
my_sim.add_tracked_edges()

# Add a list of edges as tracked edges
my_sim.add_tracked_edges(["edge_1", "edge_2", "edge_3", "edge_4", "edge_5"])
```

All data from tracked edges is stored in the `sim_data` dictionary under '_data/edges/{edge_id}_'. The main data collected is under '_data/edges/{edge_id}/step_vehicles_'. This includes the vehicle ID, position, speed and lane index for all vehicles on the edge at each step. This precise data is used to plot trajectories and space-time diagrams.

The step vehicle data is stored in a (4x1) array as follows:

  1. Vehicle ID
  2. Vehicle position measured as [0 - 1], denoting percent travelled along the edge
  3. Vehicle speed in `Simulation` class units
  4. Lane index [0 - no. lanes]

Tracked edges collect average speed, flow and density at each time step and store these values in a list. Funamental diagrams can be plotted with `TrackedEdge` data using the `Plotter.plot_fundamental_diagram()` function, but it is possible to return the individual values. This data is stored in the `sim_data` dictionary under '_data/edges/{edge_id}/speeds_', '_data/edges/{edge_id}/flows_' and '_data/edges/{edge_id}/densities_', but can be fetched from the `TrackedEdge` object as below. A value of -1 denotes that there were no vehicles on the edge during the step. Time occupancy is also collected and stored under '_data/edges/{edge_id}/occupancies_'.

```python
speeds    = my_sim.tracked_edges["edge_1"].speeds
flows     = my_sim.tracked_edges["edge_1"].flows
densities = my_sim.tracked_edges["edge_1"].densities
```

'_linestring_', '_length_', '_to_node_', '_from_node_', '_n_lanes_', '_init_time_' and '_curr_time_' are also stored for each edge.

Data collection for specific tracked edges can be reset using the function below, however, data collection is also reset when using `Simulation.reset_data()`.

```python
my_sim.tracked_edges["edge_1"].reset()
```