Cobaya, a code for Bayesian analysis in Cosmology: Installation and Using Guide
===================
Cobaya (code for bayesian analysis, and Spanish for Guinea Pig) is a framework for sampling and statistical modelling: it allows you to explore an arbitrary prior or posterior using a range of Monte Carlo samplers (including the advanced MCMC sampler from CosmoMC, and the advanced nested sampler PolyChord). The results of the sampling can be analysed with GetDist. It supports MPI parallelization (and very soon HPC containerization with Docker/Shifter and Singularity).

Its authors are Jesus Torrado and Antony Lewis. Some ideas and pieces of code have been adapted from other codes (e.g CosmoMC by Antony Lewis and contributors, and Monte Python, by J. Lesgourgues and B. Audren).

Cobaya has been conceived from the beginning to be highly and effortlessly extensible: without touching cobaya’s source code, you can define your own priors and likelihoods, create new parameters as functions of other parameter.

Though cobaya is a general purpose statistical framework, it includes interfaces to cosmological theory codes (CAMB and CLASS) and likelihoods of cosmological experiments (Planck, Bicep-Keck, SDSS… and more coming soon). Automatic installers are included for all those external modules. You can also use cobaya simply as a wrapper for cosmological models and likelihoods, and integrate it in your own sampler/pipeline.

The interfaces to most cosmological likelihoods are agnostic as to which theory code is used to compute the observables, which facilitates comparison between those codes. Those interfaces are also parameter-agnostic, so using your own modified versions of theory codes and likelihoods requires no additional editing of cobaya’s source.

The original web page [Cobaya Website](https://cobaya.readthedocs.io/en/latest/index.html) that is cited in this document. \
If you have any questions about installation, please feel free to contact me via email: j.pongsapatb@gmail.com

Preparation 
===================
First, the computer needs to install essential libraries and compilers.  
1. Ubuntu
- Install Compiler
```Linux
sudo apt update && sudo apt upgrade
sudo apt install nano
sudo apt install wget
sudo apt install git -y
sudo apt install liblapack-dev
sudo apt install libcfitsio-dev
sudo apt install build-essential
sudo apt-get install openmpi-bin openmpi-doc libopenmpi-dev
```

- Install Python and Librareis
  We recommd you to install Miniconda for better manage Python evironment.
```Linux
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
source ~/miniconda3/bin/activate
```
Then install Python and Libraries.
```Linux
python3 -m pip install pip
pip3 install numpy
pip3 install scipy
pip3 install matplotlib
pip3 install cython
pip3 install astropy
pip3 install getdist
pip3 install jupyter
conda install jupyter
```

In the other way, you can also install Python via Site-Package.
```Linux
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
brew install lapack
brew install cfitsio
brew install open-mpi
```

MacOS includes a built-in Python compiler within the site-packages libraries,  do not need to install Python via Homebrew. However, if an unresolved bug arise, it may become necessary to install Homebrew's Python at that point.

- Install Homebrew Python 
```Linux
brew install python3
python3 -m pip install --upgrade pip
```

- Install Python's required Librareis
```Linux
pip3 install numpy
pip3 install scipy
pip3 install matplotlib
pip3 install cython
pip3 install astropy
pip3 install getdist
pip3 install jupyter
brew install jupyter
```
For better handle Python evironment, we recommd you to install Miniconda.
 - For Apple Silicon chip
```Linux
mkdir -p ~/miniconda3
curl https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh -o ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
source ~/miniconda3/bin/activate
```
- For Intel chip
```Linux
mkdir -p ~/miniconda3
curl https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh -o ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
source ~/miniconda3/bin/activate
```
Then install Python and Libraries.
```Linux
python3 -m pip install pip
pip3 install numpy
pip3 install scipy
pip3 install matplotlib
pip3 install cython
pip3 install astropy
pip3 install getdist
pip3 install jupyter
conda install jupyter
```
  

Cobaya
===================

1. Cobaya Library Installation
```Linux
pip3 install cobaya
```

2. Cosmological theory codes and likelihoods. You need to set `/path/to/your/directory` to your folder such as `./`.
```Linux
cobaya-install cosmo -p /path/to/your/directory
cobaya-install planck_2018_highl_plik.TTTEEE
cobaya-install bicep_keck_2018
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
info = {'likelihood': {'planck_2018_highl_plik.TTTEEE': None,
                'planck_2018_lensing.clik': None,
                'planck_2018_lowl.EE': None,
                'planck_2018_lowl.TT': None},
 'params': {'A': {'derived': 'lambda As: 1e9*As',
                  'latex': '10^9 A_\\mathrm{s}'},
            'As': {'latex': 'A_\\mathrm{s}',
                   'value': 'lambda logA: 1e-10*np.exp(logA)'},
            'DHBBN': {'derived': 'lambda DH: 10**5*DH',
                      'latex': '10^5 \\mathrm{D}/\\mathrm{H}'},
            'H0': {'latex': 'H_0', 'max': 100, 'min': 20},
            'YHe': {'latex': 'Y_\\mathrm{P}'},
            'Y_p': {'latex': 'Y_P^\\mathrm{BBN}'},
            'age': {'latex': '{\\rm{Age}}/\\mathrm{Gyr}'},
            'clamp': {'derived': 'lambda As, tau: 1e9*As*np.exp(-2*tau)',
                      'latex': '10^9 A_\\mathrm{s} e^{-2\\tau}'},
            'cosmomc_theta': {'derived': False,
                              'value': 'lambda theta_MC_100: '
                                       '1.e-2*theta_MC_100'},
            'logA': {'drop': True,
                     'latex': '\\log(10^{10} A_\\mathrm{s})',
                     'prior': {'max': 3.91, 'min': 1.61},
                     'proposal': 0.001,
                     'ref': {'dist': 'norm', 'loc': 3.05, 'scale': 0.001}},
            'mnu': 0.06,
            'ns': {'latex': 'n_\\mathrm{s}',
                   'prior': {'max': 1.2, 'min': 0.8},
                   'proposal': 0.002,
                   'ref': {'dist': 'norm', 'loc': 0.965, 'scale': 0.004}},
            'ombh2': {'latex': '\\Omega_\\mathrm{b} h^2',
                      'prior': {'max': 0.1, 'min': 0.005},
                      'proposal': 0.0001,
                      'ref': {'dist': 'norm', 'loc': 0.0224, 'scale': 0.0001}},
            'omch2': {'latex': '\\Omega_\\mathrm{c} h^2',
                      'prior': {'max': 0.99, 'min': 0.001},
                      'proposal': 0.0005,
                      'ref': {'dist': 'norm', 'loc': 0.12, 'scale': 0.001}},
            'omega_de': {'latex': '\\Omega_\\Lambda'},
            'omegam': {'latex': '\\Omega_\\mathrm{m}'},
            'omegamh2': {'derived': 'lambda omegam, H0: omegam*(H0/100)**2',
                         'latex': '\\Omega_\\mathrm{m} h^2'},
            'rdrag': {'latex': 'r_\\mathrm{drag}'},
            's8h5': {'derived': 'lambda sigma8, H0: sigma8*(H0*1e-2)**(-0.5)',
                     'latex': '\\sigma_8/h^{0.5}'},
            's8omegamp25': {'derived': 'lambda sigma8, omegam: '
                                       'sigma8*omegam**0.25',
                            'latex': '\\sigma_8 \\Omega_\\mathrm{m}^{0.25}'},
            's8omegamp5': {'derived': 'lambda sigma8, omegam: '
                                      'sigma8*omegam**0.5',
                           'latex': '\\sigma_8 \\Omega_\\mathrm{m}^{0.5}'},
            'sigma8': {'latex': '\\sigma_8'},
            'tau': {'latex': '\\tau_\\mathrm{reio}',
                    'prior': {'max': 0.8, 'min': 0.01},
                    'proposal': 0.003,
                    'ref': {'dist': 'norm', 'loc': 0.055, 'scale': 0.006}},
            'theta_MC_100': {'drop': True,
                             'latex': '100\\theta_\\mathrm{MC}',
                             'prior': {'max': 10, 'min': 0.5},
                             'proposal': 0.0002,
                             'ref': {'dist': 'norm',
                                     'loc': 1.04109,
                                     'scale': 0.0004},
                             'renames': 'theta'},
            'zrei': {'latex': 'z_\\mathrm{re}'}},
 'sampler': {'mcmc': {'Rminus1_cl_stop': 0.2,
                      'Rminus1_stop': 0.01,
                      'covmat': 'auto',
                      'drag': True,
                      'oversample_power': 0.4,
                      'proposal_scale': 1.9}},
 'theory': {'camb': {'extra_args': {'bbn_predictor': 'PArthENoPE_880.2_standard.dat',
                                    'halofit_version': 'mead',
                                    'lens_potential_accuracy': 1,
                                    'nnu': 3.044,
                                    'num_massive_neutrinos': 1,
                                    'theta_H0_range': [20, 100]}}},
       'output' : 'chains/test1/test1'
}
```
```Python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.Get_rank()

from cobaya import run
from cobaya.log import LoggedError

success = False
try:
    upd_info, mcmc = run(info)
    success = True
except LoggedError as err:
    pass

# Did it work? (e.g. did not get stuck)
success = all(comm.allgather(success))

if not success and rank == 0:
    print("Sampling failed!")
```
Remark: output directory must be specify in running file. There are main 4 styles of setting for output directories.
* `output: something` the output will be written into the current folder, and all output file names will start with `something`.
* `output: somefolder/something` similar to the last case, but writes into the folder `somefolder`, which is created at that point if necessary.
* `output: somefolder/` writes into the folder `somefolder`, which is created at that point if necessary, with no prefix for the file names.
* `output: null` will produce no output files whatsoever – the products will be just loaded in memory. Use only when invoking from the Python interpreter.
* Instead, when calling from a `Python interpreter`, if output has not been specified, it is understood as `output: null`.

5. Cobaya Parallel Run 
Following this, compilation is required, and it is essential to load the necessary modules as follows.

```Linux
#!/bin/bash

#SBATCH -J Cobaya   # Job name
#SBATCH -n 4        # Total number of mpi tasks

module purge
module load hwloc
module load intel/19.0.5.281
module load openmpi3/4.0.2

mpirun -n 4 python3 <model.py>
```

```Linux
#!/bin/bash

#SBATCH -J Cobaya    # Job name
#SBATCH -N 1         # Total number of nodes requested
#SBATCH -n 4         # Total number of mpi tasks
#SBATCH --cpus-per-task=4

module purge
module load hwloc
module load intel/19.0.5.281
module load openmpi3/4.0.2

mpirun -n 4 -c 4 python3 <model.py>
```


Tutorial for basic Slurm Commands: [http://chalawan.narit.or.th/home/index.php/using-pollux/using-slurm/](http://chalawan.narit.or.th/home/index.php/using-pollux/using-slurm/) 


6. Output Files

* `[prefix].input.yaml` a file with the same content as the input file.
* `[prefix].updated.yaml` a file containing the input information plus the default values used by each component.
* `[prefix].[number].txt` one or more sample files, containing one sample per line, with values separated by spaces. The first line specifies the columns.
* `[prefix].progress` a file monitoring the convergence of the chain, that can be inspected or plotted.

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


