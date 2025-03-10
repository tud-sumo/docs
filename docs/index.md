# Welcome to the TUD-SUMO Wiki!

![logos](img/header.png)

<p align="center">
  <a href="https://pypi.org/project/tud-sumo/">
    <img alt="PyPI" src="https://img.shields.io/pypi/v/tud-sumo?style=for-the-badge&logo=pypi&logoColor=%23FFFFFF&color=%2300A6D6"/>
  </a>
  <a href="https://github.com/DAIMoNDLab/tud-sumo">
    <img alt="GitHub Repo" src="https://img.shields.io/badge/GitHub-%2338A6D6?style=for-the-badge&logo=github&link=https%3A%2F%2Fgithub.com%2FDAIMoNDLab%2Ftud_sumo"/>
  </a>
  <a href="https://github.com/DAIMoNDLab/tud-sumo/blob/main/LICENSE">
      <img alt="PyPI - License" src="https://img.shields.io/pypi/l/tud-sumo?style=for-the-badge&color=%2300A6D6">
  </a>
</p>
<p align="center">
  <img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/DAIMoNDLab/tud-sumo?style=for-the-badge&logo=github&color=%2300A6D6"/>
  <a href="https://github.com/DAIMoNDLab/tud-sumo/commits/main/">
    <img alt="GitHub commit activity" src="https://img.shields.io/github/commit-activity/m/DAIMoNDLab/tud-sumo?style=for-the-badge&logo=github&label=Commits&color=%2300A6D6"/>
  </a>
</p>

This is the documentation for the TUD-SUMO package, a research-oriented wrapper for SUMO<sup>[1]</sup>, developed for the DAIMoND lab at the Technische Universiteit Delft (TUD), the Netherlands. 

The main goal of TUD-SUMO is to act as a simplified framework for microscopic traffic simulation that allows researchers and students to focus on the important aspects of their projects; **their own work**, instead of simulation code. TUD-SUMO provides an easy and standardised way to simulate a wide range of scenarios whilst facilitating complex interactions. Resulting data can then be saved, summarised and visualised with minimal code.

More information on "Simulation of Urban MObility" (SUMO) can be found in the SUMO documentation, here: [sumo.dlr.de/docs/](https://sumo.dlr.de/docs/)

The main features of TUD-SUMO include:

  - Automatic and standardised data collection.
  - Simplified interface to interact with and control the simulation in complex ways.
  - Traffic signal control logic.
  - Extendable controllers already implemented (ramp metering, route guidance and variable speed limits).
  - An event system with dynamic or scheduled incidents.
  - Plotting functions for a wide range of applications.
  - And _more in the future! ..._

![logos](img/logos.png)

## Links

1. Simulation of Urban MObility (SUMO) documentation: [sumo.dlr.de/docs](https://sumo.dlr.de/docs/)
2. TUD-SUMO source code (GitHub): [DAIMoNDLab/tud-sumo](https://github.com/DAIMoNDLab/tud-sumo)
3. TUD-SUMO PyPI distribution: [project/tud-sumo](https://pypi.org/project/tud-sumo/)
4. TUD-SUMO example (GitHub): [DAIMoNDLab/tud-sumo-examples](https://github.com/DAIMoNDLab/tud-sumo-examples)

## Latest Version

The Latest version of TUD-SUMO is _v3.2.1_, and was released on 10/03/2025. All previous versions and their change notes can be found on [GitHub](https://github.com/DAIMoNDLab/tud-sumo/releases) or [PyPI](https://pypi.org/project/tud-sumo/#history). This documentation was last updated on {{ git.date.strftime("%d/%m/%Y") }}.

The change notes for this latest version, and _v3.2.0_ (released on 28/02/2025) are:

### Videos, Delays & Improvements

#### Additions

  - Added `Recorder` class & completed video recording features.
  - Added `Simulation.set_view()` and `Simulation.gui_stop_tracking()` functions.
  - Added `MultiPlotter.plot_statistics()` to plot final simulation statistics (TTS, TWT, delay).
  - Added TWT and delay data to `Plotter.plot_vehicle_data()`.
  - Added `MultiPlotter.plot_rm_rate()` to plot the average metering rate across simulations.
  - Added `plot_groups` to `MultiPlotter` functions to only plot the data of specific groups.
  - Added linestring coordinates to `Simulation.get_geometry_vals()`.
  - Added `"lane_id"`, `"allowed_speed"`, `"leader_id"` and `"leader_dist"` to `Simulation.get_vehicle_vals()`.
  - Added tracking of vehicles waiting to be inserted into the simulation.
  - Added `"type"` to `Simulation.set_vehicle_vals()`.

#### Changes & Improvements
  - Improved delay calculation to be based on the current vehicle speed and ideal lane free-flow speed.
  - Renamed previous delay data to Total Waiting Time (TWT).
  - Added changing line styles into colour wheel for `Plotter` and `MultiPlotter`.
  - Reduced sumolib calls for fetching network information.
  - Updated docstrings (replaced '_None_' with '_optional_').
  - Optimised `Simulation.get_vehicle_vals()` when fetching values that rely on the same API calls.
  - Overhauled setup for TraCI constants/getter/setters with keys.
  - Made units easier to handle throughout.
  - Added MoviePy as a package requirement.

#### Fixes
  - Fixed AttributeError in `Simulation._add_v_func()`.
  - Fixed `print_summary()` error when not saving the summary to a '_.txt_' file.

## Contact

TUD-SUMO is developed in the DAIMoND lab of TU Delft. For any questions or feedback, please contact Callum Evans at <span class="highlight">_c.evans@tudelft.nl_</span>. Bug reports can be created in the GitHub repository: [github.com/DAIMoNDLab/tud-sumo](https://github.com/DAIMoNDLab/tud-sumo/).

## Acknowledgements

TUD-SUMO is part of the research under the project "_AI in Network Management_," funded by Rijkswaterstaat, grant agreement nr. 31179439, under the label of ITS Edulab.

## Citations

  1. "_Microscopic Traffic Simulation using SUMO_"; Pablo Alvarez Lopez, Michael Behrisch, Laura Bieker-Walz, Jakob Erdmann, Yun-Pang Flötteröd, Robert Hilbrich, Leonhard Lücken, Johannes Rummel, Peter Wagner, and Evamarie Wießner. _IEEE Intelligent Transportation Systems Conference (ITSC)_, 2018.