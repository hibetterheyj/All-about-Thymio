# UsefulCode-Python

## Coverage

- read from sensor
- activate the sound
- real-time sensor plot
- proximity calibration

## Scripts

### Importing Official Python bridge

For more details, please refer to `Thymio Python Example.ipynb` and `Thymio.py` in `Python Bridge/`.

```python
import os
import sys
import time
import serial

# Adding the src folder in the current directory as it contains the script
# with the Thymio class
sys.path.insert(0, os.path.join(os.getcwd(), 'src'))

from Thymio import Thymio
```

### Connection

> For more details. please refer to `Python Bridge/Python Bridge/Thymio Python Example.ipynb`

```
## Ubuntu or Macos
th = Thymio.serial(port="/dev/cu.usbmodem14301", refreshing_rate=0.1)

## Windows
# if output as follows:
# could not open port 'COM3': PermissionError(13, 'Access is denied.', None, 5)
# please close the Thymio Suite
th = Thymio.serial(port="COM3", refreshing_rate=0.1)
```

### Basics function from bridge

```python
# check accessible method
dir(th)

# print all variables
variables = th.variable_description()

for var in variables : 
    print(var)
```

### Reading from sensors

> For the range of the sensors, please refer to 

#### Proximity sensors

```
# hyper parameters used for algorithms
prox_intensity: int = 0
prox_intensity_threshold = 2000

# print all horizontal proximity sensor values with small break
for i in range(30):
    print(th["prox.horizontal"])
    time.sleep(0.2)

# Checking the value of the middle front proximity IR sensor
prox_intensity = th["prox.horizontal"][3]
print("Intensity of the proximity sensor:", prox_intensity)
```

### Buttoms

🚧 To be updated !

### Passing values to actuators

> For the range of the actuators, please refer to 

#### Motors

```
# Nonnegative cases
th.set_var("motor.left.target", 50)
th.set_var("motor.right.target", 50)

# Negative cases
# to send a negative value, as the values are coded in uint_16, 
# you need to add 2^16-x where xis the absolute value of the 
# negative speed that you want to set
th.set_var("motor.left.target", 500)
th.set_var("motor.right.target", 2**16-500)
```

#### LEDs

```python
th.set_var_array("leds.top", [255, 0, 0]) # red
time.sleep(3)
th.set_var_array("leds.top", [0, 255, 0]) # green
time.sleep(3)
th.set_var_array("leds.top", [0, 0, 255]) # blue
time.sleep(3)
th.set_var_array("leds.top", [0, 0, 0]) # no light
```

#### Sound

```
import pyglet
sound = pyglet.media.load("sound.wav", streaming=False)
# Playing a sound
sound.play()
```

### Misc.

```python
# Sleeping a little bit before between commands
import time
time.sleep(2)  
```

## Reference

- https://github.com/Antho1426/thymio-slam

  Main functions: 

  - https://github.com/Antho1426/thymio-slam/blob/master/thymio_slam_path_planning.py

    ```
    computePath(ys, xs, ye, xe, orient, map)
    astar(maze, start, end)
    ```

  - https://github.com/Antho1426/thymio-slam/blob/master/thymio_slam_live_plot.py

    real-time plotting

  - https://github.com/Antho1426/thymio-slam/blob/master/thymio_slam.py

    about init, basic control (`stepForward()`, `turnLeft90()`, `def updateOrientInGraph(orient)`, etc.), and draw_map

