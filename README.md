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

## Extended Kalman Filters 🧮:

EKF is a mathematical algorithm that combines data from different sensors to figure out a robot’s position, which way it’s facing, and how fast it’s moving. It does this by constantly making educated guesses based on how the robot is expected to move, and then fine-tuning these guesses using the actual sensor readings. This helps to smooth out any noise or inaccuracies in the sensor data, giving us a cleaner and more reliable estimate of where the robot is and what it’s doing.

1- can handle noisy sensor data. It’s smart enough to trust the sensors that are more likely to be accurate, while still using data from the less precise sensors. This means that even if one sensor is a bit inaccurate, the EKF can still do a good job of locating the robot by relying more on the other sensors.

2- can keep working even if a sensor stops providing data for a little while. It does this by using its educated guesses and the last known sensor readings to keep tracking the robot. When the sensor starts working again, the EKF smoothly brings the new data back into the mix. 

**EKF Algorithm**

On a high-level, the EKF algorithm has two stages, a predict phase and an update (correction phase).

- Predict Step :
  
- Update (Correct) Step :
  



