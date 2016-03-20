---
author: ameyjadiye
layout: post
title: "LM35->Arduino->RPi2"
date: 2016-03-20 22:45
category : Embedded
comments: true
tags:
- raspberrypi, arduino
---

Just to add more fun in tech stuff i have started taking intrest in physical computing and electronics, this extends software field to embedded and electronics where i feel connected to realworld ;)

I did a small hobby project, goal of the project is just print a cool graph of temprature trend throughout a day at my home.

## Required stuff :

We are going to need few stuff like LM35DZ, Arduino, RaspberryPi, USB cable, jumper wires and bit of intresting mind.  

### LM35DZ (temprature sensor) :
There are  veriety of temprature scensors avilabel in market but this one is really good at what it made for and its very cheap compared to other sensors, both reasons are equilly important to convinced myself to buy this little guy after a day of reserch.

[<img src="{{ site.url }}/images/lm35.jpg" style="height: 8%;width: 8%;"/>]({{ site.url }}/images/lm35.jpg "LM35DZ")


### Arduino (UNO R3)
This guy is really awesome when we talk about interacting with physical things like light, motion, temprature, humidity, directions, speed, inshort any type of sensors, its a best microcontroller avilabel for prototyping projects.

[<img src="{{ site.url }}/images/arduinouno.jpg" style="height: 8%;width: 8%;"/>]({{ site.url }}/images/arduinouno.jpg "Arduino UNO R3")

### RaspberryPI (B2)
This is the altimate brain of the project, this is a microprocessor which will be driving all things. 

[<img src="{{ site.url }}/images/rpi2.jpg" style="height: 8%;width: 8%;"/>]({{ site.url }}/images/rpi2.jpg "Raspberry Pi 2")

## Kick Off

Circuit is preety simple connect LM35 to arduino, load arduino program which poll LM35 each second and send output to serial port, connect raspberry pi to arduino and read serial output write to file, we wil use this dump for generating cool graphs.

[<img src="{{ site.url }}/images/lm35unorpi.jpg" style="height: 30%;width: 30%;"/>]({{ site.url }}/images/lm35unorpi.jpg "LM35->Arduino->RPi2")

### Arduino Code
{% highlight c %}

float tempC;
int reading;
int tempPin = 0;

void setup()
{
analogReference(INTERNAL);
Serial.begin(9600);
}

void loop()
{
reading = analogRead(tempPin);
tempC = reading / 9.31;
Serial.println(tempC);
delay(1000);
}

{% endhighlight %}

### RPi Code

{% highlight python %}

#!/usr/bin/python

import serial
import datetime

ser = serial.Serial('/dev/ttyACM0', 9600)
while True:
	file = open('temp.out', 'a')
	temprature = ser.readline().strip()
	ts=datetime.datetime.today().strftime('%Y-%m-%d %H:%M:%S')	
	line=ts+"|"+temprature
	file.write(line)
	file.write("\n")
	#print line
	file.close()


{% endhighlight %}

## Output

[<img src="{{ site.url }}/images/tmptrend.gif" style="height: 60%;width: 60%;"/>]({{ site.url }}/images/tmptrend.gif "trend")



