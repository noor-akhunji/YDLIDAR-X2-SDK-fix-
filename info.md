✅ YDLIDAR X2 + Raspberry Pi + ROS 2 Jazzy — FULL WORKING SETUP

🔌 PART 1 — Hardware wiring (MOST IMPORTANT)

X2 → CP2102 USB-TTL
X2 5V   → CP2102 +5V
X2 GND  → CP2102 GND
X2 TX   → CP2102 RXD
X2 RX   → CP2102 TXD

⚠ TX/RX crossed

⚠ Do NOT use 3.3V

👉 When 5V + GND connected → LiDAR MUST spin

📦 PART 2 — Install tools

sudo apt update
sudo apt install git cmake build-essential python3-colcon-common-extensions

📚 PART 3 — Install YDLIDAR SDK

git clone https://github.com/KarthikeshJG/YDLidar-SDK.git
cd YDLidar-SDK
mkdir build && cd build
cmake ..
make
sudo make install

📂 PART 4 — Create ROS2 workspace

mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src

📡 PART 5 — Clone driver

git clone https://github.com/KarthikeshJG/ydlidar_ros2_driver.git

⚠ PART 6 — SELECT X2 PROFILE (CRITICAL FIX)

nano ~/ros2_ws/src/ydlidar_ros2_driver/launch/ydlidar_launch_view.py
Change:
share_dir, 'params', 'X4.yaml'
To:
share_dir, 'params', 'X2.yaml'
Save & exit.

🔧 PART 7 — Set correct serial port

ls /dev/ttyUSB*
(usually /dev/ttyUSB0)
nano ~/ros2_ws/src/ydlidar_ros2_driver/params/X2.yaml
Make sure:
port: /dev/ttyUSB0
baudrate: 115200

🔨 PART 8 — Build

cd ~/ros2_ws
colcon build

🔁 PART 9 — Clean ROS environment (recommended)

Edit bashrc:
nano ~/.bashrc
Keep ONLY:
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash
export CMAKE_PREFIX_PATH=$CMAKE_PREFIX_PATH:$HOME/YDLidar-SDK/build
Reload:
source ~/.bashrc

🔐 PART 10 — Fix serial permission (once)

sudo usermod -a -G dialout $USER
newgrp dialout
Unplug & replug LiDAR.

▶ PART 11 — Run LiDAR

ros2 launch ydlidar_ros2_driver ydlidar_launch_view.py
🎯 SUCCESS OUTPUT
Lidar successfully connected [/dev/ttyUSB0:115200]
and RViz shows red scan dots.

📊 PART 12 — Verify data

ros2 topic echo /scan
Streaming numbers = working LiDAR ✅

🗺 (Optional next step) SLAM mapping

sudo apt install ros-jazzy-slam-toolbox
ros2 launch slam_toolbox online_async_launch.py

✅ FINAL STATUS CHECK
Item	Result
LiDAR spins	✅
USB detected	✅
ROS driver runs	✅
Scan visible	✅
SLAM ready	✅

🧠 QUICK TROUBLESHOOT

Problem	Fix
No spin	wrong 5V/GND
Package not found	source workspace
Serial error	dialout group
No dots	wrong X2 profile

🎉 You now have a professional LiDAR ROS2 setup on Raspberry Pi.

If you’d like next I can help you:
🗺 Save maps
🤖 Add motors later
📍 Build navigation robot
