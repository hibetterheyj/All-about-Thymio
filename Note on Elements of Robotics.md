# Notes on *Elements of Robotics*

## Chap3 Reactive Behavior

- [Braitenberg vehicle](https://en.wikipedia.org/wiki/Braitenberg_vehicle)：只能小车雏形

#### :star:Algorithm 3.1: Timid

```
#call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(32,0,0,0,32,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

# onevent定义事件
onevent proxIf
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	if prox.horizontal[2] < 1000 then
		# motor.left.target轮速设定
		motor.left.target = 200
		motor.right.target = 200
	end
	if prox.horizontal[2] > 2000 or prox.horizontal[1] > 2000 then
		motor.left.target = 0
		motor.right.target = 0
	end
```

#### :star:Algorithm 3.2: Timid with while

```
#call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(32,0,0,0,32,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

onevent prox
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	while prox.horizontal[2] < 1000 do
		# motor.left.target轮速设定
		motor.left.target = 200
		motor.right.target = 200
	end
	while prox.horizontal[2] > 2000 do
		motor.left.target = 0
		motor.right.target = 0
	end
```

#### Activity 3.3: Dogged

> Specification (Dogged): When the robot detects an object in front, it moves backwards.
> When the robot detects an object in back, it moves forwards.

```
call sound.system(-1)
call leds.top(0,0,0)
call leds.bottom.left(0,0,0)
call leds.bottom.right(0,0,0)
call leds.circle(0,0,0,0,0,0,0,0)

onevent prox
	if prox.horizontal[2] > 2000 then
		motor.left.target = -200
		motor.right.target = -200
	end
	if prox.horizontal[5] > 2000 and prox.horizontal[6] > 2000 then
		motor.left.target = 200
		motor.right.target = 200
	end
```

#### :star:Activity 3.4: Dogged (stop)

> Specification (Dogged (stop)): As in Activity 3.3, but when an object
> is not detected, the robot stops.

```
call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(0,0,0,0,0,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

onevent prox
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	if prox.horizontal[2] > 2000 then
		motor.left.target = -100
		motor.right.target = -100
	end
	if prox.horizontal[5] > 2000 or prox.horizontal[6] > 2000 then
		motor.left.target = 100
		motor.right.target = 100
	end
	if prox.horizontal[2] < 1000 and prox.horizontal[5] < 1000 and prox.horizontal[6] < 1000 then
		motor.left.target = 0
		motor.right.target = 0
	end
```

#### Activity3.5 Attractive and repulsive

> Specification (Attractive and repulsive):When an object approaches the robot from behind, the robot runs away until it is out of range.

```
call sound.system(-1)
call leds.top(0,0,0)
call leds.bottom.left(0,0,0)
call leds.bottom.right(0,0,0)
call leds.circle(0,0,0,0,0,0,0,0)

onevent prox
	if prox.horizontal[5] > 2000 and prox.horizontal[6] > 2000 then
		motor.left.target = 300
		motor.right.target = 300
	end
	# 当两个事件同时发生才会运动
	if prox.horizontal[5] < 1000 and prox.horizontal[6] < 1000 then
		motor.left.target = 0
		motor.right.target = 0
	end
```

#### Algorithm 3.3 Paranoid

> In the algorithm, the left and right motors are set to equal but opposite powers. Experiment with these power levels to see their influence on the turning radius of the robot.

```
onevent prox
	if prox.horizontal[2] > 2000 then
		motor.left.target = 200
		motor.right.target = 200
	end
	if prox.horizontal[2] < 1000 then
		motor.left.target = -200
		motor.right.target = 200
	end
```

#### :star:Activity 3.6 Paranoid (right-left)

> Specification (Paranoid (right-left)): When an object is detected in front of the robot, the robot moves forwards. When an object is detected to the right of the robot, the robot turns right. When an object is detected to the left of the
> robot, the robot turns left. When no object is detected the robot does not move.

```
var speed = 150 # 定义速度变量，只能在最开始进行定义

call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(0,0,0,0,0,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

onevent prox
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	if prox.horizontal[2] > 2000 then
		motor.left.target = speed
		motor.right.target = speed
	end
	if prox.horizontal[2] < 1000 and prox.horizontal[0] > 2000 then
		motor.left.target = -speed
		motor.right.target = speed
	end
	if prox.horizontal[2] < 1000 and prox.horizontal[4] > 2000 then
		motor.left.target = speed
		motor.right.target = -speed
	end
	if prox.horizontal[2] < 1000 and prox.horizontal[4] < 1000 and prox.horizontal[0] < 1000 then
		motor.left.target = 0
		motor.right.target = 0
	end
	
```

#### Activity 3.8: Insecure

> Specification (Insecure): If **an object is not detected to the left of the robot**, set the right motor to rotate forwards and set the left motor off. If an object is **detected** to the left of the robot, set the right motor off and set the left motor to rotate forwards.

```
var speed = 200 # 定义速度变量，只能在最开始进行定义

call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(0,0,0,0,0,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

onevent prox
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	if prox.horizontal[0] < 1000 or prox.horizontal[1] < 1000 then
		motor.left.target = 0
		motor.right.target = speed
	end
	if prox.horizontal[0] > 2000 or prox.horizontal[1] > 2000 then
		motor.left.target = speed
		motor.right.target = 0
	end
```

会观察发现这个机器人可以跟着墙右边行走

#### Activity 3.9: Driven

> Specification (Driven): If an object is detected to the left of the robot, set the right motor to rotate forwards and set the left motor off. If an object is detected to the right of the robot, set the right motor off and set the left motor to rotate forwards.

```
var speed = 200 # 定义速度变量，只能在最开始进行定义

call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(0,0,0,0,0,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

onevent prox
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	if prox.horizontal[3] > 2000 or prox.horizontal[4] > 2000 then
		motor.left.target = 0
		motor.right.target = speed
	end
	if prox.horizontal[0] > 2000 or prox.horizontal[1] > 2000 then
		motor.left.target = speed
		motor.right.target = 0
	end
```

#### Algorithm 3.4: Line following with two sensors

> If the robot moves off the line to the left, the left sensor will not detect the line while the right sensor is still detecting it; the robot must turn to the right.
> If the robot moves off the line to the right, the right sensor will not detect the line while the left sensor is still detecting it; the robot must turn to the left.
>
> the robot must move forwardwhenever both sensors detect a dark surface,

- 主要传感器

  prox.ground.delta/relected两个值遇到黑色反光就会降到200以下

- 伪代码

  ```
  1: when both sensors detect black
  2: left-motor-power ← 100
  3: right-motor-power ← 100
  4:
  5: when neither sensor detects black
  6: left-motor-power ← 0
  7: right-motor-power ← 0
  8:
  9: when only the left sensor detects black
  10: left-motor-power ← 0
  11: right-motor-power ← 50
  12:
  13: when only the right sensor detects black
  14: left-motor-power ← 50
  15: right-motor-power ← 0
  ```

- 程序

```
var speed = 100 # 定义速度变量，只能在最开始进行定义

call sound.system(-1)
call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
call leds.bottom.left(0,0,32) # 左下灯(RGB)
call leds.bottom.right(0,32,0)# 右下灯(RGB)
call leds.circle(0,0,0,0,0,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针

onevent prox
	# prox.horizontal共有序号[0]到[6]，越接近0表示前面没有物体遮挡
	if prox.ground.delta[0] < 150 and prox.ground.delta[1] < 150 then
		motor.left.target = 2 * speed
		motor.right.target = 2 * speed
	end
	if prox.ground.delta[0] > 200 and prox.ground.delta[1] > 200 then
		motor.left.target = 0
		motor.right.target = 0
	end
	if prox.horizontal[0] < 150 and prox.ground.delta[1] > 200 then
		motor.left.target = 0
		motor.right.target = speed
	end
	if prox.horizontal[1] < 150 and prox.ground.delta[0] > 200 then
		motor.left.target = speed
		motor.right.target = 0
	end
```

#### Activity 3.12: Regaining the line after losing it

> • Modify the algorithm so that the robot returns to the line after it loses it, that is, when both sensors no longer detect black.
> 
> • Algorithm 1: Before both sensors no longer detect the line, one of them will be the first that no longer detects the line. Use a variable to remember the sensor that first lost the line; when the line is lost, turn in the opposite direction of the value of this variable.
> 
> • Algorithm 2: The previous algorithm might not work if the robot is moving too fast and runs off the line before it detects that only one sensor lost the line. Instead, if both sensors no longer detect black, cause the robot the search for the line