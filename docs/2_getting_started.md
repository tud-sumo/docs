# Getting Started

## Requirements

<a href="https://www.python.org/downloads/">
  <img alt="PyPI - Python Version" src="https://img.shields.io/pypi/pyversions/tud-sumo?style=for-the-badge&logo=python&logoColor=white&color=%2300A6D6">
</a>

To run TUD-SUMO, first install SUMO. Platform-specific information on how to do this can be found in the SUMO documentation: [sumo.dlr.de/docs/Installing/](https://sumo.dlr.de/docs/Installing/)

Once SUMO has been installed, ensure that the `SUMO_HOME` variable has been set in your environment. This should be set to the base directory of your SUMO installation. Information on how to do this can be found here: [sumo.dlr.de/docs/Basics/Basic_Computer_Skills.html](https://sumo.dlr.de/docs/Basics/Basic_Computer_Skills.html#configuring_path_settings)

Python 3.10 or later is required to run TUD-SUMO, otherwise, the required dependencies are:

  - `traci`
  - `sumolib`
  - `matplotlib`
  - `mpl-tools`
  - `shapely`
  - `requests`
  - `tqdm`
  - `moviepy`

## Creating an Environment

Although not required, it is recommended to use install the required packages in an environment, such as using [Anaconda](https://docs.anaconda.com/anaconda/install/) or [Miniconda](https://docs.anaconda.com/miniconda/). A conda environment ready for TUD-SUMO can be created using the following commands.

```
conda create --name tud-sumo
conda activate tud-sumo
conda install matplotlib tqdm
pip install traci sumolib mpl-tools shapely requests
```

Conda can then be deactivated later using the command:

```
conda deactivate tud-sumo
```

## Installation

TUD-SUMO is available on PyPI, and can be installed using the following command below. More information and previous release versions can be found on [PyPI](https://pypi.org/project/tud-sumo/).

```
pip install tud-sumo
```

To install a specific version of TUD-SUMO, use:

```
pip install tud-sumo==3.0.4
```