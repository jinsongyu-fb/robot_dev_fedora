# Robotics Development on Fedora

Since most open source libraries and tools for building robotics are developed and tested on Ubuntu, it may be challenging to install and use them on Fedora. We try to make it easier by providing verified instructions that make things work on Fedora.

The instructions are up-to-date as of December 2024, on Fedora 41.

## Fedora 41
You can verify which Fedora release you are on by 
```bash
cat /etc/fedora-release 
```
If you are on earlier version of Fedora and need to upgrade to 41:
```bash
sudo dnf upgrade --refresh
reboot

sudo dnf system-upgrade download --releasever=41
sudo dnf system-upgrade reboot
```

## ROS2 Jazzy
To install ROS2 Jazzy, basically follow [this instruction](https://docs.ros.org/en/jazzy/Installation/Alternatives/RHEL-Development-Setup.html) to install from source, but with some modifications (highlighted in bold below).

Prerequisite:
```bash
sudo dnf update
sudo dnf install -y \
    cmake \
    gcc-c++ \
    git \
    make \
    patch \
    python3-colcon-common-extensions \
    python3-mypy \
    python3-pip \
    python3-pydocstyle \
    python3-pytest \
    python3-pytest-cov \
    python3-pytest-mock \
    python3-pytest-repeat \
    python3-pytest-rerunfailures \
    python3-pytest-runner \
    python3-rosdep \
    python3-setuptools \
    python3-vcstool \
    wget

python3 -m pip install -U --user \
    flake8-blind-except==0.1.1 \
    flake8-class-newline \
    flake8-deprecated
```
<pre><b>pip3 install flake8-docstrings
</b></pre>

Getting source and install:
```bash
export ROBOT_ROOT=~/robot
export ROS2_ROOT=$ROBOT_ROOT/ros2_jazzy
mkdir -p $ROS2_ROOT/src
cd $ROS2_ROOT
vcs import --input https://raw.githubusercontent.com/ros2/ros2/jazzy/ros2.repos src

sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-6.0.1 urdfdom_headers"
```

<pre><b>export RPM_ARCH=x86_64
export RPM_PACKAGE_RELEASE=1.0
export RPM_PACKAGE_VERSION=1.0
export RPM_PACKAGE_NAME=qt_gui_cpp

colcon build
chmod u+x $ROS2_ROOT/install/setup.bash
</b></pre>

And then in each console session to use ROS2, run the following (you may want to consider adding this to your `~/.bashrc`)
```bash
export ROBOT_ROOT=~/robot
export ROS2_ROOT=$ROBOT_ROOT/ros2_jazzy
source $ROS2_ROOT/install/setup.bash
```
