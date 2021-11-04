# ME495 Homework 3
## Differential Drive Robot
### James Avtges

This is a ROS package to impliment a differential drive robot that does flips in a custom gazebo world. The robot is configured in `xacro.urdf` and `gazebo.urdf` files, and the `diff_drive` node runs the robot.

### Loading the Robot in Gazebo:

To run the node and display the robot, run the launchfile by using the command `roslaunch diff_drive ddrive.launch`. This will open gazebo, unpausing the simulation will make the robot fall and drive around the world. The robot moves forward for 1.5 seconds, briefly stops, and moves backwards for 1.5 seconds before repeating again, this causes the robot to flip itself.

To run the node and display a joint state publisher in rviz, run the command `roslaunch diff_drive ddrive_rviz.launch`.

[Example Video of Gazebo Simulation](https://www.youtube.com/watch?v=1RVTJSY-gHs&ab_channel=James)