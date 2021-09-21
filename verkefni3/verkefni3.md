```c
#pragma config(Sensor, in4,    lightSensor,    sensorReflection)
#pragma config(Sensor, dgtl4,  rightEncoder,   sensorQuadEncoder)
#pragma config(Sensor, dgtl6,  leftEncoder,    sensorQuadEncoder)
#pragma config(Sensor, dgtl10, bumpSwitch,     sensorTouch)
#pragma config(Sensor, dgtl11, Sensor,         sensorSONAR_cm)
#pragma config(Motor,  port1,           rightMotor,    tmotorServoContinuousRotation, openLoop, reversed)
#pragma config(Motor,  port10,          leftMotor,     tmotorServoContinuousRotation, openLoop)

#define BASEDEG 2.71
//+++++++++++++++++++++++++++++++++++++++++++++| MAIN |+++++++++++++++++++++++++++++++++++++++++++++++
void turn(int deg, int leftOrRight) {
	while(abs(SensorValue[rightEncoder])<deg*BASEDEG) {
		motor[rightMotor] = 80* leftOrRight;
		motor[leftMotor] = -80* leftOrRight;
	}
}
task stopTasks() {
	while(vexRT[Btn8D] != 1 && SensorValue[bumpSwitch]!=1) {
	}
	stopAllTasks();
}

void stopMotor(int stopTime) {
	motor[rightMotor] = 0;
	motor[leftMotor]  = 0;
	wait1Msec(stopTime);
}

void resetEncoder() {
	SensorValue[rightEncoder] = 0;
	SensorValue[leftEncoder] = 0;
}
task main()
{
	while(true) {
		wait1Msec(2000);
		startTask(stopTasks);
		//resetEncoder();
		while(SensorValue(lightSensor) < 500)
		{
			while(SensorValue[Sensor] > 40 || SensorValue[Sensor] == -1 ){
				if(abs(SensorValue[rightEncoder]) == abs(SensorValue[leftEncoder]))
				{
					motor[rightMotor] = -80;
					motor[leftMotor]  = -80;
				}
				else if(abs(SensorValue[rightEncoder]) > abs(SensorValue[leftEncoder]))
				{
					// Beygja smá til vinstri
					motor[rightMotor] = -60;
					motor[leftMotor]  = -80;
				}
				else
				{
					// Beygja smá til hægri
					motor[rightMotor] = -80;
					motor[leftMotor]  = -60;
				}
			}
			stopMotor(200);
			turn(90, 1);
			resetEncoder();
			stopMotor(200);
		}

	}
}
//++++++++++++++++++++++++++++++

```
