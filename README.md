# zebra_coding_challenge
Take-home assignment for the Simulation Software Engineering Position (Robotics)


Build a simulated warehouse using gazebo (version 7 or higher) or using ROSDS. Add 9 blue shelves that are 10 meter x 2 meter x 2 meter arranged like the image below. Be sure to include a floor and some lights.

40m

a. Provide (2) pictures of the map: one from above and one isometric view.

b. Provide the world file for the map. 2) The part will use the repository https://github.com/fetchrobotics/fetch_gazebo. Follow the tutorial and answer the questions. There will be some tasks beyond the tutorial. a. When you are running the freight robot simulation

i. Provide an image of rviz with the freight robot. ii. Modify the freight robot to include a 3D depth camera at the front of the robot, provide the modified file.

iii. Run it again providing an image from RVIZ. Make sure the sensor data from the 3D depth camera is being displayed. b. While demo.launch is running use rosbag record to record a bag file of topics that you think are important for pathplaning, mapping and testing. (Hint: rostopic list to see all the topics). Provide the bag file you recorded.

c. Run the modified freight robot in Part 2.a.ii in the world file you created in Part 1.

i. Provide (2) pictures of the map: one from above and one isometric view.

ii. Use teleop_twist to move the robot and provide a recorded rosbag of the manual driv
`rosrun teleop_twist_keyboard teleop_twist_keyboard.py`

