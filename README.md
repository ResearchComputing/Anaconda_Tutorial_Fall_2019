# Tutorial: Using Python with Anaconda

To support the diverse python workflows and high levels of customization Research Computing users require, [Anaconda](http://anaconda.com) is installed on the CURC system. Anaconda is an open-source _python_ and _R_ distribution that uses the _conda_ package manager to easily install software and packages. The following tutorial describes how to: 

* activate the CURC Anaconda distribution and our default environments
* how to create and activate your own custom Anaconda environments
* how to create a kernel to use your custom environment in RC JupyterHub
* how to use your custom envirnoment in RC JupyterHub

## Finding Help

For future reference, the following documentation is available:

* [CURC Anaconda distribution](https://curc.readthedocs.io/en/latest/software/python.html)
* [CURC JupyterHub](https://curc.readthedocs.io/en/latest/gateways/jupyterhub.html) 

## Using the CURC Anaconda environment

Follow these steps from a Research Computing terminal session (on a Summit _scompile_ or _compute_ node, or a Blanca _compute_ node). 

### Before you use conda for the first time:

#### Modify your ~/.condarc file so that packages are downloaded to your _/projects_ directory

Your _/home/$USER_ directory (also denoted with "_~_") is small -- only 2 GB. By default, conda downloads packages to your home directory when creating a new environment, and it will quickly become full. The steps here modify the conda configration file, called _~/.condarc_, to change the default location of _pkgs_dirs_ so that the packages are downloaed to your (much bigger) _/projects_ directory.  Additionally, we will specify the location of _envs_dirs_ so that your custom environments are also installed in your _/projects_ directory. 

Open your _~/.condarc_ file in your favorite text editor (e.g., nano):
_(note: this file may not exist yet -- if not, just create a new file with this name)_
```
[johndoe@shas0137]$ nano ~/.condarc
```

...and add the following lines (add the first two at a minimum:
```
pkgs_dirs:
  - /projects/$USER/.conda_pkgs
envs_dirs:
  - /projects/$USER/software/anaconda/envs
```

...then save and exit the file. You won't need to perform this step again -- it's permanent unless you change _pkgs_dirs_ by editing _~/.condarc_ again.

Note that there are lots of other things you can customize using the [~/.condarc file](https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html).


### Activating the CURC Anaconda environment

```
[johndoe@shas0137 ~]$ source /curc/sw/anaconda/latest
(base) [johndoe@shas0137 ~]$ conda activate idp
```

The first command activates the "base" Anaconda python3 environment. You will know that you have properly activated the environment because you should see _`(base)`_ in front of your prompt. E.g.: 

```
(base) [johndoe@shas0137 ~]$
```

The second command (_conda activate idp_) activates the Intel python distribution (idp), which is optimized for many mathematics functions and will run more efficiently on the Intel architecture of Summit and Blanca. You will know that you have properly activated the environment because you should see _`(idp)`_ in front of your prompt. E.g.: 

```
(idp) [johndoe@shas0137 ~]$
```

### Using python in Anaconda

#### To list the packages currently installed in the environment:

```
(idp) [johndoe@shas0137 ~]$ conda list
```

#### To deactivate an environment:

```
(idp) [johndoe@shas0137 ~]$ conda deactivate
```

#### To create a new environment in your /projects directory.  

*Note: In the examples below the environment is created in /projects/$USER/software/anaconda/envs. This assumes that the software, anaconda, and envs directories already exist in /projects/$USER. Environments can be installed in any writable location the user chooses.

 ##### Activate the base conda environment if you haven't already done so.
 
```
[johndoe@shas0137 ~]$ source /curc/sw/anaconda/default
```

 ##### _Create a custom environment "from scratch"_: Here we create a new environment called _tutorial1_ that has a base python version of 3.6:

```
(base) [johndoe@shas0137 ~]$ conda create --name tutorial1 python==3.6
```

  * _Note that you can also create a custom environment by cloning a preexisting environment. For example, you can clone the preexisting Intel Python3 distribution in the CURC Anaconda environment, creating a new environment called _mycustomenv_: `(base) [johndoe@shas0137 ~]$ conda create --clone idp --name myidp` (_we won't do this in the tutorial_).


#### Confirm your new environment exists by listing all environments currently available:

```
(base) [johndoe@shas0137 ~]$ conda env list
```

##### Activate and use your new environment

```
(base) [johndoe@shas0137 ~]$ conda activate tutorial1
```

##### To list existing packages in your new environment:

```
(tutorial1) [johndoe@shas0137 ~]$ conda list
```

##### To add a new package to the environment:

_in this example we install ipykernel so that we can create a Jupyter kernel later. It will take a couple of minutes_

```
(tutorial1) [johndoe@shas0137 ~]$ conda install ipykernel
```

##### Confirm the package is installed

```
(tutorial1) [johndoe@shas0137 ~]$ conda list | grep ipykernel
```

### Using Anaconda in RC JupyterHub

[Jupyter notebooks](https://jupyter.org/) are an excellent resource for interactive development and data analysis using Python, R, and other languages. Jupyter notebooks can contain live code, equations, visualizations, and explanatory text which provide an excellent enviornment to use, learn, and teach interactive data analysis.  

CU Research Computing (CURC) operates a [JupyterHub server](https://jupyterhub.readthedocs.org/en/latest/) that enables users to run Jupyter notebooks on Summit or Blanca for serial (single core) and shared-memory parallel (single node) workflows. The CURC JupyterHub runs atop of [Anaconda](http://anaconda.com).  Additional documentation on the [CURC Anaconda distribution](../software/python.md) is available and may be a good pre-requisite for the following documentation outlining use of the CURC JupyterHub.

#### Step 1: Log  in to CURC JupyterHub

CURC JupyterHub is available at [https://jupyter.rc.colorado.edu](https://jupyter.rc.colorado.edu). To log in use your RC credentials. If you do not have an RC account, please [request an account before continuing.](https://rcamp.rc.colorado.edu/accounts/account-request/create/organization)

#### Step 2: Start a notebook server

To start a notebook server, select the _Summit interactive (12hr)_ option in the *Select job profile* menu under *Spawner Options* and click *Spawn*. Available options are:

   * __Summit interactive (12hr)__ (a 12-hour, 1 core job on a Summit "shas" node)
   * __Summit Haswell (1 node, 12hr)__ (a 12-hour, 24 core job on a Summit "shas" node)
   * __Blanca (12hr)__ (A 12-hour, 1 core job on your default Blanca partition; only available to Blanca users)
   * __Blanca CSDMS (12hr)__ (A 12-hour, 1 core job on the Blanca CSDMS partition; only available to Blanca CSDMS users)

The server will take a few moments to start.  When it does, you will be taken to the Jupyter home screen, which will show the contents of your CURC `/home` directory in the left menu bar. In the main work area on the right hand side you will see the “Launcher” and any other tabs you may have open from previous sessions.

<p align="middle">
  <img src="https://raw.githubusercontent.com/ResearchComputing/Documentation/dev/docs/gateways/jupyterhub/jupyterlab1.png"/>
</p>

##### Default Features of the JupyterLab interface

* _Left sidebar:_ Click on a tab to change what you see in the left menu bar.  Options include the file browser, a list of running kernels and terminals, a command palette, a notebook cell tools inspector, and a tabs list.
* _Left menu bar:_ 
  * The _file browser_ will be active when you log in. 
    * You can navigate to your other CURC directories by clicking the folder next to `/home/<username>`. Your other CURC file systems are available too: `/projects/<username>`, `/pl/active` (for users with PetaLibrary allocations), `/scratch/summit/<username>` (Summit only), and `/rc_scratch/<username>` (Blanca only).
    * To open an existing notebook, just click on the notebook name in the file browser (e.g., _mynotebook.ipynb_).
    * Above your working directory contents are buttons to add a new Launcher, create a new folder, upload files from your local computer, and refresh the working directory. 
* _Main Work Area:_ Your workspaces will be in this large area on the right hand side. Under the "Launcher" tab you can: 
  * Open a new notebook with any of the kernels listed:
      * __Python 3 (idp)__: Python3 notebook (Intel Python distribution)
      * __Bash__: BASH notebook
      * __R__: R notebook 
      * ...and any other custom kernels you add on your own _(see the [section below](#creating-your-own-custom-jupyter-kernels) on creating your own custom kernels)._
   * Open a new console (command line) for any of the kernels.
   * Open other functions; the "Terminal" function is particularly useful, as it enables you to access the command line on the Summit or Blanca node your Jupyterhub job is currently running on. 
* See Jupyter's [documentation on the JupyterLab Interface for additional information.](https://jupyterlab.readthedocs.io/en/stable/user/interface.html)

#### Step 3: Open a notebook

There are two ways to open a notebook:
* _To open a new notebook_: In the _launcher_ click on the icon for the programming language or environment for you wish to use in the notebook (e.g., python, R, bash). Once you are in the notebook, you can save it to _myfilename_.ipynb using the _File -> Save as.._ option.
* To open an existing notebook: In the _file browser_ click on the _myfilename_.ipynb notebook that you want to work in.  This will open the notebook in the appropriate kernel (assuming that kernel is available on CURC Jupyterhub).

_Tip_: The ___Python 3___ notebook environment has many preinstalled packages. To query a list of available packages from a python notebook, you can use the following nomenclature:

```
from pip._internal import main as pipmain 
pipmain(['freeze'])
```

If the packages you need are not available, [you can create your own custom environment and Jupyter kernel](#Creating-your-own-custom-Jupyter-kernels).

#### Step 4: Shut down a Notebook Server

Go to the "File" menu at the top and choose "Hub Control Panel". Use the `Stop My Server` button in the `Control Panel` to shut down the Jupyter notebook server when finished (this cancels the job you are running on Summit or Blanca). You also have the option to restart a server if desired (for example, if you want to change from a "Summit Interactive" to a "Summit Haswell" server).

Alternately, you can use the `Quit` button from the Jupyter home page to shut down the Jupyter notebook server.

Using the `Logout` button will log you out of CURC JupyterHub.  It will not shut down your notebook server if one happens to be running.  

#### Creating your own custom Jupyter kernels

TThe CURC JupyterHub runs on top of the [CURC Anaconda distribution](https://curc.readthedocs.io/en/latest/software/python.html). [Anaconda](http://anaconda.com) is an open-source _python_ and _R_ distribution that uses the _conda_ package manager to easily install software and packages. Software and associated Jupyter [kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) other than _python_ and _R_ can also be installed using _conda_. The following steps describe how to create your own custom Jupyter kernels for use on RC JupyterHub. The _kernel_ is simply a file that provides a linkage between JupyterHub and a given conda environment.  We will create a Jupyter kernel for the `tutorial1` enviornment you created earlier. 

Follow these steps from a terminal session. You can get a new terminal session directly from Jupyter using `New`-> `Terminal`.

##### 1. Activate the environment you want to create a kernel for

```
[johndoe@shas0137 ~]$ source /curc/sw/anaconda/default
(base) [johndoe@shas0137 ~] $conda activate tutorial1
```

##### 2. Create your own custom kernel, which will enable you to use this environment in CURC Jupyterhub:

```
(tutorial1) [johndoe@shas0137 ~]$ python -m ipykernel install --user --name tutorial1 --display-name tutorial1
```

This command will create a kernel with the name _tutorial1_ and the Jupyter display name _tutorial1_ (note that the name and display-name are not required to match the environment name -- call them anything you want). By specifying the _`--user`_ flag, the kernel will be in _`/home/$USER/.local/share/jupyter/kernels`_ (a directory that is in the default __JUPYTER_PATH__) and will ensure your new kernel is available to you the next time you use CURC JupyterHub.


##### Notes
* Anaconda
  * CURC also hosts several python modules for those users who prefer modules over Anaconda. Type ```module spider python``` for a list of available python versions. Each module employs the Intel python distribution and has numerous pre-installed packages which can be queried by typing ```pip freeze```.
* Creating conda environments
  * You can create an environment in any directory location you prefer (as long as you have access to that directory).  We recommend using your _`/projects`_ directory because it is much larger than your _`/home`_ directory).
  * Although we don't show it here, it is expected that you will be installing whatever software and packages you need in this environment, as you normally would with conda).
  * We [strongly recommend] using the [Intel Python distribution](https://software.intel.com/en-us/distribution-for-python) (idp) if you will be doing any computationally-intensive work, or work that requires parallelization. The Intel Python distribution will run more efficiently on our Intel architecture than other python distributions.
* JupyterHub kernels
  * If you have already installed your own version of Anaconda or Miniconda, it is possible to create Jupyter kernels for your preexisting environments by following the steps above from within the active environment.  
  * If you need to use custom kernels that are in a location other than _`/home/$USER/.local/share/jupyter`_ (for example, if your research team has a group installation of Anaconda environments located in _`/pl/active/<some_env>`_), you can create a file in your home directory named _`~/.jupyterrc`_ containing the following line:

   ```export JUPYTER_PATH=/pl/active/<some_env>/share/jupyter```
  * If you need assistance creating or installing environments or Jupyter kernels, contact us at rc-help@colorado.edu. 
  
 ##### Troubleshooting 

* If you are having trouble loading a package, you can use `conda list` or `pip freeze` to list the available packages and their version numbers in your current conda environment. Use `conda install <packagname>` to add a new package or `conda install <packagename==version>` for a specific verison; e.g., `conda install numpy==1.16.2`.
* Sometimes conda environments can "break" if two packages in the environment require different versions of the same shared library.  In these cases you try a couple of things.
* Reinstall the packages all within the same _install_ command (e.g., `conda install <package1> <package2>`).  This forces conda to attempt to resolve shared library conflicts. 
* Create a new environment and reinstall the packages you need (preferably installing all with the same `conda install` command, rather than one-at-a-time, in order to resolve the conflicts).
  * _Troubleshooting_: Jupyter notebook servers spawned on RC compute resources log to _`~/.jupyterhub-spawner.log`_. Watching the contents of this file provides useful information regarding any problems encountered during notebook startup or execution.

### See Also

  * [CURC Anaconda distribution](https://curc.readthedocs.io/en/latest/software/python.html)
  * [CURC JupyterHub](https://curc.readthedocs.io/en/latest/gateways/jupyterhub.html) 
  * [Jupyter notebooks](https://jupyter.org)
