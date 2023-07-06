# ArduPilot SITL Setup Guide

This guide provides the steps to setup the ArduPilot's Software in the Loop (SITL) environment on Ubuntu Linux.

## Prerequisites

Before you begin, make sure your Linux distribution is up-to-date and has \`git\` and \`python3\` installed. 

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git python3 python3-dev python3-opencv python3-wxgtk4.0 python3-pip python3-matplotlib python3-lxml python3-yaml
pip3 install future
```

## Step 1: Clone the ArduPilot repository

```bash
git clone https://github.com/ArduPilot/ardupilot.git
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
echo "export PATH=$PATH:$HOME/ardupilot/Tools/autotest" >> ~/.bashrc
source ~/.bashrc
```

## Step 5a: Run SITL compatible with BlueSim

You're now ready to run SITL. For example, to simulate an ArduSub, you can use the following commands:

```bash
cd ArduSub
sim_vehicle.py --vehicle=ArduSub --frame=json --aircraft="bwsibot" -L RATBeach --out=udp:0.0.0.0:14550
```

### Next, launch QGroundcontrol
You can download it from the [QGroundControl's official download page](https://docs.qgroundcontrol.com/master/en/getting_started/download_and_install.html).

### Finally, Download and Run BlueSim

Download the correct BlueSim client for your system from the [latest releases](https://github.com/bluerobotics/bluesim/releases/tag/latest) on their GitHub page. 

Once the download is complete, extract the files and navigate to the extracted directory to execute the file.

## Step 5b: Run SITL standalone with QGroundcontrol

In order to run the sim standalone with QGroundcontrol, the frame needs to be changed to vector. (I am not sure why)
It will now be controllable from QGroundcontrol, but not connected to bluesim at all.

```bash
cd ArduSub
sim_vehicle.py --vehicle=ArduSub --frame=vector --aircraft="bwsibot" -L RATBeach --out=udp:0.0.0.0:14550
```
