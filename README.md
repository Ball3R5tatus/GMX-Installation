# GROMACS Installation with miniconda3 using WSL2

The objective is to harness the power of your GPU for molecular dynamics mining, enhancing computational efficiency and accelerating simulations.

## Description

Install GROMACS in MiniConda by creating a dedicated environment, ensuring compatibility, and leveraging informatics skills for molecular dynamic simulations. The installation process includes setting up WSL2, installing Ubuntu 22.04.3 LTS, MiniConda3, CUDATOOLKIT 12.3, and finally, configuring GROMACS with GPU support.

## Getting Started

### Dependencies

* WSL updated to version 2.0 - WSL version 1.0 will not let you use your GPU
* Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11
* GROMACS version: 2023.3-conda_forge
* Ubuntu 22.04.3 LTS
* MiniConda3
* CUDATOOLKIT 12.3

## Installation Steps

### WSL2 Installation

#### Personal Approach:

To install WSL2 and its components, enable "Virtual Machine Platform" and "Windows Subsystem for Linux" in the Windows Features window. Execute "optionalfeatures" with the "run" feature keyboard shortcut *WINDOWS+R.*

Install all necessary components for WSL using a single command. Open **PowerShell** or Windows Command Prompt as an **administrator**, right-click, choose **"Run as administrator,"** and enter the command:

```bash
wsl --install
```

OR

* **Enable WSL Feature:**

Open PowerShell as an administrator and run the following command to enable the WSL feature:

```bash
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```


* **Enable Virtual Machine Platform:**

Run the following command to enable the Virtual Machine Platform feature:

```bash
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
____________________________________________________________________________________

* **Set WSL Default Version:**

Set WSL to use version 2 as the default. Run the following command:

```bash
wsl --set-default-version 2
```


* **Install Ubuntu 22.04.3:**

Install Ubuntu 22.04.3 from the Microsoft Store. Search for "Ubuntu 22.04.3 LTS" and follow the installation prompts.
After installation, execute the following commands to update the system:

```bash
sudo apt update
sudo apt upgrade
```

* **Set Ubuntu Version to WSL2:**

Open PowerShell and set the Ubuntu instance to use WSL2:

```bash
wsl --set-version Ubuntu-22.04.3 2
```

 
* **Verify WSL Version:**

Confirm that the Ubuntu instance is using WSL2 by running:

```bash
wsl -l -v
```

You should see an entry for Ubuntu-22.04.3 with version 2.



## MiniConda3 Installation

Before downloading the file, make a directory (exemple: Desktop):
```bash
mkdir /mnt/c/Users/NAME/Desktop/miniconda3
bash /mnt/c/Users/NAME/Desktop/miniconda3/filename.sh
```

Download and install [MiniConda3](https://docs.conda.io/projects/miniconda/en/latest/) for Linux by following the instructions on the official Mini-conda website.



## CUDATOOLKIT 12.3 Installation
Follow the NVIDIA documentation to install [CUDATOOLKIT 12.3](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local) for Linux.

In WSL, you might need to manually add the CUDA bin directory to your PATH since the installation process might not do it automatically.

```
nano ~/.bashrc
```
Add the CUDA bin directory to your PATH. Append the following line to the end of the file:
```
export PATH=/usr/local/cuda/bin:$PATH
```
After saving the file, reload your shell configuration to apply the changes:
```
source ~/.bashrc
```
You can verify that nvcc is now available by running:
```
nvcc --version
```
This should display the version of CUDA installed on your system.

## GROMACS Installation
* **Create Conda Environment**
```
conda create --name gmx
conda activate gmx
```

* **Install GROMACS**
```bash
conda install -c conda-forge gromacs
```

* **Configure GROMACS with GPU Support**
```bash
Navigate to the GROMACS build directory:

cd path/to/gromacs/build
```

Use the following cmake command to enable GPU support:

```bash
cmake .. -DGMX_GPU=ON -DGMX_GPU=CUDA -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda -DCUDAToolkit_ROOT=/usr/local/cuda
make
make check
sudo make install
source /usr/local/gromacs/bin/GMXRC

gmx --version
```

**Here is the expected output related to your GROMACS version.**

![image](https://github.com/Ball3R5tatus/GMX-Installation/assets/122488999/40b5fa05-86cb-4529-809b-64f7037f4d1c)

## Increase Swap Space 
* **Swap Space** - Swap space is a crucial component that provides additional virtual memory when physical RAM is insufficient. While it helps prevent out-of-memory issues, excessive swapping can impact performance
* **Create a 300GB Swap File** - Run the following command to create a 300GB swap file and make it permanent. Please note that this process might take some time and adjust the size according to your requirement:

```bash
sudo dd if=/dev/zero of=~/swapfile bs=1G count=300
sudo chmod 600 ~/swapfile
sudo mkswap ~/swapfile
sudo swapon ~/swapfile
echo '~/swapfile swap swap defaults 0 0' | sudo tee -a /etc/fstab
```
This line ensures that your swap file is activated during the system startup, making the swap configuration persistent.

## Help


# command to run if the program contains helper info
help_command
Authors: Ball3R5tatus
Contributors: giaguaro


Version History
0.2


## Acknowledgments
