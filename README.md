# SITL documentation
[You can find the ardupilot SITL documentation here](https://ardupilot.org/dev/docs/sitl-simulator-software-in-the-loop.html)

# ArduPilot SITL Setup Guide

This guide provides the steps to setup the ArduPilot's Software in the Loop (SITL) environment on Ubuntu Linux.

## Prerequisites

Before you begin, make sure your Linux distribution is up-to-date and has \`git\` and \`python3\` installed. 

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git python3 python3-dev python3-pip
pip3 install future
```

## Step 1: Clone the ArduPilot repository

```bash
cd ~/
git clone https://github.com/ArduPilot/ardupilot.git --depth 1
cd ardupilot
```

## Step 2: Update submodules

```bash
git submodule update --init --recursive
```

## Step 3: Run the Setup Script

Navigate to the directory where the setup scripts reside and run the appropriate setup script for your environment. For Ubuntu, run:

```bash
cd Tools/environment_install
./install-prereqs-ubuntu.sh -y
```

When the script finishes execution, reload the path:

```bash
. ~/.profile
```

## Step 4: Add the \`Tools/autotest\` directory to your PATH

```bash
echo "export PATH=$PATH:$HOME/ardupilot/Tools/autotest" >> ~/.zshrc
source ~/.zshrc
```

## Step 5: Setup the sim for first use
The ardupilot instructions suggest running the sim with the -w flag on first execution to load all default values. Once everything is loaded, you can exit with `ctrl+c`.
Make sure to run this from the `ArduSub` folder
```bash
cd ~/ardupilot/ArduSub/
sim_vehicle.py -w
```

Note: sim_vehicle.py is located in the ardupilot/Tools/autotest folder

## Step 6: Run SITL simulation with QGroundcontrol

### Find your laptop hostname or ip address
The simulation will be running on your Raspberry Pi, but QGroundcontrol needs to be launched from your laptop.
For the sim to be able to find your laptop, you'll have to specify either the name of your laptop, or the ip address of your laptop.
- [Click here](https://support.apple.com/guide/mac-help/find-your-computers-name-and-network-address-mchlp1177/mac) for instructions on **MAC**
- [Click here](https://www.montana.edu/uit/ip/find-info-win.html) for **Windows**
- For **Linux** simply type `hostname` in your terminal, and the hostname will be returned

### Launch the simulator

In the command below, make sure to change the the `<hostname>` section with the correct name or ip.

```bash
cd ArduSub
sim_vehicle.py --vehicle=ArduSub --aircraft="bwsibot" -L RATBeach --out=udp:<hostname>:14550
```

### Finally, launch QGroundcontrol
If you haven't done so yet, you can download it from the [QGroundControl's official download page](https://docs.qgroundcontrol.com/master/en/getting_started/download_and_install.html).

## Optional: running SITL with the 3D bluesim simulator

Download the correct BlueSim client for your system from the [latest releases](https://github.com/bluerobotics/bluesim/releases/tag/latest) on their GitHub page. 

Once the download is complete, extract the files and navigate to the extracted directory to execute the file.

### Step 6a: Run SITL compatible with BlueSim

You're now ready to run SITL. For example, to simulate our ROV, we will use the following commands:

```bash
cd ~/ardupilot/ArduSub
sim_vehicle.py --vehicle=ArduSub --frame=json --aircraft="bwsibot" -L RATBeach --out=udp:0.0.0.0:14550
```
