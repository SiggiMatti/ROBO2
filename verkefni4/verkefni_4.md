```c
#pragma config(Sensor, in1,    lineFollowerRIGHT,   sensorLineFollower)
#pragma config(Sensor, in2,    lineFollowerCENTER,  sensorLineFollower)
#pragma config(Sensor, in3,    lineFollowerLEFT,    sensorLineFollower)
#pragma config(Motor,  port1,           rightMotor,    tmotorNormal, openLoop, reversed)
#pragma config(Motor,  port10,           leftMotor,     tmotorNormal, openLoop)
#pragma config(Sensor, dgtl10, bumpSwitch,     sensorTouch)

task stopTasks() {
	while(vexRT[Btn8D] != 1 && SensorValue[bumpSwitch]!=1) {
	}
	stopAllTasks();
}
//+++++++++++++++++++++++++++++++++++++++++++++| MAIN |+++++++++++++++++++++++++++++++++++++++++++++++
task main()
{
  wait1Msec(2000);          // Býð í 2 sekúndur.
  startTask(stopTask);
  int threshold = 2000;      /* Miðvið hvenær þeir eiga að beygja   */
  while(true)
  {
    // hægri sensor sér dökkt:
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
}
//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
```
