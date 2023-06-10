# ROS2-PX4 Installation and Simulation

## A. Installation

### 1. Install ROS2 Humble
```
cd
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null \
sudo apt update && sudo apt upgrade -y
sudo apt install ros-humble-desktop
sudo apt install ros-dev-tools
source /opt/ros/humble/setup.bash && echo "source /opt/ros/humble/setup.bash" >> .bashrc
```

### 2. Install PX4-Autopilot
```
cd
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
cd PX4-Autopilot/
make px4_sitl gz_x500
```

### 3. Setup Micro XRCE-DDS Agent & Client
```
cd
pip3 install --user -U empy pyros-genmsg setuptools
git clone https://github.com/eProsima/Micro-XRCE-DDS-Agent.git
cd Micro-XRCE-DDS-Agent
mkdir build
cd build
cmake ..
make
sudo make install
sudo ldconfig /usr/local/lib/
```

### 4. Download and Install QGC (Optional but Recommended)
```
cd
sudo usermod -a -G dialout $USER
sudo apt-get remove modemmanager -y
sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y
sudo apt install libqt5gui5 -y
sudo apt install libfuse2 -y
sudo apt install wget -y
wget https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.AppImage -P ~/QGroundControl.AppImage
chmod +x ./QGroundControl.AppImage
```

## B. Simulation

### 1. Open a new terminal and run MicroXRCEAgent:
```
MicroXRCEAgent udp4 -p 8888
```

### 2. In another terminal run QGC:(Only if installed)
```
~/QGroundControl.AppImage 
```

### 3. In another terminal start gz_x500 simulated drone:
```
cd ~/PX4-Autopilot && make px4_sitl gz_x500
```
