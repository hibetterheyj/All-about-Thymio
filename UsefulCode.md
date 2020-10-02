## Useful Code Collections

## TODO

- light blinking
- speaker output
- PID controller
- python bridge

## Collections

### change the motor speed dynamically with left/right button

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