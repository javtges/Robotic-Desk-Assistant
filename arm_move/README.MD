# ME495 Homework 3
## PX100 Arm Path Planning
### James Avtges

This portion of the homework involves commanding the Interbotix PX100 robot arm to follow a series of waypoints and avoid obstacles.

### Instructions for Running the Program:

In order to run the program, call the launchfile `arm.launch`. This has a few arguments, the ones to be used for this homework's implimentation are the following:

`mode:=yaml` - This loads some preset waypoints from a yaml file into the parameter server under `/px100/waypoints`. These are configured for a pick-and-place task in the gazebo world.

`use_gazebo:=true` - This runs both gazebo and moveit/rviz, the custom gazebo world has a realsense box and a small box to pick up loaded into it.

`use_actual:=true` - This configures the real-world PX100 robot to run. Gazebo is loaded as well to visualize the robot.

An example usage of the launchfile would be the command line input:

`roslaunch arm_move arm.launch mode:=yaml use_gazebo:=true` or `roslaunch arm.launch mode:=yaml use_actual:=true`

### Controlling the Robot Arm:

To control the robot arm, there are three ROS services provided by the `mover` node in the arm_move package. They are:

`/px100/reset` - This moves the arm to its "Home" position, adds a box to the rviz planning scene, and contains an option to reset the `/px100/waypoints` parameter list if true.

Example usage: `rosservice call /px100/reset "{x: 0.2, y: 0.0, z: 0.025, rx: 0.0, ry: 0.0, rz: 0.1, rw: 0.0, reset: false}"`

`/px100/step` - This moves the arm to a specified location, with a specified gripper state (`gripper_pose = True` will close the gripper). If it's an impossible state to reach, the arm will not move and it will return a MoveItErrorCode of -1. If it is possible, it will move to that position, return a MoveItErrorCode of 1, and add that position to the `/px100/waypoints` parameter server list.

Example usage: `rosservice call /px100/step "point: x: 0.2 y: 0.2 z: 0.2 gripper_pose: false"`

`/px100/follow` - This moves the arm, in order, to all of the waypoints in the `/px100/waypoints` parameter server. It has an argument, that when true, will continue repeating the waypoints in order.

Example usage: `rosservice call /px100/follow "repeat: false"`

### Other Functionality:

There are built-in ROS tests to test the `/px100/step` service. There is a test to first move the arm to a place where `/px100/step` returns an errorcode of 1 (a success), and a test case to move it into the ground (returning an errorcode of -1). They can be run by using `catkin_make run_tests` in the ROS workspace.