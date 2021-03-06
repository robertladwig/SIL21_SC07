# 35th Congress of the International Society of Limnology: SC-07 Aquatic Ecosystem Modeling (PCLake and GLM-AED2)
<a href="url"><img src="sil21.png" align="right" height="75" width="125" ></a>
-----

:busts_in_silhouette: Annette Janssen, Manqi Chang, Austin Delany, Robert Ladwig
:computer: [Material](https://github.com/robertladwig/SIL21_SC07)  
:email: [Questions about PCLake?](mailto:annette.janssen@wur.nl), [Questions about GLM?](mailto:rladwig2@wisc.edu)
:teacher: [Introduction slides and GLM-AED2](https://docs.google.com/presentation/d/1zqoOMVjak10cjrNXVYTRbMz5clcjO13f2dh_fM475v8/edit?usp=sharing), [PCLake slides](https://docs.google.com/presentation/d/1L7YfoevXEjjw8QKtXZt4gzbZLOtHOVj21FuAwA_gEf8/edit?usp=sharing)

-----

## Description

Aquatic ecosystem models are representations of real-world processes in aquatic ecosystems. These models provide the opportunity to explore scientific and engineering ideas or scenarios, and increase our scientific understanding of aquatic systems. Model simulations can also be used as experiments that are not possible to field-test for physical, logistical, political or financial reasons.
In this short course, we will provide a general introduction about numerical modeling. To illustrate numerical modelling we will present two lake ecosystem models with different methodological approaches: the one-dimensional ecosystem model PCLake and the coupled vertical one-dimensional hydrodynamic-ecological model GLM-AED2.

The aim of this course is to give a general introduction about numerical modeling of aquatic ecosystems. In the introduction, we will focus on modeling basics (mathematical description of hydrodynamics and ecological processes, features of both models). Afterwards, we will split the group into two groups that work on hands-on exercises with PCLake or GLM-AED2, respectively.

## What will this workshop material cover?
  - Introduction: to process-based modeling intended for all skill levels and background
  - Overview and hands-on coding with state-of-the-art models: vertical 1D hydrodynamic-water quality model (GLM-AED2) or aquatic ecosystem and food web model (PCLake)

## Prerequisites

### PCLake
For the hands-on exercises, we kindly ask you to bring your own computer with the following prerequisites: Windows and Microsoft Excel. Moreover, we ask you to install the software before the course. For that you can go to the folder [this site](https://github.com/robertladwig/SIL21_SC07/blob/main/PCLake_files/PreparationDocuments_PCLake/Install_PCLake_and_Rtools.pdf) to read the instructions. Also we ask you to read the articles in [the folder](https://github.com/robertladwig/SIL21_SC07/tree/main/PCLake_files/PreparationDocuments_PCLake) as preparation for the PCLake demonstration.

Please note that during the course we will use a version of PCLake that is specifically develloped for this course. If you want to use PCLake in your future research we refer to https://github.com/pcmodel.

### GLM-AED2
You find all material here: [GLM_workshop repository](https://github.com/robertladwig/GLM_workshop)

#### 1. Use Docker
   To be sure that all the examples will *work* during the workshop, you can use a [container](https://hub.docker.com/r/hydrobert/glm-workshop) of all the material. I'll quote the Docker website here:

   > "A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings."

   You can install the Docker software from [here](https://docs.docker.com/get-docker/):

   - For Windows users (especially Windows 10 Home), please read the installation instructions on [this site](https://docs.docker.com/docker-for-windows/install-windows-home/). You will need to enable WSL 2 features as described [here](https://docs.microsoft.com/en-us/windows/wsl/install-win10) and the whole setup can take a while.
   - For Mac users, the installation is pretty and straightforward, please take a look at [this material](https://docs.docker.com/docker-for-mac/install/).
   - You will find an overview of docker installation instructions for most Linux distributions [here](https://docs.docker.com/engine/install/).

     Once installed and started, you'll need to open a terminal and type (the pulling will take some time depending on your internet connection, it's 3.87 Gb big)


    docker pull hydrobert/glm-workshop
    docker run --rm -d  -p 8000:8000 -e ROOT=TRUE -e PASSWORD=password hydrobert/glm-workshop:latest


   Then, open any web browser and type ???localhost:8000??? and input user: rstudio, and password: password. Rstudio will open up with the script and data available in the file window.

  If you wish to save files on your local computer (everything will disappear once you close the container), you can also run


    docker run --rm -d  -p 8000:8000 -e ROOT=TRUE -e PASSWORD=password -v [LOCAL PATH]:/home/rstudio/workshop/local hydrobert/glm-workshop:latest


   where [LOCAL PATH] would be an existing directory on your machine (e.g., /home/user/docs/glm_workshop_example). Inside the Docker's Rstudio you can then move and copy files to /local to save them on your computer.

   After you have finished the workshop examples, you can close the docker application by running


    docker kill $(docker ps -q)
    docker rm $(docker ps -a -q)


   If you want to deinstall the docker after the workshop, check the docker IMAGE ID by typing:


    docker images -a


   and remove the container by exchanging "IMAGE ID" with the actual one next to your "hydrobert/glm-workshop" container:


    docker rmi "IMAGE ID"

  Here are some more helpful instructions on how to use [docker](https://docs.google.com/document/d/1uxw5aa1gsMpvCBpsGZlaQOkBELR1MJmBQzu4vEKYBoY/edit?usp=sharing).

#### 2. Use Github and your local R setup
   Alternatively, you can clone or download files from this [Github repository](https://github.com/robertladwig/GLM_workshop) (click the green "Code" button and select the "Clone" or "Download ZIP" option).
  You???ll need R (version >= 4.x), preferably a GUI of your choice (e.g., Rstudio) and these packages:
  ```
  require(devtools)
  devtools::install_github("robertladwig/GLM3r", ref = "v3.1.1")
  devtools::install_github("hdugan/glmtools", ref = "ggplot_overhaul")
  install.packages("rLakeAnalyzer")
  install.packages("tidyverse")
  ```
Update: If the GLM3r installation does not work for you and you're experiencing problems when running ```run_glm()```, then you can try installing:

  ```
  # macOS/Linux
  devtools::install_github("GLEON/GLM3r", ref = "GLMv.3.1.0a3")
  # Windows:
  devtools::install_github("GLEON/GLM3r")
  ```

Windows users will then run v3.1.0a4 whereas Unix users use v3.1.0b1. Unfortunately, some differences between these versions can occur in the model outputs. We are still working on the GLM3r and glmtools packages to keep them updated with new GLM-AED2 releases and to implement new features for model evaluation. This Windows binary sometimes freezes, which can stop the calibration routine. If this happens, please 'stop' the command and re-run it. If you experience problems on macOS (we tested the package only for macOS Catalina) with error messages like 'dyld: Library not loaded', you can also try the following approaches:

   - use and try ``` devtools::install_github("robertladwig/GLM3r", ref = "v3.1.0a3") ``` to install GLM3r
   - or install the missing libraries, e.g. by using ['brew'](https://brew.sh): ``` brew install gcc ```, ``` brew install netcdf```, ``` brew install gc```; afterwards you should install this GLM3r version: ```devtools::install_github("robertladwig/GLM3r", ref = "v3.1.0a3-2")``` (we are working on fixing all these macOS-specific problems)

-----
