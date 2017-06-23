# Underwater Image Systems Simulation

<p text-align="justify">
Digital cameras are ubiquitus and are slowly finding their way in niche application areas, such
as imaging below the surface of the sea. Underwater imaging is becoming popular among
consumers (GoPro action cameras) as well as professionals (underwater exploration). 

Underwater imaging faces many more challenges than imaging on the ground, for example excessive 
absorption of the red light component or sever backscattering. Realistic simulations enable 
to explore some of those limitations without the need for troublesome experiments.

This project provides a set of tools to realisticly simualte the appearance of underwater scenes
in a variety of water conditions. Simulated images faciliate evaluation, and comparisons across 
different underwater imaing systems and correction algorithms.
</p>

<p align="center"> 
<img src="https://github.com/hblasins/uwSim/blob/master/Figures/scatter_default.png">
<img src="https://github.com/hblasins/uwSim/blob/master/Figures/scatter_direct.png">
</p>


If you use these tools please cite
```
@inproceedings{blasinski2017uiss,
    title={Underwater Image Systems Simulation},
    author={Blasinski, Henryk and Lian, Trisha and Farrell, Joyce},
    booktitle={Imaging and Applied Optics Congress},
    year={2017},
    organization={OSA}
}
```

## Dependencies

To succesffuly run the scripts in this project the following dependencies need to be 
installed.

* [Homebrew](https://brew.sh) - a package manager for Mac OSX, it will make installing many components much easier.
* [RenderToolbox4](http://rendertoolbox.org) - a set of Matlab tools to create, manipulate and
render complex 3D scene files.
* [Docker](https://www.docker.com) - RenderToolbox4 uses either Mitsuba or PBRT renderers to 
produce final images. Rather than install these programs we provide their dockerized implementations.
A cross-platform Docker engine is required to run these.
* [CVX](http://cvxr.com/) - a Matlab toolbox for convex optimization.
* [ISET](http://imageval.com) - Image Systems Engineering Toolbox for camera sensor simulations.
* [DCRaw](https://www.cybercom.net/~dcoffin/dcraw/) - a simple program for reading raw camera images.

We have successfully installed and run the simulator on OSX and Linux (Debian) systems. If you are interested in using the code on a Windows machine the biggest limitation is RenderToolbox4, which is not officially supported on Windows platforms (but we have heard of some successful installations).


## Data

In order to reproduce the results from our publications please download the input data (underwater
images, 3D models) from [Stanford Digital Repository](https://purl.stanford.edu/wp894vt1248).

The Stanford Digital Repository contains a single, ~3GB `.zip` file. Once you extract it's contents you will see two directories, please move them to the github directory for this project. 
* **Images** contains two sets of real images captured with a Canon G7X camera. The first set are underwater images of a Macbeth chart captured in a few locations in the West Indies. The second set are images of a white Spectralon target illuminated with monochromatic light. This set was used to derive the camera responsivity curves.
* **Results** are outputs of some of the scripts we provide. You should be able to re-generate this data. We only provide this directory for your reference and so you can run the scripts that use the data in different analyses. 

## Directory structure

* **Code** contains all the Matlab source code.
  * **Code/Utilities** a number of 'unrelated' functions we use thourghout the project (for example XML file parsing)
  * **Code/Remodellers** functions used by RenderToolbox4 to change the scene geometry or other properties. Please refer to [RenderToolbox4 docummentation](https://github.com/RenderToolbox/RenderToolbox4/wiki/Flythrough) for details.
  * **Code/VideosAndFigures** functions to generate sample images, figures and videos used in presentations and publications.
* **Figures** images and movies that were generated from the simulation results. For example 
the two figures on top of this page.
* **Parameters** contains calibration data for different cameras and light sources. For example
the spectral responsivities of the Canon G7X used in the experiments.
* **Scenes** 3D scene files used in simulations. We usually generate these scenes in [Blender](https://www.blender.org)

## Installation


### 1. Homebrew (MacOS)
To install Homebrew run the following in your terminal window.
```
>> /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
### 1. Apt-get (Linux)
Update `apt-get`
```
>> sudo apt-get update
>> sudo apt-get upgrade
```


### 2. RenderToolbox4
The RenderToolbox code is contained in three github repositories: RenderToolbox4, mexximp and mPbrt, you will need to clone all of them.
```
>> git clone https://github.com/RenderToolbox/RenderToolbox4.git
>> git clone https://github.com/RenderToolbox/mexximp.git
>> git clone https://github.com/RenderToolbox/mPbrt.git
```
RenderToolbox4 requires several dependencies and can be a little tricky to install. Please make sure to follow the installation instructions either from the [official site](https://github.com/RenderToolbox/RenderToolbox4) or, if you run into isses, you can also go over [these notes](https://github.com/scienstanford/ciset/wiki/Installation).

### 3. Docker
Go to [Docker website](https://www.docker.com) and search for the installation package. We use Docker Community Edition for Mac. Follow the installation instructions. Once Docker is set up, the software will automatically download the necessary image rendering components from the cloud. 

### 4. CVX
Visit the [CVX site](http://web.cvxr.com/cvx/cvx-maci64.zip) to download the standard `CVX` bundle for Mac OSX. Unzip the archive in your target directory (for example `~/Documents/MATLAB`). After you launch MATLAB, set the `CVX` directory as the working folder and run the `cvx_setup.m` script, which will set up `CVX` on your machine.

### 5. Image Systems Engineering Toolkit (ISET)
Clone the ISET repository to your local drive.
```
>> git clone https://github.com/imageval/iset.git
```
In MATLAB go to the ISET root directory and run `isetPath(pwd)`. This function will add ISET directory and sub-directories to your MATLAB PATH.

### 6. DCRaw
Run the following command in your terminal window.

```
MacOS:
>> brew install dcraw

Linux:
>> sudo apt-get install dcraw
```


### 7. Underwater Image Systems Simulator
Clone this repository to your local drive
```
>> git clone https://github.com/hblasins/uwSim.git
```
Inside MATLAB run `Code/install.m` function to correctly set up MATLAB PATH in your environment. 

## Getting started

To get you started please have a look (and run) at the `Code/renderUnderwaterChart.m` script. This is a simple script that renders an image of a Macbeth chart submertged in water. You can specify a number of parameters that affect the final image appearance :
* depth
* camera to chart distance
* chlorophyl concentraion
* color dissolved organic matter (CDOM) concentraion
* small particle concentration
* large particle concentration

Once the computation is completed you should see the following image.

<p align="center"> 
<img src="https://github.com/hblasins/uwSim/blob/master/Figures/sampleOutput.tiff">
</p>

Many other scripts in the `Code` directory are a variation on the `renderUnderwaterChart.m` where we simply vary or co-vary different parameters and produce a large number of output images.


