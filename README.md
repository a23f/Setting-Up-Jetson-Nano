# Setting Up Jetson Nano

## Hardware Required
1. Nvidia Jetson Nano Developer Kit
2. Micro SD Card (minimum recommended is a 32GB UHS-1 card)
3. Monitor & HDMI Cable
4. Keyboard & Mouse
5. Wireless Network (Wifi Adapter/Ethernet)
6. Standard USB Webcam
7. Micro SD Card Reader (for flashing purposes)

## Setting Up the Jetson Nano
1. Format the Micro SD Card using `SDFormatter` software
   - SDFormatter is special software that can be used to provide formatting functions to different types of SD memory cards. This includes SD, SDHC and SDXC high capacity cards. The software has been configured to work only with SD cards and provide the best formatting experience.
   - click this link to download: https://www.download3k.com/DownloadLink3-SDFormatter.html
2. Download the Jetson Nano Developer Kit SD Card Image
   - This SD card image works for all Jetson Nano Developer Kits (both 945-13450-0000-100 and the older 945-13450-0000-000) and is built with `JetPack 4.6.1`.
   - click this link to download: https://developer.nvidia.com/embedded/l4t/r32_release_v7.1/jp_4.6.1_b110_sd_card/jeston_nano/jetson-nano-jp461-sd-card-image.zip
3. Flash Jetson Nano Image onto the micro SD card using `balenaEtcher` software
   - click this link to download (Windows): https://github.com/balena-io/etcher/releases/download/v1.7.9/balenaEtcher-Portable-1.7.9.exe?d_id=42c8edb5-88af-48f7-9280-3c07bf4f1250R
   - click this link to download (macOS): https://github.com/balena-io/etcher/releases/download/v1.7.9/balenaEtcher-1.7.9.dmg?d_id=42c8edb5-88af-48f7-9280-3c07bf4f1250R
4. Boot Jetson Nano
   - Insert the Micro SD Card into the Jetson Nano (the Micro SD Card slot is located under the module)
   - Connect the monitor, keyboard, and mouse to the Jetson Nano
   - Power on the Jetson Nano by connecting the micro USB (for Jetson Nano (4GB)) or USB-C (for Jetson Nano 2GB) charger to the port.

## Update, Upgrade, and Install important packages on Jetson Nano
1. Open the Terminal on Jetson Nano
2. To update the packages, run `sudo apt-get update`
   - Update is used to resynchronize the package index files from their sources. The indexes of available packages are fetched from the location(s) specified in /etc/apt/sources.list.
   - In short, executing this command fetches a list of packages for all of the repositories and PPA’s and make sure it is up to date.
3. To upgrade the packages, run `sudo apt-get upgrade`
   - Upgrade is used to install the newest versions of all packages currently installed on the system from the sources enumerated in /etc/apt/sources.list. Packages currently installed with new versions available are retrieved and upgraded; under no circumstances are currently installed packages removed, or packages not already installed retrieved and installed.
   - An update must be performed first so that apt-get knows that new versions of packages are available.
4. Install some important python packages, run `sudo apt install python3-h5py libhdf5-serial-dev hdf5-tools python3-matplotlib python3-pip libopenblas-base libopenmpi-dev`

## Swap Memory
- The Jetson Nano by default has 2GB of swap memory. The swap memory allows for “extra memory” when there is memory pressure on main (physical) memory by swapping portions of memory to disk. Because the Jetson Nano has a relatively small amount of memory (4GB) this can be very useful, especially when compiling large projects.
- Run `zramctl` to examine the swap memory information. You will notice that there are four entries (one for each CPU of the Jetson Nano) /dev/zram0 – /dev/zram3. Each device has an allocated amount of swap memory associated with it, by default 494.6M, for a total of around 2GB. This is half the size of the main memory.
- On the JetsonHacksNano account on Github, there is a repository named resizeSwapMemory. The repository contains a convenience script to set the size of the swap memory. Run the following codes to download the repository and set the entire swap memory size to 4GB.
  - `git clone https://github.com/JetsonHacksNano/resizeSwapMemory`
  - `cd resizeSwapMemory`
  - `./setSwapMemorySize.sh -g 4`
- This will modify the /etc/systemd/nvzramconfig.sh to set the requested memory for the swap memory as specified. You will need to reboot for the change to take effect.

## Required Installation
1. Install Anaconda
   - Jetson Nano’s linux system runs in AArch64 architecture so installing the regular Anaconda does not work. One of the options is by installing Archiconda instead. Run the following codes to download and install it.
     - `wget https://github.com/Archiconda/build-tools/releases/download/0.2.3/Archiconda3-0.2.3-Linux-aarch64.sh`
     - `sudo sh Archiconda3-0.2.3-Linux-aarch64.sh`
   - Run the following codes to give root permission to it.
     - `sudo chmod 777 -R ~/archiconda3/`
     - `sudo chmod 777 -R ~/.conda/`
   - Run the following codes to create an environment inside conda with Python v3.8 (change 'myenv' to your environment name) then activate the environment.
     - `conda create -name myenv python=3.8`
     - `conda activate myenv`
2. Install PyTorch
   - PyTorch is a Python package that provides two high-level features:
     - Tensor computation (like NumPy) with strong GPU acceleration
     - Deep neural networks built on a tape-based autograd system
   - Run the following codes to download and install PyTorch.
     - `git clone --recursive --branch 1.7 http://github.com/pytorch/pytorch`
     - `cd pytorch`
     - `python3.8 -m pip install -r requirements.txt`
     - `python3.8 setup.py install`
3. Install torchvision
   - The torchvision package consists of popular datasets, model architectures, and common image transformations for computer vision.
   - Run the following codes to download and install torchvision
     - `git clone https://github.com/pytorch/vision`
     - `cd vision`
     - `git checkout v0.8.0`
     - `python3 setup.py bdist_wheel`
     - `python3 -m pip install dist/torchvision-0.8.0a0+291f7e2-cp38-cp38-linux_aarch64.whl`
4. Install YoloV5
   - Clone the repository
     - `git clone https://github.com/ultralytics/yolov5`
   - Modify requirements.txt
     - Locate and open YoloV5 directory
     - Open requirements.txt using text editor
     - Comment #torch>=1.7.0 #torchvision>=0.8.1
     - Save
   - Install requirements.txt
     - `cd yolov5`
     - `pip install -r requirements.txt`

## Check whether PyTorch recognize GPU on Jetson Nano
- Run the following codes.
  - `cd ~`
  - `python3`
  - `import torch`
  - `torch.cuda.is_available()`
- The codes should return 'True' as an output if PyTorch recognize GPU on Jetson Nano.

## We're good to go!
