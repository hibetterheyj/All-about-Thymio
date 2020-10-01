
     Thymio Implementations of Activities
            Elements of Robotics
      Moti Ben-Ari and Francesco Mondada

Copyright 2017 by Moti Ben-Ari and Francesco Mondada

This work is licensed under the Creative Commons
Attribution-ShareAlike 3.0 Unported License. To view a copy
of this license, visit
http://creativecommons.org/licenses/by-sa/3.0/ or send a
letter to Creative Commons, 444 Castro Street, Suite 900,
Mountain View, California, 94041, USA.

The programs in this archive are implementations of many of
the activities in the book. The source files are the titles
of the activities with the extension aesl used by the Aseba
Studio development environment of the Thymio.

For algorithms which were difficult to implement on the
robot, a simulation in Python is provided. See the Python
archive.

Some of the simpler programs have been implemented using the
Visual Program Language (VPL) environment.

On the Thymio website https://www.thymio.org, follow the
link "Program" to download the Aseba environment for your
system.

Comments in the source files explain how to use the programs
and provide documentation of the code. For extensive
descriptions of the design and implementation of many of the
programs, see the document: "Projects for the Thymio Robot
in the Aseba Studio Environment" which can be found by
following the link "Text programming" on the "Program" page.

The activities in Chapter 12 on image processing use printed
patterns that are attached to the surface. The file
image-patterns.pdf contains images to use in these
activities. The LaTeX source code is in image-patterns.tex.

The values returned by the Thymio's sensors can be read
directly in the Studio environment without writing a
program. On the left of the window is a pane labeled
"Variables". Click the box "auto" and the values of the
values in the pane will be updated continuously. Of
particular importance are the values of the horizontal
proximity sensors: prox.horizontal[i], where i=0,1,2,3,4 for
the front sensors and i=5,6 for the rear sensors (counting
from left to right).


Notations:

Program for n.m: The program for Activity n.m
  is used for this Activity.

No program: This activity does not require
  an implemention of a robot.

Python: This activity is implemented as a Python program,
  not on a robot.

Also in Python: This activity is implemented both on a robot
  and as a Python program.

VPL: This activity is implemented using the
  VPL visual environment.

Variables: This activity can be carried out using the data
  displayed in the Variables pane.

Or Variables: There is a program for this activity,
  but it can also be carried out using the Variables pane.


List of activities

 2.1:  Range of a distance sensor (Variables)
 2.2:  Thresholds (Program for 2.1 or Variables)
 2.3:  Reflectivity (Program for 2.1 or Variables)
 2.4:  Triangulation (No program)
 2.5:  Measuring the attitude using accelerometers (Or Variables)
         ...-music
         ...-incline
 2.6:  Precision and resolution (Variables)
 2.7:  Accuracy (Variables)
 2.8:  Linearity (Variables)

 3.1:  Timid (VPL)
 3.2:  Indecisive (VPL)
 3.3:  Dogged (VPL)
 3.4:  Dogged1 (VPL)
 3.5:  Attractive and repulsive (VPL)
 3.6:  Paranoid (VPL)
 3.7:  Paranoid1 (VPL)
 3.8:  Insecure (VPL)
 3.9:  Driven (VPL)
 3.10: Line following with two sensors
 3.11: Different line configurations (Program for 3.10)
 3.12: Regaining the line after losing it (Program for 3.10)
 3.13: Sensor configuration (No program)
 3.14: Line following with one sensor
 3.15: Line following with proportional correction
 3.16: Line following without a gradient (Program for 3.14)
 3.17: Line following in practice (No program)
 3.18  Braitenberg vehicles
         ...-coward
         ...-aggressive
         ...-love
         ...-explorer

 4.1:  Consistent (VPL)
 4.2:  Nondeterminism
 4.3:  Persistent (VPL)
 4.4:  Paranoid2 (VPL)
 4.5:  Search and approach

 5.1:  Velocity over a fixed distance
 5.2:  Change of velocity (Program for 5.1)
 5.3:  Acceleration
 5.4:  Computing distance when accelerating (No program)
 5.5:  Measuring motion at constant acceleration
 5.6:  Distance from speed and time
 5.7:  Odometry in two dimensions
 5.8:  Odometry errors
 5.9:  Correcting odometry errors
 5.10: Wheel encoding (Program for 5.1)
 5.11: A robot that can only rotate (No program)
 5.12: Robotic crane
 5.12: Robotic crane alternatives (also Python)
 5.13: Holonomic and non-holonomic motion (No program)

 6.1:  Setting the control period (No program)
 6.2:  On-off algorithm
 6.3:  Proportional control
 6.4:  PI controller
 6.5:  PID controller
 6.6:  Ziegler-Nichols tuning (Program for 6.5)

 7.1:  Conditional expressions for wall following
 7.2:  Simple wall following
 7.3:  Wall following with direction
         (Program for 7.4, Also in Python)
 7.4:  Pledge algorithm (Also in Python)
 7.5:  Line following while reading a code
 7.6:  Circular line following while reading a code
         (no program for clocks)
 7.7:  Locating the nest
 7.8:  Sensing areas of high density (Program for 3.14)
 
 8.1:  Play the landmark game (No program)
 8.2:  Determining position from an angle and a distance
 8.3:  Determining position by triangulation
 8.4:  Localization with uncertainty in the sensors (Also in Python)
 8.5:  Localization with uncertainty in the motion (Also in Python)

9.1   Probabilistic map of obstacles (Program for 8.4)
9.2   Frontier algorithm (Python) (Not implemented on robot)
9.3   A robotic lawnmower
         ... -landmark
9.4   Localize the robot from the computed perceptions (Python)
9.5   Localize the robot from the measured perceptions

10.1:  Dijkstra’s algorithm on a grid map (Python)
10.2:  Dijkstra’s algorithm for continuous maps (No program)
10.3:  The A* algorithm (Python)
10.4:  Combining path planning and obstacle avoidance

11.1:  Fuzzy logic

      Images in image-patterns.{tex,pdf}.
12.1: Image enhancement: smoothing (Also in Python)
12.2: Image enhancement: histogram manipulation
        (program for 12.1) (Also in Python)
12.3: Detecting an edge (Also in Python)
12.4: Detecting a corner (Also in Python)
12.5: Detecting a blob (Also in Python)
12.6: Recognizing a door

13.1:  Artificial neurons for logic gates
       ... not
       ... or-and
13.2:  Analog artificial neurons
       ... one input
       ... two inputs
13.3:  ANN for obstacle avoidance: design (No program)
13.4:  ANN for obstacle avoidance: implementation
13.5:  ANN for object attraction
13.6:  Multilayer ANNs (Also in Python)
13.7   Multilayer ANN for obstacle avoidance (Also in Python)
13.8:  ANN with memory
13.9:  ANN for spatial processing
13.10: Hebbian learning for obstacle avoidance

       Images in gray-areas.{tex,pdf}.
14.1:  Robotic chameleon
       Offline computations in Python
14.2:  Robotic chameleon with LDA (Program for 14.1)
       Offline computations in Python
14.3:  Obstacle avoidance with two sensors
       Offline computations in Python
14.4:  Following an object (Program for 14.3)
14.5:  Learning by a perceptron
       Offline computations in Python

15.1:  BeeClust algorithm (no program)
15.2:  Pulling force by several robots (no program)
15.3:  Total force (no program)
15.4:  Occlusion-based pushing (no program)

16.1:  Forward kinematics
16.2:  Inverse kinematics (Program for 16.1)
       Offline computations in Python
16.3:  Rotation matrices (No program)
16.4:  Homogeneous transforms (No program)
16.5:  Multiple Euler angles (No program)
16.6:  Distinct Euler angles (No program)
