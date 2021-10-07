### Lýsing á verkefni
Ég læt róbotinn elta línu með línusensorunum sem LCD sýnir values af. Ég læt hluti fyrir veginn hans og hann tekur eftir því með sonic sensor og færir það frá með því að beygja með hjálp gyro og heldur síðan áfram eftir línunni.
Þrautin: [Hér](https://github.com/SiggiMatti/ROBO2/blob/main/verkefni5/traut.png)  
Vídjó af virkni: [Hér](https://youtu.be/DamfeEkbZJY)  

### Sensorar:  
* Línusensor (analog) 
* Lightsensor (analog)
* Gyro (Analog)
* Neyðarhnappur (Digital)
* Neyðarhnappur fjarstýring (Digital)
* Sonic sensor (Digital)
* LCD (UART 2)

### Sauðakóði: 5  
```c
task main()
{
	while(true) {
		eltaLinu();
		if (sonicSensor < 30) {
			claw(-1);
			turn(90);
			claw(1);
			turn(-90);
		}
	}
}
```

### Kóði
```c
#pragma config(Sensor, in1,    lineFollowerRIGHT, sensorLineFollower)
#pragma config(Sensor, in2,    lineFollowerCENTER, sensorLineFollower)
#pragma config(Sensor, in3,    lineFollowerLEFT, sensorLineFollower)
#pragma config(Sensor, in4,    lightSensor,    sensorReflection)
#pragma config(Sensor, dgtl10, bumpSwitch,     sensorTouch)
#pragma config(Sensor, dgtl11, sonicSensor,    sensorSONAR_cm)
#pragma config(Motor,  port1,           rightMotor,    tmotorServoContinuousRotation, openLoop, reversed)
#pragma config(Motor,  port2,           clawMotor,     tmotorVex269_MC29, openLoop, reversed)
#pragma config(Motor,  port10,          leftMotor,     tmotorServoContinuousRotation, openLoop)

task stopTasks() {
	while(vexRT[Btn8D] != 1 && SensorValue[bumpSwitch]!=1) {
	}
	stopAllTasks();
}

int threshold = 2000;
void eltaLinu() {
	if(SensorValue(lineFollowerRIGHT) > threshold)
			{
				// beygja smá til hægri:
				motor[leftMotor]  = -63;
				motor[rightMotor] = 0;
			}
			// miðju sensorinn sér dökkt:
			if(SensorValue(lineFollowerCENTER) > threshold)
			{
				// fara beint
				motor[leftMotor]  = -63;
				motor[rightMotor] = -63;
			}
			// vinstri sensorinn sér dökkt:
			if(SensorValue(lineFollowerLEFT) > threshold)
			{
				// beygja smá til vinstri:
				motor[leftMotor]  = 0;
				motor[rightMotor] = -63;
			}
}

void takaHlut(int f_b) {
	motor[clawMotor] = f_b * 127;
	wait1Msec(500);
	motor[clawMotor] = 0;
	wait1Msec(100);
}

	void turn(int f_b) {

	  SensorType[in8] = sensorNone;
	  wait1Msec(1000);

	  SensorType[in8] = sensorGyro;
	  wait1Msec(2000);

	  int degrees10 = 900;
	  int error = 5;

	  while(abs(SensorValue[in8]) < degrees10 - 100)
	  {
	    motor[rightMotor] = 50 * f_b;
	    motor[leftMotor] = -50 * f_b;
	  }
	  motor[rightMotor] = -5 * f_b;
	  motor[leftMotor] = 5 * f_b;
	  wait1Msec(100);

	  while(abs(SensorValue[in8]) > degrees10 + error || abs(SensorValue[in8]) < degrees10 - error)
	  {
	    if(abs(SensorValue[in8]) > degrees10)
	    {
	      motor[rightMotor] = -30 * f_b;
	      motor[leftMotor] = 30 * f_b;
	    }
	    else
	    {
	      motor[rightMotor] = 30 * f_b;
	      motor[leftMotor] = -30 * f_b;
	    }
	  }
	  //Stop
	  motor[rightMotor] = 0;
	  motor[leftMotor] = 0;
	  wait1Msec(250);
	}

//+++++++++++++++++++++++++++++++++++++++++++++| MAIN |+++++++++++++++++++++++++++++++++++++++++++++++
task main()
{
	clearLCDLine(0);                      // Clear line 1 (0) of the LCD
  clearLCDLine(1);                      // Clear line 2 (1) of the LCD
  bLCDBacklight = true;                 // Turn on LCD Backlight
  displayLCDPos(0,0);
	wait1Msec(2000);          // Býð í 2 sekúndur.
	while(lightSensor < 500)
	{
		startTask(stopTasks);
		displayLCDCenteredString(0, "LEFT  CNTR  RGHT");  
    displayLCDPos(1,0);                                     
    displayNextLCDNumber(SensorValue(lineFollowerLEFT));   
    displayLCDPos(1,6);                                   
    displayNextLCDNumber(SensorValue(lineFollowerCENTER)); 
    displayLCDPos(1,12);                                    
    displayNextLCDNumber(SensorValue(lineFollowerRIGHT));
		eltaLinu();
		if(SensorValue[sonicSensor] < 30) {
			motor[leftMotor]  = 0;
			motor[rightMotor] = 0;
			takaHlut(-1);
			wait1Msec(700);
			motor[leftMotor]  = -68;
			motor[rightMotor] = -68;
			wait1Msec(400);
			motor[leftMotor]  = 0;
			motor[rightMotor] = 0;
			takaHlut(1);
			turn(1);
			takaHlut(-1);
			turn(-1);
		}
	}
}
//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
```
