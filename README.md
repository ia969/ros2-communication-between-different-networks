# Remote Communication between Two ROS2 Devices Connected over Different Networks (tested on Ubuntu 18.04 and ROS2 dashing)

## Introduction

In order to enable ROS2 communication over two different internet networks you must need to have the **Public IP** and enable the **Port Forward** on the host pc.

This documentation provides a step-by-step guide for enabling remote communication between two ROS2 devices connected over different networks using [Zenoh](https://zenoh.io/blog/2021-04-28-ros2-integration/). The process involves downloading Zenoh on both devices, setting up routers (obtaining public ip and opening port) for host PC, and running ROS packages with Zenoh plugins.


Step 1: Clone respository, need to be done on both devices **Host** and **Client**
```
clone the repositry using git clone
cargo build --release -p zplugin-dds
cargo build --release -p zenoh-bridge-dds
rosdep install --from-paths . --ignore-src -r -y
colcon build --packages-select zenoh_bridge_dds --cmake-args -DCMAKE_BUILD_TYPE=Release
```
For more details are updated version, refer to Zenoh Plugin DDS GitHub - [Zenoh DDS for ROS 2](https://zenoh.io/blog/2021-04-28-ros2-integration/)

Step 2: Set Up Routers, Need to done only for the router connected with Host  _**Note: We used Fritz box router and telekom network**_
    
Connect to the FritzBox router and log in to the GUI.
Navigate to Internet > Account Information > Settings.
write any name for "Internet service provider."
Enter "internet.t-d1.de" in the "APN" text box.
Press Apply and wait for the public IP.

Now, Go to Internet > Permit Access > Port Sharing.  Set port forwarding for **7440**
    
Step 3: Run Zenoh Plugins and ros exectuable.    
_For Host PC_
    
    ./target/release/zenoh-bridge-dds -l tcp/0.0.0.0:7448
    ros2 run turtlesim turtlesim_node
_For Client PC_
    
    ./target/release/zenoh-bridge-dds -e tcp/<Public Ip of host pc or DNS>:7440
    ros2 run turtlesim turtle_teleop_key 

