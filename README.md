# Arduino_LinkIt7697_Servo_motor_Light_Sensor

This project shows how to detect the intensity of a given light source and move a servo motor accordingly using LinkIt 7697.

![demo](/images/demo.gif?raw=true)

## Installation

If you want to use the **LinkIt 7697** with the Arduino IDE, follow [this](https://docs.labs.mediatek.com/resource/linkit7697-arduino/en/environment-setup) tutorial. As a summary:
1. Go to *File -> Preferences* and in the **Additional Boards Manager URLs**, type
```
http://download.labs.mediatek.com/package_mtk_linkit_7697_index.json
```
![install01](/images/install00.gif?raw=true)

2. Then go to *Tools -> Boards -> Board Manager...* and in the search box look for **LinkIt**. Install the additional libraries.

![install02](/images/install01.gif?raw=true)

3. Finally install the USB driver (CP210x) from [here](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers), selecting the correct OS configuration.

![install03](/images/pic99.png?raw=true)

## Code and configuration

In order to correctly connect the board and the sensor, the code has to be analyzed first.

### Code

#### Servo motor

In order to use a servo motor, we need to import the **Servo** library and initialize the **Servo** object as follows.
```arduino
#include <Servo.h>

Servo myservo;        // create servo object to control a servo
```
Note that the servo motor is polarized, i.e., can turn in both directions depending on the polarity. 

For starting it, just add the *begin* command to setup.
```arduino
void setup()
{
	myservo.attach(8);  // attaches the servo on pin 8 to the servo object
	Serial.begin(9600);
}
```
The servo can be rotated to an specific angle depending of a value. This can be done inside the *loop* cycle as follows.
```arduino
void loop(){
  int val = 1500;					//An example value
  val = map(val, 0, 3500, 0, 180);  // scale it to use it with the servo (value between 0 and 180)

  Serial.println(val);
  myservo.write(val);               // sets the servo position according to the scaled value

  delay(100);                       // waits for the servo to get there
}
```
where the *myservo.write(val)* helps in the rotation of the servo.

**NOTE:** A delay needs to be performed after the rotation of the servo to give time to it to finish the movement.

#### Light sensor

The light sensor used in this project was a standard analog light sensor.

![sensor01](/images/sensor02.jpg?raw=true)

![sensor02](/images/sensor03.jpg?raw=true)

In order to read its inputs, we need to define the analogic pin that will provide the board with the sensors data

```arduino
int potpin = A0;      // analog pin used to connect the potentiometer
```
and we need to read its content inside the *loop* cycle as
```arduino
void loop()
{
	...

	int val = analogRead(potpin);         // reads the value of the potentiometer (value between 0 and 1023)
	
	...
}
```
### Configuration

Because in our code we find the lines
```arduino
#include <Servo.h>

Servo myservo;        // create servo object to control a servo

int potpin = A0;      // analog pin used to connect the potentiometer
int val;              // variable to read the value from the analog pin

void setup() {
  myservo.attach(8);  // attaches the servo on pin 8 to the servo object
  Serial.begin(9600);
}
```
We then need to update the connections accordingly:
1. Connect the servo motor to the IC2 port.
2. Connect the light sensor to the A0 port.

![sensor03](/images/motor00.jpg?raw=true)

Please beware of the polarity of the servo motor and the position of the GND and 3.3V pins for the sensor.

![sensor04](/images/sensor00.jpg?raw=true)

![sensor03](/images/image00.jpg?raw=true)

## Extensions

This project can be modified to use any other analogic sensor, such as a light sensor. Just connect it to the A0 port.

## Demo

A demo of servo motor moving to different positions depending on the detected light intensity can be seen below.

![demo](/images/demo.gif?raw=true)