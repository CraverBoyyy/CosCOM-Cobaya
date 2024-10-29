Cobaya, a code for Bayesian analysis in Cosmology: Installation and Using Guide
===================
Cobaya (code for bayesian analysis, and Spanish for Guinea Pig) is a framework for sampling and statistical modelling: it allows you to explore an arbitrary prior or posterior using a range of Monte Carlo samplers (including the advanced MCMC sampler from CosmoMC, and the advanced nested sampler PolyChord). The results of the sampling can be analysed with GetDist. It supports MPI parallelization (and very soon HPC containerization with Docker/Shifter and Singularity).

Its authors are Jesus Torrado and Antony Lewis. Some ideas and pieces of code have been adapted from other codes (e.g CosmoMC by Antony Lewis and contributors, and Monte Python, by J. Lesgourgues and B. Audren).

Cobaya has been conceived from the beginning to be highly and effortlessly extensible: without touching cobaya’s source code, you can define your own priors and likelihoods, create new parameters as functions of other parameters…

Though cobaya is a general purpose statistical framework, it includes interfaces to cosmological theory codes (CAMB and CLASS) and likelihoods of cosmological experiments (Planck, Bicep-Keck, SDSS… and more coming soon). Automatic installers are included for all those external modules. You can also use cobaya simply as a wrapper for cosmological models and likelihoods, and integrate it in your own sampler/pipeline.

The interfaces to most cosmological likelihoods are agnostic as to which theory code is used to compute the observables, which facilitates comparison between those codes. Those interfaces are also parameter-agnostic, so using your own modified versions of theory codes and likelihoods requires no additional editing of cobaya’s source.

The original web page [Cobaya Website](https://cobaya.readthedocs.io/en/latest/index.html) that is cited in this document.

Preparation 
===================
cobaya is a python library that need

1. Ubuntu
- Install Compiler
```Linux
sudo apt update && sudo apt upgrade
sudo apt install nano
sudo apt install wget
sudo apt install git -y
sudo apt install python3
sudo apt install python3-pip
pip3 install numpy
pip3 install scipy
pip3 install matplotlib
pip3 install cython
pip3 install astropy
pip3 install getdist
pip3 install jupyter
sudo apt install jupyter
sudo apt install liblapack-dev
sudo apt install libcfitsio-dev
sudo apt install build-essential
sudo apt-get install openmpi-bin openmpi-doc libopenmpi-dev
```

2. MacOS
- Install HomeBrew
```Linux
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
- Install Compiler
```Linux
brew install wget
brew install git
brew install nano
brew install python3
python3 -m pip install --upgrade pip
pip3 install numpy
pip3 install scipy
pip3 install matplotlib
pip3 install cython
pip3 install astropy
pip3 install getdist
pip3 install jupyter
brew install jupyter
brew install lapack
brew install cfitsio
brew install open-mpi
```

Cobaya
===================

1. Cobaya Library Installation
```Linux
pip3 install cobaya
```

2. Cosmological theory codes and likelihoods
```Linux
cobaya-install cosmo -p /path/to/packages
cobaya-install planck_2018_highl_plik.TTTEEE
```
You need to place theory codes and likelihoods in the `/path/to/packages` directory, but you can also modified this path to suit on your own machine.

3. Setting Cosmology Run
Creating the input for a realistic cosmological case is quite a bit of work. But to make it simpler, cobaya has created an automatic input generator, that you can run from the shell.

```Linux
python3 -m pip install PySide6
```

```Linux
cobaya-cosmo-generator
```
<p align="center">
<img src="https://github.com/user-attachments/assets/400ca718-cfd2-405f-a69a-8405e64a2e89"  width="960px" height="516px">
</p>
4. Sample Run

 ```Python

```


5. Cobaya Parallel Run 
For running on the pollux node, if model has been modified, a symbolic link for `$CLIK_PATH` must be created in step 1. Following this, compilation is required, and it is essential to load the necessary modules as follows.
```Linux
module purge
module load hwloc
module load intel/19.0.5.281
module load openmpi3/4.0.2
```
```Linux
#!/bin/bash

#SBATCH -J CosmoMC      # Job name
#SBATCH -n 10           # Number of tasks
#SBATCH -p chalawan_gpu # Partition
#SBATCH -w pollux3

module purge
module load hwloc
module load intel/19.0.5.281
module load openmpi3/4.0.2

mpirun ./cosmomc <path to .ini file>
```

For running on the castor node, it is necessary to install the Planck likelihood and recompile using the GNU Compiler. The important modules should be loaded as follows.
```Linux
module purge
module load hwloc
module load openmpi3
```
```Linux
#!/bin/bash

#SBATCH -J Cobaya      # Job name
#SBATCH -p chalawan_cpu # Partition
#SBATCH -n 10           # Number of task

module purge
module load hwloc
module load openmpi3
source data/clik_14.0/bin/clik_profile.sh

mpirun ./cosmomc <path to .ini file>
```

Tutorial for basic Slurm Commands: [http://chalawan.narit.or.th/home/index.php/using-pollux/using-slurm/](http://chalawan.narit.or.th/home/index.php/using-pollux/using-slurm/) 


6. Output

* `.txt file` lists each accepted set of parameters for each chain.
* `.log file` contains some info which may be useful to assess performance.
* `.data file` contains full computed model information at the independent sample points (power spectra etc).
* `.chk file` stores the current status that if the processes are terminated they can be restarted again from close to where they left off.
* `.inputparams file` contains the input values of the parameters and setting.
* `.likelihood file` lists the names of the likelihoods.
* `.paramnames file` lists the names and labels of the parameters.
* `.ranges file` lists the name tags of the parameters, and their upper and lower bounds.
* `.converge_stat file` contains "R-1" statistic that also used for the stopping criterion when generating chains with MPI.

Plotting with GetDist
===================

To generate distribution plots and triangle plots, you need packages for graph creation from files, which are output files from running CosmoMC. The packages have been use in Python. For displaying plots, it is recommended to use Jupyter Notebook or Jupyter Lab. Additional information can be found at: [https://getdist.readthedocs.io/en/latest/](https://getdist.readthedocs.io/en/latest/) and [ https://getdist.readthedocs.io/en/latest/plot\_gallery.html](https://getdist.readthedocs.io/en/latest/plot\_gallery.html)

First, you need to import the getdist library and select the chain you want to use for plotting graph by specifying the directory path to your output files in the line `file_root`. For example, `file_root1` has been dowloaded output files from `planck/plikHM_TTTEEE_lowl_lowE_BK15_lensing/base_r_plikHM_TTTEEE_lowl_lowE_BK15_lensing_post_BAO`. You do not to specify type of files such as `base_r_plikHM_TTTEEE_lowl_lowE_BK15_lensing_post_BAO_1.txt` or `base_r_plikHM_TTTEEE_lowl_lowE_BK15_lensing_post_BAO.inputparams`. Set only name of files `base_r_plikHM_TTTEEE_lowl_lowE_BK15_lensing_post_BAO`.

```python
%matplotlib inline
import getdist
from getdist import plots, MCSamples, loadMCSamples

file_root1 = 'planck/plikHM_TTTEEE_lowl_lowE_BK15_lensing/base_r_plikHM_TTTEEE_lowl_lowE_BK15_lensing_post_BAO'
samples1 = loadMCSamples(file_root=file_root1,settings={'ignore_rows':0.5})
```
2D plot
```python
g2 = plots.get_subplot_plotter(width_inch=5)
g2.settings.axes_fontsize = 16
g2.settings.axes_labelsize = 20
g2.plot_2d([samples1],'omegabh2','omegach2',filled=True,contour_lws=1.5)
```
<p align="center">
<img src="https://github.com/CraverBoyyy/CosmoMC-Installation/assets/109847168/b3fff9b8-fdac-4e60-9a71-68af71c1f8b9"  width="500px" height="500px">
</p>

Triangle plot
```python
g = plots.get_subplot_plotter(width_inch=10)
g.settings.axes_fontsize = 16
g.settings.axes_labelsize = 20
g.triangle_plot(samples1,['omegabh2','omegach2','theta','tau','logA','ns'],filled=True,contour_lws=1.5)
```
<p align="center">  
<img src="https://github.com/CraverBoyyy/CosmoMC-Installation/assets/109847168/6b1cf5c6-e40f-4157-88c4-7ff48a081819" width="700px" height="700px"  align="center" >
</p>

Triangle plot with uncertainty limit at 68%CL and 95%CL
```python
g.triangle_plot(samples1,['omegabh2','omegach2','theta','tau','logA','ns'],filled=True,contour_lws=1.5,title_limit=2)
```
<p align="center">  
<img src="https://github.com/CraverBoyyy/CosmoMC-Installation/assets/109847168/e3cfdad7-847a-4424-ba9d-5f79e7c897dc" width="700px" height="700px"  align="center" >
</p>

If you want to compare the two or more models results, you can add additional chains to the code by including another file_root similar to the first dataset. You can also adjust the number of parameters in the same way.
```python
file_root2 = 'planck/plikHM_TTTEEE_lowl_lowE_BK15_lensing/base_r_plikHM_TTTEEE_lowl_lowE_BK15_lensing'
samples2 = loadMCSamples(file_root=file_root2,settings={'ignore_rows':0.5})

g.triangle_plot((samples1,samples2),['omegabh2','omegach2','theta','tau','logA','ns'],filled=True,contour_lws=1.5)
```
<p align="center">        
<img src="https://github.com/CraverBoyyy/CosmoMC-Installation/assets/109847168/b236a90e-5337-41a9-8252-988f8944275a" width="700px" height="700px"  align="center" >
</p>


