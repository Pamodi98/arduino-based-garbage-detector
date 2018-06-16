# arduino-based-garbage-detector
We are living in an age where tasks and systems are fusing together with the power of IOT to have a more efficient system of working and to execute jobs quickly! With all the power at our finger tips this is what we have come up with.
The Internet of Things (IoT) shall be able to incorporate transparently and seamlessly many different systems, while providing data for millions of people to use and capitalize. Building a general architecture for the IoT is hence a very complex task, mainly because of the extremely large variety of devices, link layer technologies, and services that may be involved in such a system.
One of the main concerns with our environment has been solid waste management which impacts the health and environment of our society. The detection, monitoring and management of wastes is one of the primary problems of the present era. The traditional way of manually monitoring the wastes in waste bins is a cumbersome process and utilizes more human effort, time and cost which can easily be avoided with our present technologies.
This is our solution, a method in which waste management is automated. This is our IoT Garbage Monitoring system, an innovative way that will help to keep the cities clean and healthy.
Follow on to see how you could make an impact to help clean your community, home or even surroundings, taking us a step closer to a better way of living.

IMPLEMENTED ARDUINO CODE
#define BLYNK_PRINT Serial
#include <SPI.h>
#include <Ethernet.h>
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#define TRIGGERPIN D1
#define ECHOPIN    D2
// You should get Auth Token in the Blynk App.
// Go to the Project Settings (nut icon).
char auth[] = "67307562c69f46f9bdd6ddcf1d7d8ef4";

// Your WiFi credentials.
// Set password to "" for open networks.
char ssid[] = "Dialog 4G";
char pass[] = "sparkle ";

WidgetLCD lcd(V1);
WidgetLED ledgreen(V2);
WidgetLED ledred(V3);

void setup()
{
  // Debug console
  Serial.begin(9600);
pinMode(TRIGGERPIN, OUTPUT);
  pinMode(ECHOPIN, INPUT);
  Blynk.begin(auth, ssid, pass);
  // You can also specify server:
  //Blynk.begin(auth, ssid, pass, "blynk-cloud.com", 8442);
  //Blynk.begin(auth, ssid, pass, IPAddress(192,168,1,100), 8442);

 // lcd.clear(); //Use it to clear the LCD Widget
  lcd.print(0, 0, "Distance in cm"); // use: (position X: 0-15, position Y: 0-1, "Message you want to print")
  //Blynk.email("kavindufonseka@gmail.com","GARBAGE NOTIFICATION","Please collect your garbage bins");
  // Please use timed events when LCD printintg in void loop to avoid sending too many commands
  // It will cause a FLOOD Error, and connection will be dropped
}

void loop()
{
  //lcd.clear();
 // lcd.print(0, 0, "Distance in cm"); // use: (position X: 0-15, position Y: 0-1, "Message you want to print")
  long t, distance;
  digitalWrite(TRIGGERPIN, LOW);  
  delayMicroseconds(3); 
  
  digitalWrite(TRIGGERPIN, HIGH);
  delayMicroseconds(12); 
  
  digitalWrite(TRIGGERPIN, LOW);


   t=pulseIn(ECHOPIN,HIGH);
   distance= t*340/20000;
  delay(1000);
//long cm=t / 29 / 2;
 Serial.print(distance);
  Serial.println("cm");
  
  
  //lcd.print(0, 0,cm);

  if(distance<5)
  {
    Serial.print("basket is full\n");
     lcd.print(1,1,"full");
     ledred.on();
     ledgreen.off();
     Blynk.notify("you may collect your garbage bins now");
     Blynk.email("kavindufonseka@gmail.com","GARBAGE NOTIFICATION","Please collect your garbage bins");
  }else
  {
    Serial.print("basket is empty\n");
     lcd.print(1,1,"empty");
     ledgreen.on();
     ledred.off();
     
  }
  Blynk.run();

  delay(1000);

}

