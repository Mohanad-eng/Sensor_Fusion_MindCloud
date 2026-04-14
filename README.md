# Sensor_Fusion_MindCloud

## What is Sensor Fusion ?

![photo for sensr fusion](<img width="722" height="414" alt="image" src="https://github.com/user-attachments/assets/f275f807-f199-4168-8d75-325b35a13dd0" />)

**Sensor fusion** is Combining more than one reading from Different Sensors to :

1- remove the effect of the noise in the hardware.

2- increase the accuarcy of Readings for odometry.

3- Correcting Wheel Slip: Sometimes the robot’s wheels can slip, causing inaccurate data. By fusing data from both the wheels and the IMU, we can correct these errors and improve the robot’s navigation.

4- Improve reability(means we can be relabile on the readings)

5- Enhancing Navigation: Accurate data from sensor fusion allows the robot to navigate more efficiently, making fewer mistakes and taking more precise paths.

and more benefits for it.

## Let's dive in the Types of the sensor fusion types :

![fusion types](<https://app.dropinblog.com/uploaded/blogs/34241363/files/Types_of_Sensor_3.png>)

- **Low-Level Fusion (Early Fusion):**

  here we fuse the raw data from all sensors
  
- **Mid-Level Fusion:**

  here we fuse the detected data not the raw data only 

- **High-Level Fusion (Late Fusion):**

  here we fuse the data and and do tracking algorithms for each individual sensor, and then fuse the results.

## Extended Kalman Filters :
