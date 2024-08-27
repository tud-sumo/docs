# Welcome to the TUD-SUMO Wiki!

![logos](img/header.png)

<p style="text-align: center;">
  <a href="https://pypi.org/project/tud-sumo/">
    <img alt="PyPI" src="https://img.shields.io/pypi/v/tud-sumo?style=for-the-badge&logo=pypi&logoColor=%23FFFFFF&color=%2300A6D6"/>
  </a>
  <a href="https://github.com/tud-sumo/tud_sumo">
    <img alt="GitHub Repo" src="https://img.shields.io/badge/GitHub-%2338A6D6?style=for-the-badge&logo=github&link=https%3A%2F%2Fgithub.com%2Ftud-sumo%2Ftud_sumo"/>
  </a>
  <a href="https://github.com/tud-sumo/tud_sumo/blob/main/LICENSE">
      <img alt="PyPI - License" src="https://img.shields.io/pypi/l/tud-sumo?style=for-the-badge&color=%2300A6D6">
  </a>
  <br>
  
  <img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/tud-sumo/tud_sumo?style=for-the-badge&logo=github&color=%2300A6D6"/>

  <a href="https://github.com/tud-sumo/tud_sumo/commits/main/">
    <img alt="GitHub commit activity" src="https://img.shields.io/github/commit-activity/m/tud-sumo/tud_sumo?style=for-the-badge&logo=github&label=Commits&color=%2300A6D6"/>
  </a>
</p>

This is the documentation for the TUD-SUMO package, a research-oriented wrapper for SUMO<sup>[1]</sup>, developed for the DAIMoND lab at the Technische Universiteit Delft (TUD), the Netherlands.

The main goal of TUD-SUMO is to act as a simplified framework for microscopic traffic simulation that allows researchers and students to focus on the important aspects of their projects; **their own work**, instead of simulation code. TUD-SUMO provides an easy and standardised way to simulate a wide range of scenarios whilst facilitating complex interactions. Resulting data can then be saved, summarised and visualised with minimal code.

More information on "Simulation of Urban MObility" (SUMO) can be found in the SUMO documentation, here: [sumo.dlr.de/docs/](https://sumo.dlr.de/docs/)

The main features of TUD-SUMO include:

  - Automatic and standardised data collection.
  - Simple interface to interact with and control the simulation.
  - Traffic signal control logic.
  - Extendable controllers already implemented (ramp metering, route guidance and variable speed limits).
  - An event system with dynamic or scheduled incidents.
  - Plotting functions for a wide range of applications.
  - And _more in the future!_

![logos](img/logos.png)

## Links

1. Simulation of Urban MObility (SUMO) documentation: [sumo.dlr.de/docs/](https://sumo.dlr.de/docs/)
2. TUD-SUMO source code: [github.com/tud-sumo/tud_sumo](https://github.com/tud-sumo/tud_sumo/)
3. TUD-SUMO PyPI distribution: [pypi.org/project/tud-sumo/](https://pypi.org/project/tud-sumo/)
4. TUD-SUMO example: [github.com/tud-sumo/example](https://github.com/tud-sumo/example)

## Latest Version

The Latest version of TUD-SUMO is _v3.0.9_, and was released on 21/08/2024. All previous versions and their change notes can be found on [GitHub](https://github.com/tud-sumo/tud_sumo/releases) or [PyPI](https://pypi.org/project/tud-sumo/#history).

The change notes for the latest version are:

### Interval Detector Data Improvements

#### Changes & Improvements

  - Improved `Simulation.get_interval_detector_data()` (added 'flows', 'densities', `unique_count` and `interval_end` and allowing for multiple detectors).
  - Improved README to match documentation.
  - Setting metering rate multiple times in the same step now overwrites the rate (only one value per time step).
  - Added incoming/outgoing edges to tracked junctions.
  - Added missing `__str__` functions.

#### Bug Fixes

  - Fixed index error in `Plotter.plot_throughput()`.
  - Removed occupancy showing as an empty list in sim_data for multi-entry-exit detectors.
  - Corrected spelling in README.

## Contact

TUD-SUMO is developed in the DAIMoND lab of TU Delft. For any questions or feedback, please contact Callum Evans at <span class="highlight">_c.evans@tudelft.nl_</span>. Bug reports can be created in the GitHub repository: [github.com/tud-sumo/tud_sumo](https://github.com/tud-sumo/tud_sumo/).

## Acknowledgements

TUD-SUMO is part of the research under the project "_AI in Network Management_," funded by Rijkswaterstaat, grant agreement nr. 31179439, under the label of ITS Edulab.

## Citations

  1. "_Microscopic Traffic Simulation using SUMO_"; Pablo Alvarez Lopez, Michael Behrisch, Laura Bieker-Walz, Jakob Erdmann, Yun-Pang Flötteröd, Robert Hilbrich, Leonhard Lücken, Johannes Rummel, Peter Wagner, and Evamarie Wießner. _IEEE Intelligent Transportation Systems Conference (ITSC)_, 2018.