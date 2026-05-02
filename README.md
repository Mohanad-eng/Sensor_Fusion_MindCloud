# Sensor_Fusion_MindCloud🧭
![](<https://images.squarespace-cdn.com/content/v1/6320a2f4b74abf777594e293/934a7d74-1f25-4155-b40d-4b82cec9964a/multi-sensor-fusion.jpg?format=original>)

## What is Sensor Fusion ?🛰

![photo for sensr fusion](<https://www.novelic.com/wp-content/uploads/2023/08/sensor-fusion.png>)

**Sensor fusion** is Combining more than one reading from Different Sensors to :

1- remove the effect of the noise in the hardware.

2- increase the accuarcy of Readings for odometry.

3- Correcting Wheel Slip: Sometimes the robot’s wheels can slip, causing inaccurate data. By fusing data from both the wheels and the IMU, we can correct these errors and improve the robot’s navigation.

4- Improve reability(means we can be relabile on the readings)

5- Enhancing Navigation: Accurate data from sensor fusion allows the robot to navigate more efficiently, making fewer mistakes and taking more precise paths.

and more benefits for it.

## ✔️Let's dive in the Types of the sensor fusion:

![fusion types](<https://app.dropinblog.com/uploaded/blogs/34241363/files/Types_of_Sensor_3.png>)

- **Low-Level Fusion (Early Fusion):**

  here we fuse the raw data from all sensors
  
- **Mid-Level Fusion:**

  here we fuse the detected data not the raw data only 

- **High-Level Fusion (Late Fusion):**

  here we fuse the data and and do tracking algorithms for each individual sensor, and then fuse the results.

## Filter Types 📌:

![](<https://miro.medium.com/v2/resize:fit:720/format:webp/1*O_DUzPkZyrMovzD7cf8wlg.png>)

## Extended Kalman Filters 🧮:

EKF is a mathematical algorithm that combines data from different sensors to figure out a robot’s position, which way it’s facing, and how fast it’s moving. It does this by constantly making educated guesses based on how the robot is expected to move, and then fine-tuning these guesses using the actual sensor readings. This helps to smooth out any noise or inaccuracies in the sensor data, giving us a cleaner and more reliable estimate of where the robot is and what it’s doing.

1- can handle noisy sensor data. It’s smart enough to trust the sensors that are more likely to be accurate, while still using data from the less precise sensors. This means that even if one sensor is a bit inaccurate, the EKF can still do a good job of locating the robot by relying more on the other sensors.

2- can keep working even if a sensor stops providing data for a little while. It does this by using its educated guesses and the last known sensor readings to keep tracking the robot. When the sensor starts working again, the EKF smoothly brings the new data back into the mix. 

**EKF Algorithm**

On a high-level, the EKF algorithm has two stages, a predict phase and an update (correction phase).

- Predict Step :
  
  Using the state space model of the robotics system, predict the state estimate at time t based on the state estimate at time t-1 and the control input applied at time t-1.
    
  Predict the state covariance estimate based on the previous covariance and some noise.
  
- Update (Correct) Step :

  Calculate the difference between the actual sensor measurements at time t minus what the measurement model predicted the sensor measurements would be for the current timestep t.
  
  Calculate the measurement residual covariance.
  
  Calculate the near-optimal Kalman gain.
  
  Calculate an updated state estimate for time t.
  
  Update the state covariance estimate for time t.

**Robot_localization Pkg 💻**

Extended Kalman Filter(EKF) was developed. This article coherently explains how it works. You need basic knowledge of linear algebra, and statistics specially gaussian distribution to understand the theory. Now, implementing this EKF could be laborious.

This package is the implemented version of the EKF in ROS. All you need is to install it and edit some parameters and you are good to go without going through the mathematics and programming part.

we must define “odom_frame”, “base_link_frame”, and “world_frame” which will be like “odom_frame”. We define “publish_tf” as true, which means, the package will publish the “odom” → “base_link” transformation.

We also need to define which data we want to fuse. You can see from the above image, here we are fusing one odometry message (odom0: topic name) and one IMU message(imu0: topic name). In the odom0_config and imu0_config, there are 15 elements. Each should be either true or false. These elements are: (X, Y,Z, roll, pitch,yaw, X_velocity,Y_velocity,Z_velocity, roll_velocity,pitch_velocity,yaw_velocity, X_acceleration,Y_acceleration,Z_acceleration). Setting one entry to true means we are fusing that data. Be sure to modify them according to your sensor. The accuracy of the fusion model highly depends on the fusion configuration.

Once you run the simulation and our package, you will see the non-filtered and filtered odometry in the RVIZ window. To print the filtered odometry message, run “rostopic echo /odometry/filtered” in the command terminal and you will see the better odometry messages.

## What is a  Covariance Matrix :

A covariance matrix describes how uncertain a sensor is about its measurements, and how those uncertainties relate to each other. Think of it as a "confidence score" but in matrix form.

![](<img width="1440" height="1040" alt="image" src="https://github.com/user-attachments/assets/2ad7b932-a2fa-4008-8728-b64378fe1eb4" />)

In plain English: if your GPS has σ²x = 0.5, it means the x-position error has a standard deviation of √0.5 ≈ 0.7 meters. A value of 9.0 means you're about 3 meters off — less trustworthy.

## Fusing GPS or GNSS (Global Navigation Satellite System)  : 

This is a Digram that shows how the Navsat transform that convert the readings of the GPS to an odometry readings on **/odometry/gps** topic
![](<https://camo.githubusercontent.com/a1106d70e4170dcb53a7de9039911cf33f9b0ca5213bb590d53b87abf7ce784f/68747470733a2f2f692e6962622e636f2f4373427953776b2f6e61767361742d7472616e73666f726d2e706e67>)

## Explaining Important Things :

**navsat_transform_node** needs the IMU for one specific reason — the GPS gives x/y position but has no idea which direction the rover is facing. The IMU provides the heading (yaw from qz), and navsat uses it to rotate the GPS position into the correct orientation before it can be expressed in the odom frame. Without the IMU heading, navsat cannot initialize.

- Messege of **gps/fix** :
  
![](<Screenshot from 2026-05-02 16-14-14.png>)

Let's Explain more :

## Project 1: 
Using Rosbot From **Husarion** to fuse the Odometry data from wheel encoders and imu 

- First We make a workspace called **sensor_fusion_ws**

``mkdir sensor_fusion_ws``

- then inside it src folder 

``cd sensor_fusion_ws``

``mkdir src``

- then we make a pkg called sensor_fusion_pkg

``ros2 pkg create --build-tool ament_python sensor_fusion_pkg``

- inside our pkg we will make a launch folder >>>> to launch the ekf_node 

and we will make a config folder to put inside it our yaml file

``cd sensor_fusion_pkg``

``mkdir launch``

``mkdir config``

``cd config``

``touch rosbot_ekf.yaml``

``cd launch``

``touch rosbot_launch.py``

``chmod +x rosbot_launch.py``

- then copy and paste the codes : 

in the **rosbot_launch.py** :

```
import os
from launch import LaunchDescription
from launch_ros.actions import Node
from ament_index_python.packages import get_package_share_directory

def generate_launch_description():

    ekf_config = os.path.join(
        get_package_share_directory('sensor_fusion_pkg'),
        'config',
        'rosbot_ekf.yaml'
    )

    return LaunchDescription([

        Node(
            package='robot_localization',
            executable='ekf_node',
            name='ekf_filter_node',
            output='screen',
            parameters=[ekf_config],
        ),

    ])
```
in the **rosbot_ekf.yaml**:

```
ekf_filter_node:
  ros__parameters:

    frequency: 30.0
    two_d_mode: true
    publish_tf: true
    use_sim_time: true

    map_frame: map
    odom_frame: odom
    base_link_frame: base_link
    world_frame: odom

    # -------------------
    # ODOM
    # -------------------
    odom0: /rosbot_xl_base_controller/odom
    odom0_config: [
      true, true, false,
      false, false, true,
      true, false, false,
      false, false, true,
      false, false, false
    ]

    # -------------------
    # IMU
    # -------------------
    imu0: /imu_broadcaster/imu
    imu0_config: [
      false, false, false,
      true, true, true,
      false, false, false,
      true, true, true,
      true, true, true
    ]

    sensor_timeout: 1.0
    
```

- then we build the ws : 

``cd sensor_fusion_ws``

``colcon build --symlink-install``

- Then we run the project :

1- run the webots simulation if the rosbot :

``ros2 launch webots_ros2_husarion rosbot_xl_launch.py``

> ![Photo From Rviz2 Simulation](<Screenshot from 2026-04-19 01-13-54.png>)

2- Then in another Terminal run the launch file we made to run the config file and the robot_localization pkg :

`` ros2 launch sensor_fusion_pkg rosbot_launch.py``

> ![](<>)

3- Then in a third terminal run the teleopkey (we make it stampted because webots use stamped) :

``ros2 run teleop_twist_keyboard teleop_twist_keyboard   --ros-args -p stamped:=true``

4- then open rviz and visualize the odometry in it :

``rviz2``

> How to setup Rviz for simulation :
  1-
> 
  2-
  
  3- 

  4- 
  
  > here is a photo for the simualtion :

  ![](<>)


- The demo video : 


