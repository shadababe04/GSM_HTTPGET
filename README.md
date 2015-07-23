/*
* GSM_HTTPGET_22_Jul_Test1.ino
*
* Created: 7/22/2015 11:35:17 AM
* Author: shadab
*/

#include "SoftwareSerial.h"
SoftwareSerial mySerial(2,3);
void setup()
{
	mySerial.begin(9600);
	Serial.begin(9600);
	
	init_sim900();
}

void loop()
{

	serverHit();

}

int init_sim900()
{
	int ret_val=0;
	
	Serial.println("Config Sim900..");
	delay(20000);
	Serial.println("Done...");
	mySerial.flush();
	Serial.flush();
	
	if(mySerial.println("AT+CSQ"))
	{
		delay(100);
		//printSerial();
	}
	else
	{
		ret_val=0;
		
	}
	
	if (mySerial.println("AT+CGATT?"))
	{
		delay(100);
		//printSerial();
	}
	else
	{
		ret_val=0;
		
	}
	
	if (mySerial.println("AT+SAPBR=3,1,\"CONTYPE\",\"GPRS\""))
	{
		delay(2000);
		//printSerial();
	}
	else
	{
		ret_val=0;
		
	}
	
	if (mySerial.println("AT+SAPBR=3,1,\"APN\",\"internet\""))
	{
		delay(2000);
		//printSerial();
	}
	else
	{
		ret_val=0;
		
	}
	if (mySerial.println("AT+SAPBR=1,1"))
	{
		delay(2000);
		//printSerial();
		ret_val=1;
	}
	else
	{
		ret_val=0;
	}
	return ret_val;
}

int serverHit()
{
	int prev=0,cur=0,actual=0,ret_val=0;
	prev = millis();
	Serial.println(prev);
	
	if (mySerial.println("AT+HTTPINIT"))
	{
		delay(3500);
		//printSerial();
	}
	else
	{
		ret_val=0;
		
	}
	if (  mySerial.println("AT+HTTPPARA=\"URL\",\"http://210.7.66.197:8080/bDeviceData/data.html?fn=T001-2-1\""))
	
	{
		delay(1000);
		//printSerial();
		
	}
	else
	{
		ret_val=0;
		
	}
	if ( mySerial.println("AT+HTTPACTION=0"))
	{
		delay(10000);
		//printSerial();
	}
	else
	{
		ret_val=0;
		
	}
	
	if ( mySerial.println("AT+HTTPREAD"))
	{
		printSerial();
		delay(300);
		
	}
	else
	{
		ret_val=0;
		
	}
	if (mySerial.println("AT+HTTPTERM"))
	{
		//printSerial();
		delay(300);
		ret_val=1;
	}
	else
	{
		ret_val=0;
	}
	
	mySerial.println("");
	cur = millis();
	actual = cur - prev;
	Serial.print("Loop runtime is:");
	Serial.println(actual);
	
	return ret_val;
}

void printSerial()
{
	char *data;
	int i=0, j=0;
	data = (char*)malloc(sizeof(char)*100);
	//data = (char*)malloc(sizeof(char));
	//memset(data,0,sizeof(char));
	memset(data,0,sizeof(char)*100);
	// Serial.print("Testing");
	while(mySerial.available()!=0)
	{
		//Serial.write(mySerial.read());
		*(data+i) = mySerial.read();
		//realloc(data,1);
		i++;
	}
	while(*(data+j)!=0)
	{
		Serial.write(*(data+j));
		
		j++;
	}
	free(data);
}
