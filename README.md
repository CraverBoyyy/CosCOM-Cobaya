
Cobaya, a code for Bayesian analysis in Cosmology: Installation and Using Guide
===================
Cobaya (code for bayesian analysis, and Spanish for Guinea Pig) is a framework for sampling and statistical modelling: it allows you to explore an arbitrary prior or posterior using a range of Monte Carlo samplers (including the advanced MCMC sampler from CosmoMC, and the advanced nested sampler PolyChord). The results of the sampling can be analysed with GetDist. It supports MPI parallelization (and very soon HPC containerization with Docker/Shifter and Singularity).

Its authors are Jesus Torrado and Antony Lewis. Some ideas and pieces of code have been adapted from other codes (e.g CosmoMC by Antony Lewis and contributors, and Monte Python, by J. Lesgourgues and B. Audren).

Cobaya has been conceived from the beginning to be highly and effortlessly extensible: without touching cobaya’s source code, you can define your own priors and likelihoods, create new parameters as functions of other parameters…

Though cobaya is a general purpose statistical framework, it includes interfaces to cosmological theory codes (CAMB and CLASS) and likelihoods of cosmological experiments (Planck, Bicep-Keck, SDSS… and more coming soon). Automatic installers are included for all those external modules. You can also use cobaya simply as a wrapper for cosmological models and likelihoods, and integrate it in your own sampler/pipeline.

The interfaces to most cosmological likelihoods are agnostic as to which theory code is used to compute the observables, which facilitates comparison between those codes. Those interfaces are also parameter-agnostic, so using your own modified versions of theory codes and likelihoods requires no additional editing of cobaya’s source.

Preparation 
===================
* Hardware : To employ the provided parallel capabilities of CosmoMC, you need more than just one core. Markov chain algorithms are time-consuming and performing them on parallel machines will boost the code’s performance.
* Operating System : CosmoMC is available for Linux and MacOS. Here, we installed and tested CosmoMC on Ubuntu and also MacOS.
* Compilers : CosmoMC is compatible with GFORTRAN compiler (GNU) and IFORT compiler (Oneapi).
* Open-MPI : OpenMPI library is required for running on parallel machines.
* Planck Likelihood : Planck Likelihood Code V3.0 is required to run CosmoMC with Planck 2018 data.
 
1. Ubuntu
- Install GNU Compiler
```Linux
sudo apt update && sudo apt upgrade
sudo apt install gcc
sudo apt install gfortran
sudo apt install g++
sudo apt install make
sudo apt install gedit
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
- Install GNU Compiler
```Linux
brew install gcc
brew install gfortran
brew install g++
brew install make
brew install gedit
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

Planck Likelihood
===================
Due to the large size of the data file, approximately 20 GB, it is recommended to download only **Code** and **Baseline** for compilation. For other files, you can download and move the files into the successfully compiled directory. Alternatively, you can directly download the files from  [Planck Likelihood](https://pla.esac.esa.int) on the Cosmology page by clicking on Likelihood, which will display all 7 files available for download.
```Linux
wget -O COM_Likelihood_Code-v3.0_R3.10.tar.gz "http://pla.esac.esa.int/pla/aio/product-action?COSMOLOGY.FILE_ID=COM_Likelihood_Code-v3.0_R3.10.tar.gz"
wget -O COM_Likelihood_Data-baseline_R3.00.tar.gz "http://pla.esac.esa.int/pla/aio/product-action?COSMOLOGY.FILE_ID=COM_Likelihood_Data-baseline_R3.00.tar.gz"
```
```Linux
tar -xzvf COM_Likelihood_Code-v3.0_R3.10.tar.gz
tar -xzvf COM_Likelihood_Data-baseline_R3.00.tar.gz
mv -f baseline/plc_3.0/* code/plc_3.0/plc-3.1/
cd code/plc_3.0/plc-3.1/
python3 ./waf configure --install_all_deps
python3 ./waf install
source ./bin/clik_profile.sh
```
If you want to use the Intel Compiler, add the command `--lapack\_mkl=\$MKLROOT` at the end of `python3 ./waf configure --install_all_deps` line. After installing the **code** file, proceed to extract the remaining files. Then, move the **hi\_l**, **low\_l**, **lensing** files from the extracted directory to the `code/plc\_3.0/plc-3.1/` directory.
```Linux
wget -O COM_Likelihood_Data-extra-plik-lite-ext_R3.00.tar.gz "http://pla.esac.esa.int/pla/aio/product-action?COSMOLOGY.FILE_ID=COM_Likelihood_Data-extra-plik-lite-ext_R3.00.tar.gz"
wget -O COM_Likelihood_Data-extra-camspec-ext_R3.00.tar.gz "http://pla.esac.esa.int/pla/aio/product-action?COSMOLOGY.FILE_ID=COM_Likelihood_Data-extra-camspec-ext_R3.00.tar.gz"
wget -O COM_Likelihood_Data-extra-plik-ext_R3.00.tar.gz "http://pla.esac.esa.int/pla/aio/product-action?COSMOLOGY.FILE_ID=COM_Likelihood_Data-extra-plik-ext_R3.00.tar.gz"
wget -O COM_Likelihood_Data-extra-bflike-ext_R3.00.tar.gz "http://pla.esac.esa.int/pla/aio/product-action?COSMOLOGY.FILE_ID=COM_Likelihood_Data-extra-bflike-ext_R3.00.tar.gz"
wget -O COM_Likelihood_Data-extra-lensing-ext_R3.00.tar.gz "http://pla.esac.esa.int/pla/aio/product-action?COSMOLOGY.FILE_ID=COM_Likelihood_Data-extra-lensing-ext_R3.00.tar.gz"
```

```Linux
tar -xzvf COM_Likelihood_Data-extra-plik-lite-ext_R3.00.tar.gz
tar -xzvf COM_Likelihood_Data-extra-camspec-ext_R3.00.tar.gz
tar -xzvf COM_Likelihood_Data-extra-plik-ext_R3.00.tar.gz
tar -xzvf COM_Likelihood_Data-extra-bflike-ext_R3.00.tar.gz
tar -xzvf COM_Likelihood_Data-extra-lensing-ext_R3.00.tar.gz
mv extended_plik_lite/plc_3.0/hi_l/plik_lite/* code/plc_3.0/plc-3.1/hi_l/plik_lite/
mv extended_plik/plc_3.0/hi_l/plik/* code/plc_3.0/plc-3.1/hi_l/plik/
mv extended_lensing/plc_3.0/lensing/* code/plc_3.0/plc-3.1/lensing/
mv -f extended_bflike/plc_3.0/low_l/bflike code/plc_3.0/plc-3.1/low_l/
mv -f extended_camspec/plc_3.0/hi_l/camspec code/plc_3.0/plc-3.1/hi_l/
```

Cobaya
===================

1. Installation
```Linux
git clone --recursive https://github.com/cmbant/CosmoMC.git
cd CosmoMC
ln -s /PATH/to/plc-3.1 ./data/clik_14.0
```
The PATH of the `/plc-3.1` file can be checked by typing the command `echo $CLIK_PATH`. It will display the PATH of the file.

2. Compilation
```Linux
make
```
After successful compiling, a file named `cosmomc` will be created in CosmoMC directory.

```Linux
make clean && make
```
If unable to execute the `make` command, delete the file `cosmomc` first by typing the command `rm cosmomc`. The compilation of the CAMB program, which is used for calculating the theoretical part of CosmoMC and is located within a subdirectory `camb/fortran/`. If the compilation is successful, the file `camb` will appear in the directory.

```Linux
cd camb/fortran/
make
```

3. Running
To run CosmoMC you need to run it with MPI/OpenMPI using mpirun command. Set your corrent directory to CosmoMC directory and run this command.

```Linux
mpirun -np number of processers (cores) ./cosmomc <.ini file>
```
Inside the `.ini file` there are some lines which start with DEFAULT keyword. If these lines are not commented, it means we can use likelihoods related to these data.
```
#high-L plik likelihood
DEFAULT(batch3/plik_rd12_HM_v22_TTTEEE.ini)

#low-L temperature
DEFAULT(batch3/lowl.ini)

#low-L EE polarization
DEFAULT(batch3/simall_EE.ini)

#Bicep-Keck-Planck 2015, varying cosmological parameters (use only if varying r)
DEFAULT(batch3/BK15.ini)

#DES 1-yr joint
DEFAULT(batch3/DES.ini)

#Planck 2018 lensing (native code, does not require Planck likelihood code)
DEFAULT(batch3/lensing.ini)

#BAO compilation
DEFAULT(batch3/BAO.ini)

#Pantheon SN
DEFAULT(batch3/Pantheon.ini)

#general settings
DEFAULT(batch3/common.ini)

propose_matrix= planck_covmats/<.covmat file for each work>

#Folder where files (chains, checkpoints, etc.) are stored
root_dir = chains/<model directory for each work>/

#Root name for files produced
file_root=<name>
#action= 0 runs chains, 1 importance samples, 2 minimizes
#use action=4 just to quickly test likelihoods
action = 0

#turn on checkpoint for real runs where you want to be able to continue them
checkpoint = T
```
Setting `root_dir` to model directory and `file_root` should contain information about the model and used datasets. To do a MCMC run you need to find the line which starts with action and set its value to 0.

4. Running on Cluster

Running CosmoMC can be initiated on an HPC (High Performance Computing) machine for faster completion compared to using a regular computer. Currently in Thailand, access to the Chalawan HPC facility at the National Astronomical Research Institute of Thailand (NARIT) can be requested for running jobs. There are two main nodes available for execution: the pollux node (GPU) and the castor node (CPU). The pollux node tends to run faster, but since there are only 2 nodes, it is recommended to use the castor node, which offers more nodes for processing.

For running jobs on the cluster, `.sh` files must be used to keep commands for the HPC to execute the jobs. In the Chalawan HPC, built-in modules are available for running unmodified models. For instance, the LCDM model can be run using these built-in modules.

 ```Linux
#!/bin/bash

#SBATCH -J CosmoMC      # Job name
#SBATCH -n 10           # Number of tasks

module purge
module load hwloc
module load intel/19.0.5.281
module load openmpi3/4.0.2
module load CosmoMC
mpirun <path to .ini file>
```
For running on the pollux node, if model has been modified, a symbolic link for `$CLIK_PATH` must be created in step 1. Following this, compilation is required, and it is essential to load the necessary modules as follows.
```Linux
module purge
module load hwloc
module load intel/19.0.5.281
module load openmpi3/4.0.2
export PLANCKLIKE=cliklike
export CLIK_DATA=/data3/opt/ohpc/pub/apps/CosmoMC-Oct19/plc_3.1/share/clik
export CLIK_PATH=/data3/opt/ohpc/pub/apps/CosmoMC-Oct19/plc_3.1
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CLIK_PATH/lib
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
export PLANCKLIKE=cliklike
export CLIK_DATA=/data3/opt/ohpc/pub/apps/CosmoMC-Oct19/plc_3.1/share/clik
export CLIK_PATH=/data3/opt/ohpc/pub/apps/CosmoMC-Oct19/plc_3.1
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CLIK_PATH/lib
mpirun ./cosmomc <path to .ini file>
```
For running on the castor node, it is necessary to install the Planck likelihood and recompile using the GNU Compiler. The important modules should be loaded as follows.
```Linux
module purge
module load gnu8
module load hwloc
module load openmpi3
```
```Linux
#!/bin/bash

#SBATCH -J CosmoMC      # Job name
#SBATCH -p chalawan_cpu # Partition
#SBATCH -n 10           # Number of task

module purge
module load gnu8
module load hwloc
module load openmpi3
source data/clik_14.0/bin/clik_profile.sh

mpirun ./cosmomc <path to .ini file>
```

Tutorial for basic Slurm Commands: [http://chalawan.narit.or.th/home/index.php/using-pollux/using-slurm/](http://chalawan.narit.or.th/home/index.php/using-pollux/using-slurm/) 

5. Output

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


