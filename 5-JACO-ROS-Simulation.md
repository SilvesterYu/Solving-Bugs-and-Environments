# JACO-ROS-Simulation
ROS simulation preparation for JACO robot in Ubuntu 18.04

**IMPORTANT : IF YOU USE ZSH INSTEAD OF BASH, REPLACE ALL bashrc with zshrc**


**First ,get a clean install of Ubuntu 18.04**

# Install ROS

1. Get the key
```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

```bash
sudo apt install curl # if you haven't already installed curl^C
```

Add the ROS Key
```bash
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

Then update
```bash
sudo apt update
```

Install ROS melodic using
```bash
sudo apt install ros-melodic-desktop-full
```


Add path to `~/.bashrc` and source it

```bash
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc

```


Install essential packages

```bash
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
```

Install rosdep
```bash
sudo apt install python-rosdep

```


Initiate Rosdep and update

```bash
sudo rosdep init
rosdep update
```

Source the `~/.bashrc`  again
```bash
source ~/.bashrc
```

If you type 
```bash
roscore
```

and it is working, it means it is working.


# Set up ROS filesystem

Create catkin directory

```bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
```

Try building catkin workspace

```bash
catkin_make
```

Source the setup
```bash
source devel/setup.bash
```

Check if package path exists
```bash
echo $ROS_PACKAGE_PATH
```

# Set up JACO Robotic arm simulation

```bash
cd ~/catkin_ws/src
```

Clone the package
```bash
git clone https://github.com/Kinovarobotics/kinova-ros.git kinova-ros
sudo apt-get install ros-melodic-moveit*
```

```bash
cd ~/catkin_ws
```

Build the package
```bash
catkin_make
```

