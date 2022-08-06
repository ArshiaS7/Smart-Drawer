<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links

[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![MIT License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]
-->

<!-- PROJECT LOGO -->
<div align="center">

  <img src=https://user-images.githubusercontent.com/54024838/183236427-fb8f542f-3cfa-4f5e-8f01-a7178bebbb9c.png alt="Logo" width="300">

  <h1 align="center">Smart-Drawer</h1>

  <p align="center">
  Smart-Drawer with Stepper motor interfacing AVR MicroController & Ultrasonic Sensor
    <br />
    <br />
    <a href="https://github.com/othneildrew/Best-README-Template"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/othneildrew/Best-README-Template">View Demo</a>
    .
    <a href="https://github.com/othneildrew/Best-README-Template/issues">Report Bug</a>
    .
    <a href="https://github.com/othneildrew/Best-README-Template/issues">Request Feature</a>
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li>
          <a href="#Ultrasonic-sensor">Ultrasonic Sensor</a>
             <ul style="list-style-type:disc">
               <li><a href="#Code">C Code</a></li>
             </ul>
        </li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>


<!-- ABOUT THE PROJECT -->
## About The Project

Handsfree-Drawer allows you to open and close drawer without the interference of any handles.


### Built With

Componenets and Modules which were used in this project, are listed as follow:

- HC-SR04 Ultrasonic Sensor
- ATmega16A Microcontroller
- STP-58D Unipolar Stepper Motor
- TB6600 Stepper MicroDriver


<!-- GETTING STARTED -->
## Getting Started

This is an example of how you may give instructions on setting up your project locally.
To get a local copy up and running follow these simple example steps.

### Ultrasonic Sensor
<div align="justify">
  
For this project, we used HC-SR04 ultra sonic sensor. Ultrasonic sensors (or transducers) work similarly to radar systems. Acoustic waves are generated by ultrasonic sensors when electrical energy is converted into them. The acoustic wave signal is an ultrasonic wave traveling at a frequency above 18kHz. The famous HC SR04 ultrasonic sensor generates ultrasonic waves at 40kHz frequency. Typically, a microcontroller is used for communication with an ultrasonic sensor. We used ATmega16 AVR microcontroller in this case. To begin measuring the distance, the microcontroller sends a trigger signal to the ultrasonic sensor. The duty cycle of this trigger signal is 10µS for the HC-SR04 ultrasonic sensor. When triggered, the ultrasonic sensor generates eight acoustic (ultrasonic) wave bursts and initiates a time counter. As soon as the reflected (echo) signal is received, the timer stops. The output of the ultrasonic sensor is a high pulse with the same duration as the time difference between transmitted ultrasonic bursts and the received echo signal.
  
</div> 

<div align = "center"> 
  <img src = "https://user-images.githubusercontent.com/47887796/182787238-3abde9bc-c56c-4752-8739-b8b54be96aa1.png" width = "600"> 
<br><br><br>
  <img src = "https://user-images.githubusercontent.com/47887796/182788257-ed91de5a-b888-429a-900a-bd455d68451f.png" width = "600"> 
</div>



<br> For the code, we have to enable intrupt and write a function to measure distance:

### Code:

``` 
long timer = 0, xTimer = 0;
float pulsa, distance;
// Timer1 overflow interrupt service routine
interrupt [TIM1_OVF] void timer1_ovf_isr(void)
{
   xTimer++;
}

void ultra() {
   TRIGGER = 1;         
   delay_us(10);
   TRIGGER = 0;
   while(ECHO == 0);    
   TCNT1 = 0;           
   xTimer = 0;          
   while(ECHO == 1);    
   timer = TCNT1;      
   pulsa = (float)xTimer * 65535 * 0.5 + (float)timer * 0.5;   
   distance = pulsa / 29.034 / 2;   
}
```
