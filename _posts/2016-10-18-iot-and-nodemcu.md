---
author: ameyjadiye
layout: post
title: "IoT and nodemcu"
date: 2016-10-18 8:45
category : Embedded
comments: true
tags:
- IoT,nodemcu,ESP8266
---

Bought a brand new NodeMCU couple of days back and i bet its much better than Arduino. 

It has built in WiFi and that's very much like Arduino with batteries included ;) also arduino ide is compatible with NodeMCU that really sweet <3. Range of scanning Access Points is no less than any powerful Mobile Phone. Another best feature is this magic piece supports Lua :), Highlevel modern lang, fastest executing scripting language which competes performance with C, literally I have tested this, for small embedded devices to giants like <a href="https://www.nginx.com/case-studies/appnexus-chooses-nginx-plus-to-enhance-its-real-time-bidding-platform/" taarget="_blank" >AppNexus</a> having billion request per sec using Lua for its performance. 

<img width="50%" height="50%" src="{{ site.url }}/images/nodemcu.jpg"/>

Some of the interesting and core facts about NodeMCU.

NodeMCU is an open source IoT platform. It includes firmware which runs on the ESP8266 Wi-Fi SoC from Espressif Systems, and hardware which is based on the ESP-12 module. The term "NodeMCU" by default refers to the firmware rather than the dev kits. The firmware uses the Lua scripting language. It is based on the eLua project, and built on the Espressif Non-OS SDK for ESP8266. It uses many open source projects, such as lua-cjson and spiffs.

Some sample code

### Connecting to Access Point

```Lua
print(wifi.sta.getip())
--nil
wifi.setmode(wifi.STATION)
wifi.sta.config("SSID","password")
-- wifi.sta.connect() not necessary because wifi.sta.config sets auto-connect = true
tmr.create():alarm(1000, 1, function(cb_timer)
  if wifi.sta.getip() == nil then
    print("Connecting...")
  else
    cb_timer:unregister()
    print("Connected, IP is "..wifi.sta.getip())
  end
end)
```


### Controlling GPIO

```Lua
ledPin = 1
swPin = 2
gpio.mode(ledPin,gpio.OUTPUT)
gpio.write(ledPin,gpio.HIGH)
gpio.mode(swPin,gpio.INPUT)
print(gpio.read(swPin))
```


### working as http server 

```Lua
-- a simple HTTP server
srv = net.createServer(net.TCP)
srv:listen(80, function(conn)
    conn:on("receive", function(sck, payload)
        print(payload)
        sck:send("HTTP/1.0 200 OK\r\nContent-Type: text/html\r\n\r\n<h1> Hello, NodeMCU.</h1>")
    end)
    conn:on("sent", function(sck) sck:close() end)
end)
```



### Chatting with MQTT

```Lua

-- init mqtt client with keepalive timer 120sec
m = mqtt.Client("clientid", 120, "user", "password")

-- setup Last Will and Testament (optional)
-- Broker will publish a message with qos = 0, retain = 0, data = "offline"
-- to topic "/lwt" if client don't send keepalive packet
m:lwt("/lwt", "offline", 0, 0)

m:on("connect", function(con) print ("connected") end)
m:on("offline", function(con) print ("offline") end)

-- on publish message receive event
m:on("message", function(conn, topic, data)
  print(topic .. ":" )
  if data ~= nil then
    print(data)
  end
end)

-- for secure: m:connect("192.168.11.118", 1880, 1)
m:connect("192.168.11.118", 1880, 0, function(conn) print("connected") end)

-- subscribe topic with qos = 0
m:subscribe("/topic",0, function(conn) print("subscribe success") end)
-- or subscribe multiple topic (topic/0, qos = 0; topic/1, qos = 1; topic2 , qos = 2)
-- m:subscribe({["topic/0"]=0,["topic/1"]=1,topic2=2}, function(conn) print("subscribe success") end)
-- publish a message with data = hello, QoS = 0, retain = 0
m:publish("/topic","hello",0,0, function(conn) print("sent") end)

m:close();
-- you can call m:connect again

```


Happy Coding ... :)
