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

The Latest version of TUD-SUMO is _v3.2.3_, and was released on 30/04/2025. All previous versions and their change notes can be found on [GitHub](https://github.com/DAIMoNDLab/tud-sumo/releases) or [PyPI](https://pypi.org/project/tud-sumo/#history). This documentation was last updated on {{ git.date.strftime("%d/%m/%Y") }}.

The most recent change notes are:

### Demand Profiles & Plotting Improvements

#### Additions

  - Added `DemandProfile` class.
  - Added ability to save profiles with `DemandProfiles.save()` and added to `Simulation.save_objects()`.
  - Added ability to load profiles with `Simulation.load_demand_profiles()` and `Simulation.load_objects()`.
  - Added `Simulation.gui_is_tracking()` to return whether a GUI view is tracking a vehicle.
  - Added `Simulation.get_[twt|to_depart]()` functions.
  - Added `"twt"` and `"to_depart"` to `Simulation.get_interval_network_data()`.
  - Added several view functions (`[add|remove]_gui_view()`, `get_gui_views()`, `get_view_[boundaries|zoom]()`)
  - Added verbose option to Simulation initialisation.
  - Added ability to plot regression line on fundamental diagrams.
  - Added ability to plot labels at specific distances along trajectory diagrams and space-time diagrams.
  - Added `MultiPlotter.plot_rm_queue_length()`

#### Changes & Improvements
  - Added version to simulation start/data.
  - Changed `recording_name` to `recording_id`.
  - Can now create mp4/avi/gif files.
  - Allowed for `video_filename` to be defined separately to `recording_id` in `Recorder.record_[network|vehicle]()`.
  - Changed all GUI functions to use the main view ("View #0") by default.
  - `Simulation.gui_stop_tracking()` will now throw an error if the view is not tracking a vehicle.
  - Removed traCI calls from `Recorder` class.
  - Changed `Recorder.save_recording()` to only save single recordings (not by a list of IDs).
  - Replaced `Plotter.plot_od_demand()` with updated `Plotter.plot_demand()` that works with `DemandProfile` objects.

## Contact

TUD-SUMO is developed in the DAIMoND lab of TU Delft. For any questions or feedback, please contact Callum Evans at <span class="highlight">_c.evans@tudelft.nl_</span>. Bug reports can be created in the GitHub repository: [github.com/DAIMoNDLab/tud-sumo](https://github.com/DAIMoNDLab/tud-sumo/).

## Acknowledgements

TUD-SUMO is part of the research under the project "_AI in Network Management_," funded by Rijkswaterstaat, grant agreement nr. 31179439, under the label of ITS Edulab.

## Citations

  1. "_Microscopic Traffic Simulation using SUMO_"; Pablo Alvarez Lopez, Michael Behrisch, Laura Bieker-Walz, Jakob Erdmann, Yun-Pang Flötteröd, Robert Hilbrich, Leonhard Lücken, Johannes Rummel, Peter Wagner, and Evamarie Wießner. _IEEE Intelligent Transportation Systems Conference (ITSC)_, 2018.