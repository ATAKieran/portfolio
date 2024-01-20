[home](README.md)

# Robot Arm Satellite Positioning Simulator (RASPS)

This was my computer engineering design capstone project. Myself and three other students were tasked with developing a "test bed system to support hardware-in-the-loop simulation and modelling of satellite proximity operations." In practice, this meant restoring an existing 3D-printed robot arm, mounting it to a rail system, and writing control software for the combined arm-rail system.

## The Arm

The robot arm was a 5 degree-of-freedom (DoF) 3D-printed arm based on an [open-source design](https://www.bcn3d.com/bcn3d-moveo-the-future-of-learning-robotic-arm/) and from a previous team's capstone project. The arm had been left to us in an unkown state, so our first task was to diassemble the arm, test each component, and add any missing pieces. The arm was missing a microcontroller, which had to be sourced, installed, and programmed. We also replaced the "claw" at the end of the arm with a mounting plate so that satellite models could be mounted to the arm during motion simulation tests. The completed arm looked like this:

## The Rail

The rail system was our main addition to the system as it had to be designed and built from scratch. We decided on using a ball screw system, as it would best allow us to achieve our targeted rail motion precision. The rails were mounted to plywood for stability, and the arm was then mounted to the rails. 

## The Control System

The control system consisted of two main parts: the microcontroller and the control system. An arduino microcontroller using the AccelStepper and MultiStepper libraries controlled the stepper motors, and received commands from a laptop running our control software. Our software used an inverse kinematics library and a model of the robot to convert our input coordinates into the robot's motion. The link to the project's GitHub repository can be found [here](https://github.com/ECE492-RASPSP1/RASPSP1/).

## The Result
[blah](4Q4A1082.MP4)