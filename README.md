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
## Fusing GPS : 
![](<https://camo.githubusercontent.com/a1106d70e4170dcb53a7de9039911cf33f9b0ca5213bb590d53b87abf7ce784f/68747470733a2f2f692e6962622e636f2f4373427953776b2f6e61767361742d7472616e73666f726d2e706e67>)

## Project 1: 
Using Rosbot From **Husarion** to fuse the Odometry data from wheel encoders and imu 

- First We make a workspace called **sensor_fusion_ws**

``mkdir sensor_fusion_ws``

then inside it src folder 

``cd sensor_fusion_ws``

``mkdir src``

then we make a pkg called sensor_fusion_pkg

``ros2 pkg create --build-tool ament_python sensor_fusion_pkg``

inside our pkg we will make a launch folder >>>> to launch the ekf_node 

and we will make a config folder to put inside it our yaml file

``cd sensor_fusion_pkg``

``mkdir launch``

``mkdir config``

``cd config``

``touch rosbot_ekf.yaml``

``cd launch``

``touch rosbot_launch.py``

``chmod +x rosbot_launch.py``

then copy and paste the codes : 

``

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
``

``

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
``

then we build the ws : 

``cd ``

``colcon build --symlink-install``



