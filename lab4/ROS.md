# ROS

## Ⅰ、Introduction

​	ROS (Robot Operating System) provides libraries and tools to help software developers create robot applications. It provides hardware abstraction, device drivers, libraries, visualizers, message-passing, package management, and more. ROS is licensed under an open source, BSD license.

## Ⅱ、Installation

> The version I am using is Ubuntu 15.10.

### 1)Configure your Ubuntu repositories

​	Configure your Ubuntu repositories to allow "restricted," "universe," and "multiverse." You can [follow the Ubuntu guide](https://help.ubuntu.com/community/Repositories/Ubuntu)for instructions on doing this. 

### 2)Setup your sources.list

​	Setup your computer to accept software from packages.ros.org. ROS Kinetic **ONLY** supports Wily (Ubuntu 15.10), Xenial (Ubuntu 16.04) and Jessie (Debian 8) for debian packages. 

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

### 3)Setup your keys

```
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
```

### 4)Installation

​	First, make sure your Debian package index is up-to-date: 

```
sudo apt-get update
```

​	There are many different libraries and tools in ROS. We provided four default configurations to get you started. You can also install ROS packages individually. And we choose the desktop-full.

In case of problems with the next step, you can use following repositories instead of the ones mentioned above [ros-shadow-fixed](http://wiki.ros.org/ShadowRepository)

- Desktop-Full Install: (Recommended)** : ROS, [rqt](http://wiki.ros.org/rqt), [rviz](http://wiki.ros.org/rviz), robot-generic libraries, 2D/3D simulators, navigation and 2D/3D perception 

  ```
  sudo apt-get install ros-kinetic-desktop-full
  ```

  To find available packages, use: 

  ```
  apt-cache search ros-kinetic
  ```

### 5)Initialize rosdep

​	Before you can use ROS, you will need to initialize rosdep. rosdep enables you to easily install system dependencies for source you want to compile and is required to run some core components in ROS. 

```
sudo rosdep init
rosdep update
```

### 6)Environment setup

​	It's convenient if the ROS environment variables are automatically added to your bash session every time a new shell is launched: 

```
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

​	If you have more than one ROS distribution installed, ~/.bashrc must only source the setup.bash for the version you are currently using.*

If you just want to change the environment of your current shell, instead of the above you can type: 

```
source /opt/ros/kinetic/setup.bash
```

### 7)Getting rosinstall

​	[rosinstall](http://wiki.ros.org/rosinstall) is a frequently used command-line tool in ROS that is distributed separately. It enables you to easily download many source trees for ROS packages with one command. 

To install this tool on Ubuntu, run: 

```
sudo apt-get install python-rosinstall
```

### 8)Build farm status

​	The packages that you installed were built by the [ROS build farm](http://build.ros.org/). You can check the status of individual packages [here](http://repositories.ros.org/status_page/ros_kinetic_default.html). 

## Ⅲ、Thought

​	这次的实验是根据教程进行ROS系统的配置，配置过程很顺利，除了下载速度较慢，几乎很顺利。当然，对此系统的使用则是更加重要的且艰难的过程，在下次上课前应多了解一下系统的应用及库的内容。