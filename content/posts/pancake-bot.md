---
categories: ["projects"]
date: 2019-03-10T10:59:00+01:00
tags: ["projects"]
title: "Pancakebot: An over-the-top fully automated crêpe maker."
show-meta: false
toc: false
featured_image: /pancakebot/pancakebot.jpg
---

Sometimes, the simplest problems requires the most over-engineered, complicated and ridicoulus solutions. Pancakebot is one of those.

It all started as a mechatronics project on DTU with a couple of fellow students (Frederik, Emilie, Sofie, Lasse and Ditte for credits). We were all incredibly dissatisfied with the time-consuming and boring process of baking crêpes, and decided to do something radical about it. After 3 intense weeks of staying up late, coding, cutting and gluing, our prayers where heard: Panckebot!

![PancakeBot](/pancakebot/pancakebot.jpg)
##### Pancakebot, in all its maginificance

## Actuator frenzy!

Pancakebot contains no less than 10 actuators and sensors, making this project oversize, overkill and almost impossible to finish in 3 weeks. Heres a complete list:

* An ultrasonic distance sensor for measuring the amount of pancake dough left in the container
* A 230V relay for toggling the pancake heater
* A peristaltic pump for dispensing dough for each pancake
* A crane for opening and closing the pancake heater lid (yes you read that right, a custom built crane with two servos)
* A linear actuator for pushing of the pancake when it's done (not even that ridiculous, our initial idea was a fan for blowing it off)
* A screen with buttons to order any amount of pancakes, with feedback if one of the sensors screws up.
* A physical arrow with numbers, indicating the progress of pancake cooking.
* A weight measuring if the pancake successfully landed on the serving plate.
* A servo rotating the measurement cup for dough, tilting the dough into the heater.

![PancakeBot](/pancakebot/crane.gif)![PancakeBot](/pancakebot/linearactuator.gif)![PancakeBot](/pancakebot/linearactuator2.gif)

## Controller
All these components were controlled by a single Arduino Mega, and oh my did we regret that. Connections didn't fix, the relay crashed and reset everything and we couldn't found our way in all the wires. When everything worked independently and we assembled all components, noise from the components made everything crash. Many sleepless nights where spent in front of the beautiful Arduino IDE.

## Totally Worth It
In spite of the long days, even longer nights, non-functioning tech, hazardous laser-cutting and 3D-printing fumes, it was absolutely worth it. Why? Because of the astronomic amount of pancakes we ate during those three weeks.
{{< youtube GzSxFrhAfVE >}}