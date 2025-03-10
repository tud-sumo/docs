# GUI & Videos

## GUI Functions

Several functions are provided specifically for controlling the Graphical User Interface (GUI). More general information on using the SUMO GUI can be found at [sumo.dlr.de/docs/sumo-gui.html](https://sumo.dlr.de/docs/sumo-gui.html). In order to run a simulation with the GUI active using TUD-SUMO, use:

```python
from tud_sumo.simulation import Simulation

my_sim = Simulation(scenario_name="example", scenario_desc="Example simulation.")
my_sim.start("example_scenario.sumocfg", gui=True)
```

The 4 functions used for interacting with the SUMO GUI are listed below. The GUI itself can have multiple different views, or windows, but the default/main view is called '_View #0_'.

  - `set_view()`:
    - Sets a view's bounds and/or zoom level.
    - Bounds use a tuple of two coordinates to define the new view, as in (lower-left point, upper-right point).<br><br>
  - `take_screenshot()`:
    - Takes a screenshot of a view in the next time step and saves the result to a file.
    - If the bounds and/or zoom is not specified, the current view is used.<br><br>
  - `gui_track_vehicle()`:
    - Sets a view to follow a vehicle through the network, either until it leaves the network or the tracking is manually stopped.
    - The vehicle is tracked by its ID, and can be highlighted if `highlight` is set to `True`.
    - By default, the main/default view is used. An example of vehicle tracking is shown below.<br><br>
  - `gui_stop_tracking()`:
    - Stops a view from tracking a vehicle.<br><br>

![Tracked vehicle](img/track_vehicle.gif)

```python
# Define view by its ID
default_view = "View #0"

while my_sim.curr_step < 500:

    my_sim.step_through()

    if my_sim.curr_step == 100:
        # Set the view boundaries to (min_x: 0, max_x: 200, min_y: 0, max_y: 200) and zoom to 500
        my_sim.set_view(view_id=default_view, bounds=((0, 0), (200, 200)), zoom=500)

        # Take a screenshot of the current view settings and save it to "images/screenshot.png"
        my_sim.take_screenshot(view_id=default_view, "images/screenshot.png")

    if my_sim.curr_step == 200:
        # Set the default view to follow "car_0" as it travels through the network
        my_sim.gui_track_vehicle(vehicle_id="car_0", view_id=default_view)

    if my_sim.curr_step == 300:
        # Set the default view to stop following "car_0"
        my_sim.gui_stop_tracking(view_id=default_view)
```

