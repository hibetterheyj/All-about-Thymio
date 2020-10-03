# Notes on *Elements of Robotics*

> :star: for detailed-commented and modified code​
>
> Recommendation: using [GayHub](https://chrome.google.com/webstore/detail/gayhub/mdcffelghikdiafnfodjlgllenhlnejl) Chrome Extension for better reading experience on Github !

## Chap3 Reactive Behavior

> [Braitenberg vehicle](https://en.wikipedia.org/wiki/Braitenberg_vehicle): Valentino Braitenberg’s vehicles were constructed as thought experiments not intended for implementation with electronic components or in software

### 3.2 Reacting to the Detection of an Object

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

### 3.3 Reacting and Turning

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

### 3.4 Line Following - 3.4.1 Line Following with a Pair of Ground Sensors

#### Algorithm 3.4: Line following with two sensors

> If the robot moves off the line to the left, the left sensor will not detect the line while the right sensor is still detecting it; the robot must turn to the right.
> If the robot moves off the line to the right, the right sensor will not detect the line while the left sensor is still detecting it; the robot must turn to the left.
>
> the robot must move forwardwhenever both sensors detect a dark surface,

- 主要传感器

  prox.ground.delta/relected两个值遇到黑色反光就会降到200以下

- pseudocode | 伪代码

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

- 我的初始程序

  ```
  var speed = 100 # 定义速度变量，只能在最开始进行定义
  
  call sound.system(-1)
  call leds.top(32,0,32) # 顶部大灯，位于圆孔左右两侧(RGB)
  call leds.bottom.left(0,0,32) # 左下灯(RGB)
  call leds.bottom.right(0,32,0)# 右下灯(RGB)
  call leds.circle(0,0,0,0,0,0,0,0)# 围绕方向键盘四周的八盏灯，从正前顺时针
  
  onevent prox
  	# prox.ground.delta 共有序号[0]和[1]，越接近0表示前面两个都在线上
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

- 官方版本代码（这样的算法无法跑出全程）

  ```
  # Constants 常量
  var CHANGE = 25
  var INCREMENT = 100
  var THRESHOLD = 200
  
  # Variables 变量
  var motor          # The base value of the motor power
  var motor_change   # The amount to change when approaching and turning
  
  var state # 0 = off, 1 = on 运行状态
  
  # Initialization 变量以及LED灯状态初始化
  state = 0
  motor = 0
  motor_change = (motor * CHANGE)/100
  call leds.circle(0,0,0,0,0,0,0,0)
  call leds.top(0,0,0)
  
  # Set the circle leds to indicate the motor power
  # 设计subroutine，实现在后续motor功率调整后，车上LED灯显现出对应的颜色
  sub set_circle_leds
    if motor/100==0 then call leds.circle(0,0,0,0,0,0,0,0) end
    if motor/100==1 then call leds.circle(32,0,0,0,0,0,0,0) end
    if motor/100==2 then call leds.circle(32,32,0,0,0,0,0,0) end
    if motor/100==3 then call leds.circle(32,32,32,0,0,0,0,0) end
    if motor/100==4 then call leds.circle(32,32,32,32,0,0,0,0) end
    if motor/100==5 then call leds.circle(32,32,32,32,32,0,0,0) end
  
  # Left and right button event handlers 使用左右按钮调整功率
  #   Increase or decrease motor power within 0-500
  onevent button.left
    if  button.left == 0 then
      motor = motor - INCREMENT
      if  motor < 0 then motor = 0 end
      motor_change = (motor * CHANGE)/100
      callsub set_circle_leds
    end
  
  onevent button.right
    if  button.right == 0 then
      motor = motor + INCREMENT
      if  motor > 500 then motor = 500 end
      motor_change = (motor * CHANGE)/100
      callsub set_circle_leds
    end
  
  # When center button released
  onevent button.center
    if  button.center == 0 then
      # If off, set state to 1 (on) and drive forwards
      if  state == 0 then
        state = 1
        motor.left.target  = motor
        motor.right.target = motor
      #   else if state is on, call stop the motors
      else
        state = 0
        motor.left.target  = 0
        motor.right.target = 0
      end
      motor_change = (motor * CHANGE)/100
      callsub set_circle_leds
    end
  
  # Proximity event occurs, turn if necessary 
  # 判定是否接触地面上特定的条纹
  onevent prox
    if  state == 0 then return end
    # If both sensors detect the tape, go straight
    if  prox.ground.delta[0] < THRESHOLD and
        prox.ground.delta[1] < THRESHOLD then
      motor.left.target  = motor
      motor.right.target = motor
      call leds.top(32,0,0)
    # If left detects and right doesn't, turn left
     elseif prox.ground.delta[0] < THRESHOLD and
           prox.ground.delta[1] >= THRESHOLD then
    	 motor.left.target  = motor - motor_change
    	 motor.right.target = motor + motor_change
       call leds.top(0, 0, 32)
    # If right detects and left doesn't, turn right
     elseif prox.ground.delta[0] >= THRESHOLD and
           prox.ground.delta[1] < THRESHOLD then
      motor.left.target  = motor + motor_change
      motor.right.target = motor - motor_change
      call leds.top(0, 32, 0)
    end
  ```

- 我的最终程序

  ```
  # Constants 常量
  var CHANGE = 25
  var INCREMENT = 50
  var THRESHOLD = 200
  
  # Variables 变量
  var motor          # The base value of the motor power
  var motor_change   # The amount to change when approaching and turning
  
  var state # 0 = off, 1 = on 运行状态
  
  # Initialization 变量以及LED灯状态初始化
  state = 0
  motor = 0
  motor_change = (motor * CHANGE)/100
  call leds.circle(0,0,0,0,0,0,0,0)
  call leds.top(0,0,0)
  
  # Set the circle leds to indicate the motor power
  # 设计subroutine，实现在后续motor功率调整后，车上LED灯显现出对应的颜色
  sub set_circle_leds
    if motor/50==0 then call leds.circle(0,0,0,0,0,0,0,0) end
    if motor/50==1 then call leds.circle(32,0,0,0,0,0,0,0) end
    if motor/50==2 then call leds.circle(32,32,0,0,0,0,0,0) end
    if motor/50==3 then call leds.circle(32,32,32,0,0,0,0,0) end
    if motor/50==4 then call leds.circle(32,32,32,32,0,0,0,0) end
    if motor/50==5 then call leds.circle(32,32,32,32,32,0,0,0) end
    if motor/50==6 then call leds.circle(32,32,32,32,32,32,0,0) end
    if motor/50==7 then call leds.circle(32,32,32,32,32,32,32,0) end
    if motor/50>=8 then call leds.circle(32,32,32,32,32,32,32,32) end
  
  # Left and right button event handlers 使用左右按钮调整功率
  #   Increase or decrease motor power within 0-500
  onevent button.left
    if  button.left == 0 then
      motor = motor - INCREMENT
      if  motor < 0 then motor = 0 end
      motor_change = (motor * CHANGE)/100
      callsub set_circle_leds
    end
  
  onevent button.right
    if  button.right == 0 then
      motor = motor + INCREMENT
      if  motor > 500 then motor = 500 end
      motor_change = (motor * CHANGE)/100
      callsub set_circle_leds
    end
  
  # When center button released
  onevent button.center
    if  button.center == 0 then
      # If off, set state to 1 (on) and drive forwards
      if  state == 0 then
        state = 1
        motor.left.target  = motor
        motor.right.target = motor
      #   else if state is on, call stop the motors
      else
        state = 0
        motor.left.target  = 0
        motor.right.target = 0
      end
      motor_change = (motor * CHANGE)/100
      callsub set_circle_leds
    end
  
  # Proximity event occurs, turn if necessary 
  # 判定是否接触地面上特定的条纹
  onevent prox
    if  state == 0 then return end
    # If both sensors detect the tape, go straight
    if  prox.ground.delta[0] < THRESHOLD and
        prox.ground.delta[1] < THRESHOLD then
      motor.left.target  = motor
      motor.right.target = motor
      call leds.top(32,0,0)
    # If left detects and right doesn't, turn left
     elseif prox.ground.delta[0] < THRESHOLD and
           prox.ground.delta[1] >= THRESHOLD then
    	 #motor.left.target  = motor - motor_change
    	 #motor.right.target = motor + motor_change
    	 #motor.left.target  = motor - 2 * motor_change
    	 #motor.right.target = motor
    	 motor.left.target = 0
    	 motor.right.target = motor
       call leds.top(0, 0, 32)
    # If right detects and left doesn't, turn right
     elseif prox.ground.delta[0] >= THRESHOLD and
           prox.ground.delta[1] < THRESHOLD then
      #motor.left.target  = motor + motor_change
      #motor.right.target = motor - motor_change    
      #motor.left.target  = motor
      #motor.right.target = motor - 2 * motor_change
    	 motor.left.target = motor
    	 motor.right.target = 0
      call leds.top(0, 32, 0)
    end
  ```

#### :star:Activity 3.12: Regaining the line after losing it

> • Modify the algorithm so that the robot returns to the line after it loses it, that is, when both sensors no longer detect black.
> 
> • Algorithm 1: Before both sensors no longer detect the line, one of them will be the first that no longer detects the line. Use a variable to remember the sensor that first lost the line; when the line is lost, turn in the opposite direction of the value of this variable.
> 
> • Algorithm 2: The previous algorithm might not work if the robot is moving too fast and runs off the line before it detects that only one sensor lost the line. Instead, if both sensors no longer detect black, cause the robot the search for the line

- Algorithm1

  - 特性

    - 结合`button.backward`可以实现救援模式的打开与关闭
    - 结合`button.center`代码进行小车车速控制
    - 实现`sub set_bottom_leds`实现救援模式下不同LED效果的控制

  - 代码

    ```
    # 2020.10.02
    # Constants 常量
    var CHANGE = 25
    var INCREMENT = 50
    var THRESHOLD = 200
    
    # Variables 变量
    var motor          # The base value of the motor power
    var motor_change   # The amount to change when approaching and turning
    # Activity 3.12 Algorithm 1 P46
    var lost_first = -1 # 0 for left doesn't, 1 for right doesn't
    var recovery_mode = 0 # 0 = off, 1 = on
    
    var state # 0 = off, 1 = on 运行状态
    
    # Initialization 变量以及LED灯状态初始化
    state = 0
    motor = 0
    motor_change = (motor * CHANGE)/100
    call leds.circle(0,0,0,0,0,0,0,0)
    call leds.top(0,0,0)
    
    call leds.bottom.left(0,0,0)
    call leds.bottom.right(0,0,0)
    
    # 对于救援模式的灯
    sub set_bottom_leds
      if recovery_mode==1 then
      	  # 如果救援模式打开，若未离开线，就显示左右红灯
    	  if lost_first==-1 then
    		call leds.bottom.left(32,0,0)
    	  	call leds.bottom.right(32,0,0)
    	  # 如果救援模式打开，若离开线，就显示左右蓝灯
    	  else
    	  	call leds.bottom.left(0,0,32)
    	  	call leds.bottom.right(0,0,32)
    	  end
      # 如果救援模式不打开，就不开左右灯
      elseif recovery_mode==0 then
    	  	call leds.bottom.left(0,0,0)
    	  	call leds.bottom.right(0,0,0)
      end
    
    # Set the circle leds to indicate the motor power
    # 设计subroutine，实现在后续motor功率调整后，车上LED灯显现出对应的颜色
    sub set_circle_leds
      if motor/50==0 then call leds.circle(0,0,0,0,0,0,0,0) end
      if motor/50==1 then call leds.circle(32,0,0,0,0,0,0,0) end
      if motor/50==2 then call leds.circle(32,32,0,0,0,0,0,0) end
      if motor/50==3 then call leds.circle(32,32,32,0,0,0,0,0) end
      if motor/50==4 then call leds.circle(32,32,32,32,0,0,0,0) end
      if motor/50==5 then call leds.circle(32,32,32,32,32,0,0,0) end
      if motor/50==6 then call leds.circle(32,32,32,32,32,32,0,0) end
      if motor/50==7 then call leds.circle(32,32,32,32,32,32,32,0) end
      if motor/50>=8 then call leds.circle(32,32,32,32,32,32,32,32) end
    
    # Left and right button event handlers 使用左右按钮调整功率
    #   Increase or decrease motor power within 0-500
    onevent button.left
      if  button.left == 0 then
        motor = motor - INCREMENT
        if  motor < 0 then motor = 0 end
        motor_change = (motor * CHANGE)/100
        callsub set_circle_leds
      end
    
    onevent button.right
      if  button.right == 0 then
        motor = motor + INCREMENT
        if  motor > 500 then motor = 500 end
        motor_change = (motor * CHANGE)/100
        callsub set_circle_leds
      end
    
    # When center button released
    onevent button.center
      if  button.center == 0 then
        # If off, set state to 1 (on) and drive forwards
        if  state == 0 then
          state = 1
          motor.left.target  = motor
          motor.right.target = motor
        #   else if state is on, call stop the motors
        else
          state = 0
          motor.left.target  = 0
          motor.right.target = 0
        end
        motor_change = (motor * CHANGE)/100
        callsub set_circle_leds
      end
    
    # 打开救援模式按钮
    # When backford button released
    onevent button.backward
      if  button.backward == 0 then
        if  recovery_mode == 0 then
          recovery_mode = 1
        else
          recovery_mode = 0
          lost_first = -1
        end
      	callsub set_bottom_leds
      end
    
    # Proximity event occurs, turn if necessary 
    # 判定是否接触地面上特定的条纹
    onevent prox
      if  state == 0 then return end
      # If both sensors detect the tape, go straight
      if  prox.ground.delta[0] < THRESHOLD and
          prox.ground.delta[1] < THRESHOLD then
        motor.left.target  = motor
        motor.right.target = motor
        call leds.top(32,0,0)
        lost_first = -1
        callsub set_bottom_leds
      # If left detects and right doesn't, turn left
       elseif prox.ground.delta[0] < THRESHOLD and
             prox.ground.delta[1] >= THRESHOLD then
      	motor.left.target  = motor - motor_change
      	motor.right.target = motor + motor_change
        call leds.top(0, 0, 32)
        # Activity 3.12 Algorithm 1 P46
        lost_first = 1 # 0 for left doesn't, 1 for right doesn't
      # If right detects and left doesn't, turn right
       elseif prox.ground.delta[0] >= THRESHOLD and
             prox.ground.delta[1] < THRESHOLD then
        motor.left.target  = motor + motor_change
        motor.right.target = motor - motor_change    
        call leds.top(0, 32, 0)
        # Activity 3.12 Algorithm 1 P46
        lost_first = 0 # 0 for left doesn't, 1 for right doesn't
      # If both don't detect
       elseif prox.ground.delta[0] >= THRESHOLD and
             prox.ground.delta[1] >= THRESHOLD then
        # 若打开救援模式
        if recovery_mode == 1 then
    	    if  lost_first == 1 then
    		  	motor.left.target = -motor_change
    		  	motor.right.target = motor
    	    	callsub set_bottom_leds
    	    elseif lost_first == 0 then
    	    	motor.left.target = motor
    		  	motor.right.target = -motor_change
    	    	callsub set_bottom_leds
    	    end
    	# 若不开救援模式，则两个传感器均没有检测到就停止
    	elseif recovery_mode == 0 then
    		motor.left.target = 0
    		motor.right.target = 0
        end
      end
    ```

- Algorithm2 (TODO)

  > The previous algorithm might not work if the robot is moving too fast and runs off the line before it detects that only one sensor lost the line. Instead, if both sensors no longer detect black, cause the robot the search for the line for a short distance, first in one direction and then in the other direction.

### 3.4 Line Following - 3.4.2 Line Following with only One Ground Sensor (TODO)

#### Algorithm 3.5: Line following with one sensor

> A robot can follow a line with only one ground sensor if the reflectivity of the line varies across its width.

- pseudocode | 伪代码

---

## Chap13 (TODO)

### 13.5 Learning - 13.5.2 The Hebbian Rule for Learning in ANNs

#### Algorithm 13.1: ANN for obstacle avoidance

- Background

  In Algorithm 13.1 a timer is set to a period such as 100 ms. The timer is decremented by the operating system (not shown) and when it expires, the outputs y1 and y2 are computed by Eq. 13.3 below. These outputs are then used to set the power of the left and right motors, and, finally, the timer is reset. 

- pseudocode | 伪代码

  ```
  integer period←· · · // Timer period (ms)
  integer timer ← period
  float array[5] x
  float array[2] y
  float array[2,5] W
  
  1: when timer expires
  2: x ← sensor values
  3: y ← W x
  4: left-motor-power ← y[1]
  5: right-motor-power ← y[2]
  6: timer ← period
  ```

- Official Code (Only using 3 proximity sensors): fixed weights, using the 3 sensors for wheel speed change

  ```
  # Weights of neuron inputs
  var w_fwd  = 20  # Reasonable forward speed
  var w_back = 40  # > fwd so robot will move backwards
  var w_neg  = 30  # also > fwd
  var w_pos  = 10  # amplifies fwd so not too large
  
  # Neuron inputs and outputs
  var x1
  var x2
  var x3
  var y1
  var y2
  
  # Scale factors for sensors and constant factor
  var sensor_scale   = 200
  var constant_scale =  20
  
  # State for start and stop
  var state = 0
  
  timer.period[0] = 100    # Milliseconds
  
  # Toggle start and stop with center button
  onevent button.center
    when button.center == 0 do
      if state == 0 then
      	state = 1
      else
        state = 0
        motor.left.target  = 0
        motor.right.target = 0
      end
    end
  
  # Activiate neurons periodically
  onevent timer0
    # Do nothing if stopped
    if state == 0 then return end
  
    # Get and scale inputs
    x1 = prox.horizontal[0]/sensor_scale
    x2 = prox.horizontal[2]/sensor_scale
    x3 = prox.horizontal[4]/sensor_scale
  
    # corresponding to Fig. 13.4 Neural network for obstacle avoidance
    # Compute outputs of neurons and set motor powers
    y1 = 1*constant_scale*w_fwd + x1*w_pos - x2*w_back - x3*w_neg
    y2 = 1*constant_scale*w_fwd - x1*w_neg - x2*w_back + x3*w_pos
    motor.left.target  = y1
    motor.right.target = y2
  ```

- My code (TODO)

#### Algorithm 13.2: Feedback on the robot’s behavior

- Background

  To cause the algorithm to learn, feedback is used to modify the weightsW(Algorithms 13.2, 13.3). Let us assume that there are four buttons on the robot or on a remote control, one for each direction forwards, backwards, left and right. Whenever we note that the robot is in a situation that requires a certain behavior, we touch the corresponding button. For example, if the left sensor detects an obstacle, the robot should turn right.

  (from course exercise)

  Instead of making random movements and waiting for good situations to reinforce, you can present good situations to the network by placing the robot in an avoidance situation and pressing a button showing the right reaction

- pseudocode | 伪代码

  ```
  1: when button {forward / backward / left / right} touched
  2: y1 ← {100 / −100 / −100 / 100}
  3: y2 ← {100 / −100 / 100 / −100}
  ```

- Code (TODO)

  😅 It's hard to implement for the lack of remote control

#### :question: Algorithm 13.3: Applying the Hebbian rule

- Background

  The next phase of the algorithm is to update the connection weights according to
  the Hebbian rule (Algorithm 13.3).

- pseudocode | 伪代码

  ```
  1: x ← sensor values
  2: for j in {1, 2, 3, 4, 5}
  3: wjl ← wjl + α y1 x j
  4: wjr ← wjr + α y2 x j
  ```

- Official Code

  - X: all seven sensors and one bias, size (8,1)
  - Weight: size with (2, 8)
  - Y: size with (2, 1)
  - using bottoms for weights update
  
  ```
  # Neuron inputs (7 IR sensors and constant input) and outputs
  var x[8]
  var y[2]
  
  # Scale factors for sensors, outputs and motors
  var sensor_scale   = 100
  var output_scale   =  20
  var motor_scale    =  15
  
  # Constant input to move forwards
  var constant_input =  25
  
  # Threshold for start/stop with ground sensors
  var start_stop_threshold = 300
  
  # Left and right weights
  var w_left[8]
  var w_right[8]
  
  # Learning rate
  var alpha = 2
  
  # Speed increment for each learning episode
  var speed = 10
  
  # The desired output for each button: +/-speed
  var y_left
  var y_right
  
  # Loop index
  var i
  
  # State: off or on
  var state = 0
  
  # Time period (millisecconds) to compute motor outputs from inputs
  timer.period[0] = 100
  
  # Start in off state and not moving
  call leds.top(0,0,0)
  motor.left.target  = 0
  motor.right.target = 0
  
  # Toggle start and stop with ground sensors
  onevent prox
    if prox.ground.delta[0] > start_stop_threshold and
       prox.ground.delta[1] > start_stop_threshold then
      return
    end
    motor.left.target  = 0
    motor.right.target = 0
    if state == 0 then
      # Set weights initially to zero
      for i in 0:7 do
    	    w_left[i]  = 0
    	    w_right[i] = 0
    	 end
      call leds.top(32,32,32)
      state = 1
    else
      call leds.top(0,0,0)
      state = 0
    end
  
  # Change weights according to Hebbian rule
  sub change_weights
  	for i in 0:7 do
  		w_left[i]  = w_left[i]  + (alpha*y_left*x[i])  / output_scale
  		w_right[i] = w_right[i] + (alpha*y_right*x[i]) / output_scale
  	end
  
  # For each button set y_left, y_right and change weights
  onevent button.center
    y_left  = speed
    y_right = speed
    callsub change_weights
  
  onevent button.left
    y_left  = -speed
    y_right = speed
    callsub change_weights
  
  onevent button.right
    y_left  = speed
    y_right = -speed
    callsub change_weights
  
  onevent button.forward
    y_left  = speed
    y_right = speed
    callsub change_weights
  
  onevent button.backward
    y_left  = -speed
    y_right = -speed
    callsub change_weights
  
  # Timer event
  onevent timer0
     if state == 0 then return end
  
     # Read and scale inputs
     x[7] = constant_input
     for i in 0:6 do
       x[i]=prox.horizontal[i]/sensor_scale
     end
  
     # Compute dot product of inputs and weights
     y[0] = 0
     y[1] = 1
     for i in 0:7 do
     	y[0] = y[0] + x[i]*w_left[i]
     	y[1] = y[1] + x[i]*w_right[i]
     end

    # Scale and set motor powers
    motor.left.target  = y[0] / motor_scale
    motor.right.target = y[1] / motor_scale
  ```
  
  