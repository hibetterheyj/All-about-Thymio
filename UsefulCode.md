## Useful Code Collections

## TODO

- light blinking
- speaker output
- PID controller
- python bridge

## Collections

### 1. start/stop the robot with center bottom

```
onevent button.center
  if  button.center == 0 then
    # If off, set state to 1 (on) and drive forwards
    if  state == 0 then
      state = 1
      #motor.left.target  = motor
      #motor.right.target = motor
    #   else if state is on, call stop the motors
    else
      state = 0
      motor.left.target  = 0
      motor.right.target = 0
    end
    #you can add other code for light effect etc.
    #callsub set_circle_leds
  end
```

### 2. change the motor speed dynamically with left/right button

#### code

```
# Constants
var CHANGE = 25
var INCREMENT = 100

# Variables
var motor          # The base value of the motor power
var motor_change   # The amount to change when approaching and turning

# Initialization
motor = 0
motor_change = (motor * CHANGE)/100
call leds.circle(0,0,0,0,0,0,0,0)
call leds.top(0,0,0)

# Set the circle leds to indicate the motor power
sub set_circle_leds
  if motor/100==0 then call leds.circle(0,0,0,0,0,0,0,0) end
  if motor/100==1 then call leds.circle(32,0,0,0,0,0,0,0) end
  if motor/100==2 then call leds.circle(32,32,0,0,0,0,0,0) end
  if motor/100==3 then call leds.circle(32,32,32,0,0,0,0,0) end
  if motor/100==4 then call leds.circle(32,32,32,32,0,0,0,0) end
  if motor/100==5 then call leds.circle(32,32,32,32,32,0,0,0) end

# Left and right button event handlers
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
```

#### Reference

TO BE UPDATED !