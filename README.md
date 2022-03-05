# Zebra Take Home Coding Challenge
This is the take-home assignment for the Simulation Software Engineering Position (Robotics) at Zebra Technologies

# Software used
- ROS Kinetic
- Ubuntu 16.04
- Gazebo 7
- RVIZ

# Task 1
Build a simulated warehouse using gazebo (version 7 or higher) or using ROSDS. Add 9 blue shelves that are 10 meter x 2 meter x 2 meter. Be sure to include a floor and some lights. Warehouse dimensions 40 meter x 20 meter.

1. Provide (2) pictures of the map: one from above and one isometric view: Top view (./warehouse_isometric_view.png) and isometric view (./warehouse_isometric_view.png)
2. Provide the world file for the map: The .world file can be found in ./worlds/warehouse.world


# Task 2
The part will use the repository https://github.com/fetchrobotics/fetch_gazebo. Follow the tutorial and answer the questions below. There will be some tasks beyond the tutorial. 
1. When you are running the freight robot simulation (used `roslaunch fetch_gazebo simulation.launch robot:=freight`)
  i. Provide an image of rviz with the freight robot: (`rosrun rviz rviz`)
  ii. Modify the freight robot to include a 3D depth camera at the front of the robot, provide the modified file.
  iii. Run it again providing an image from RVIZ. Make sure the sensor data from the 3D depth camera is being displayed. 
2. While demo.launch is running use rosbag record to record a bag file of topics that you think are important for pathplaning, mapping and testing. (Hint: rostopic list to see all the topics). Provide the bag file you recorded.
3. Run the modified freight robot in Part 2.a.ii in the world file you created in Part 1.
  i. Provide (2) pictures of the map: one from above and one isometric view.
  ii. Use teleop_twist to move the robot and provide a recorded rosbag of the manual drive
  `rosrun teleop_twist_keyboard teleop_twist_keyboard.py`

