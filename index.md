Plant Monitor 

Watering plants is growing to be a hard task that not many can keep up with. Allow me to demonstrate my journey of a plant monitor project that makes life a lot easier; it went from an incomplete circuit that had to overcome the frustrations of loose wires and the ESP8266(WIFI) chip, all the way to a circuit that's efficient and simple. Now, the push of a button allows the water pump to send water to a plant, and all plant information(room temperature, humidity, intensity of light, and soil moisture) is sent to a screen that you can view. This project taught me that there's always an alternate path that we can take to reach a solution/goal which will make the project all the more unique and stronger. 


| Ryan W. | Greenwich High School | Mechanical Engineer | Incoming Sophomore |

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/dMqqA7JS2iE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

The last milestone was a success! In my last milestone, I added multiple different elements to the project in order to make it look more organized and complex. So to make it look more organized, I created a housing using an old tissue box. I then cut holes in the side of the box in order for components such as the soil moisture module, the USB cable, and the two pumps to be able to come out of the box for their use. Then, since it's a tissue box, there's also a hole at the top of the box, which allows the sensors to pick up on the necessary information, and then the OLED screen that I had recently added to the circuit to view all that information, can be seen since the OLED is sticking through the top. This OLED is definitely one of the most triumphant components that I'm very proud of in my project while the biggest challenge was coding everything for the OLED screen and changing it from the Blynk application to the OLED. SO ultimately, I learned a lot about the functionality of multiple different sensors, coding in Arduino IDE, and how breadboards work. After I leave BSE, I would like to continue learning about different parts and gaining a deep understanding of them so that I can continue to make new projects and/or add to my current project, which allowed me to have a great experience at BSE. 


# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/r4cs4SHCONk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


The path of a WIFI-driven project has completely changed its trajectory during my Milestone 2. After I realized that I wouldn't be able to use a WIFI connection to display the information, I had to switch up my way of displaying all the information; so I used an OLED screen, which very clearly displays all the necessary information(room temperature, light, humidity, and soil moisture), meaning that for the base project, I added sensors for each of these parts. Furthermore, I added a button that is able to control the pump that I've added for the water delivery to the plant. Also, the code had to be drastically changed, as well as the addition of many new libraries because now I'm sending the information to the OLED screen instead of BLYNK. This was another big challenge that came into account because it was tough to figure out and make sure that everything was changed correctly, especially with a new coding language. Before my final milestone, I need to be able to map out each of the values to a relative change for the regular human to be able to understand it. What's so surprising about this idea of mapping out the values, as well as in general the whole project, is how simple it all seems until it becomes so complex and is a much tougher task to complete. 

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/mhgGyL9YesA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

In milestone 1, I created a connection between the ESP8266, my Arduino(R3) board, and the Blynk application. The ESP8266 acts as a middleman between the communication of the Arduino R3 board and the Blynk application. The ESP chip is a wifi chip that allows the connection between the circuit and the R3 board, allowing the R3 board to deliver the necessary information such as soil moisture information to be viewed on the Blynk application. In my future milestones, I'd like to use these features to be automated using the information from the circuit, which will then determine when the water pump needs to be on or off. Yet, one of the challenges occurring very frequently is the physical cable connections between the R3 board and the ESP causing larger problems with the functionality of the actual ESP wifi connection. In the future, I might have to stop using the ESP8266 due to its recurring problems, so either I'll go around this problem, or figure out how to solve this problem for future milestones. 


# Schematics 

![Milestone 1 Schematic](/RyanW_BSE_Template_Portfolio/Milestone1_Schematic.png)

# ⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️

![Milestone 2 Schematic](/RyanW_BSE_Template_Portfolio/Milestone2.png)


# ⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️


![Milestone 3 Schematic](/RyanW_BSE_Template_Portfolio/Milestone 3.png)


# Code


```c++
int readMoisture() {
//both these commands are calling the sensor from the initializations and then finding out these values. 
  return analogRead(moisturePin);
}

int readLight() {
  return analogRead(lightPin);
}


bool readDHT() {
//Both are using the DHT to get the values from the sensors
  roomHumidity = dht.readHumidity();
  delay(100);
  roomTemperature = dht.readTemperature();
  return true;
}

void myTimerEvent() {
  bool chk = readDHT();
  int light = readLight();
  int moisture = readMoisture();
  if (chk == true) {
//printing out the values of the Room temp and humidity to the OLED screen
    display.setTextColor(WHITE);
    display.setTextSize(1);
    display.setFont();
    display.setCursor(0, 43);
    display.println("Room Temp  :");
    display.setCursor(80, 43);
    display.println(roomTemperature);
    display.setCursor(114, 43);
    display.println("C");
    display.setCursor(0, 56);
    display.println("Humidity   :");
    display.setCursor(80, 56);
    display.println(roomHumidity);
  }

//Printing out the light and moisture to specific positions on the OLED 

  display.setTextColor(WHITE);
  display.setTextSize(1);
  display.setFont();
  display.setCursor(0, 16);
  display.println("Light      :");
  display.setCursor(80, 16);
  display.println(light);
  display.setCursor(0, 30);
  display.println("Moisture   :");
  display.setCursor(80, 30);
  display.println(moisture);


  //pumpA
//"edge detection" --> when the button is being turned from on to off 
  oldBtnVal = btnVal;
  btnVal = digitalRead(inbtn);
  //Button Press from high --> low --> that turns the pump on or off
  if (btnVal == LOW && oldBtnVal == HIGH) {
    if (currentState == HIGH) {
      Serial.println("current state = low");
      currentState = LOW;
    } else if (currentState == LOW) {
      currentState = HIGH;
      Serial.println("current state = high");
    } else {
      //Self-error check
      Serial.println("The button has an undefined value");
    }
    digitalWrite(pumpA, currentState);
  }
//same thing but for second pump
  //pumpB
  oldBtnVal2 = btnVal2;
  //whether the button has been pressed or not
  btnVal2 = digitalRead(inbtn2);
  //Button Press from high --> low
  if (btnVal2 == LOW && oldBtnVal2 == HIGH) {
      if (currentState2 == HIGH) {
      Serial.println("current state 2 is high ");
      currentState2 = LOW;
    }
    else if (currentState2 == LOW) {
      currentState2 = HIGH;
    }
    else {
      Serial.println("The button has an undefined value");
    }
    digitalWrite(pumpB, currentState2);
  }
}


```
This part is the most important in the code. To start off, the code starts off by reading each of the values from the sensors. Then the start of the myTimerEvent() is what allows the user to view the information on the OLED screen. Next in the myTimerEvent() is how the buttons control the pump, and allow it to function. Essentially what we are doing is that we are taking into consideration how long a user might hold the button, so what we have coded here is that even if the button is held for a long time, the button will have the same function(either turning the pump on or off). This represents a concept called edge detection. For example, if I were to hold the button for a long time, then it will only output one answer of on or off, but without edge detection, the value of whether the pump is on or off will be inconsistent and unknown. Ultimately, this allows the user to turn the pump on or off with consistency and without any problems. 

```c++
void setup() {
  // Debug console
  Serial.begin(115200);
  dht.begin();
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3c)) {  // Address  0x3D for 128x64
    Serial.println(F("SSD1306 allocation failed"));
  }

  pinMode(inbtn, INPUT_PULLUP);
  pinMode(inbtn2, INPUT_PULLUP);
  pinMode(pumpA, OUTPUT);
  pinMode(pumpB, OUTPUT);
}

void loop() {
  display.clearDisplay();
  myTimerEvent();  // Initiates timer function
  display.display();
  delay(200);
}
```
To end, the setup() function allows each of the previous functions to run in order to work due to C++ in Arduino being objected oriented programming. Furthermore, the dht.begin() is crucial in this case because if it isn't called, then the dht won't be able to startup, meaning we won't be able to see the information provided by the dht; if it isn't working, the following error statement will print. Lastly, the loop is simply going through each of the functions repeatedly to update each of the values on the OLED screen. 


# Bill of Materials

| Sun Founder Arduino R3 Board | This was what controlled the whole circuit that I'd created and it was where all of the analog and digitial pins would go into. Furthermore, it was the power supply of the whole circuit with a USB cable and a 9-volt battery attached to it. | $16.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/SunFounder-Board-Arduino-ATMEGA328P-ATMEGA16U2/dp/B08353DL5P/ref=sr_1_6?crid=21AG7IRUNDT1D&keywords=SunFounder+R3+Board&qid=1691761480&sprefix=sunfounder+r3+board%2Caps%2C216&sr=8-6)"> Link </a> |
| Photoresistor | This is one of the three sensors that I used in my project. The photoresistor is a light sensor, and for my project, it would detect the amount of light in the room. | $7.39 | <a href="(https://www.amazon.com/HiLetgo-Dependent-Photoresistor-Photoconductive-Resistance/dp/B00N1ZJUN4/ref=sr_1_4?crid=V1RQPTM3GTMQ&keywords=Photoresistor+elegoo&qid=1691761587&sprefix=photoresistor+elegoo%2Caps%2C93&sr=8-4)https://www.amazon.com/HiLetgo-Dependent-Photoresistor-Photoconductive-Resistance/dp/B00N1ZJUN4/ref=sr_1_4?crid=V1RQPTM3GTMQ&keywords=Photoresistor+elegoo&qid=1691761587&sprefix=photoresistor+elegoo%2Caps%2C93&sr=8-4"> Link </a> |
| DHT11 Temperature and Humidity Sensor Module | This is my second sensor, but it can detect two different things. The DHT11 detects both the temperature and humidity in a room, allowing me to view those values.| $8.99 | <a href="(https://www.amazon.com/DIYables-Temperature-Humidity-Arduino-Raspberry/dp/B0BPFZ54QQ/ref=sr_1_2?crid=36VUBISJIBBVG&keywords=DHT11+Humiture+Sensor+elegoo&qid=1691761645&sprefix=dht11+humiture+sensor+elegoo%2Caps%2C84&sr=8-2)https://www.amazon.com/DIYables-Temperature-Humidity-Arduino-Raspberry/dp/B0BPFZ54QQ/ref=sr_1_2?crid=36VUBISJIBBVG&keywords=DHT11+Humiture+Sensor+elegoo&qid=1691761645&sprefix=dht11+humiture+sensor+elegoo%2Caps%2C84&sr=8-2"> Link </a> |
|  Soil Moisture Sensor | This is the third module that I used in my project. The soil moisture sensor is used to detect how moisturized the soil is( values of how wet or dry). So, when a plant's soil was very wet, the moisture would decrease down to a lower number than if it were dry soil.   | $8.68 | <a href="(https://www.amazon.com/Gikfun-Capacitive-Corrosion-Resistant-Detection/dp/B07H3P1NRM/ref=sr_1_12?crid=285ABR4I4DY85&keywords=Soil+Moisture+Module+elegoo+2+pack&qid=1691761810&sprefix=soil+moisture+module+elegoo+2+pack%2Caps%2C103&sr=8-12)"> Link </a> |
| Mini Water Pump | This DC mini water pump was used mainly for testing because I wanted to see if the general concept of using a pump would work. But, in my larger goal, the pump would've been used to push water through the tubing, and ultimately provide water to the plant. Also, I used a second pump as a demonstration that I'd be providing fertilizer to the plant.  | $10.36 | <a href="(https://www.amazon.com/Sipytoph-Submersible-Flexible-Aquariums-Hydroponics/dp/B097F4576N/ref=sr_1_10?crid=26A14MLYTSWWB&keywords=DC+mini+water+pump+DC&qid=1691594612&sprefix=dc+mini+water+pump+dc%2Caps%2C134&sr=8-10)](https://www.amazon.com/Sipytoph-Submersible-Flexible-Aquariums-Hydroponics/dp/B097F4576N/ref=sr_1_10?crid=26A14MLYTSWWB&keywords=DC+mini+water+pump+DC&qid=1691594612&sprefix=dc+mini+water+pump+dc%2Caps%2C134&sr=8-10)"> Link </a> |
| Tactile Push Button | The tactile push button was used for a single task, and that was to control the pumps. The physical use of the tactile push button avoided me from having to use the Serial Monitor on the Arduino IDE, making my project a lot more complex.  | $5.99 | <a href="(https://www.amazon.com/Taiss-Momentary-Installation-Breadboard-Miniature/dp/B08DRB78MX/ref=sr_1_3?crid=17TAO0H99KUW7&keywords=tactile%2Bpush%2Bbutton%2Belegoo&qid=1691761958&sprefix=tactile%2Bpush%2Bbutton%2Belegoo%2Caps%2C87&sr=8-3&th=1)https://www.amazon.com/Taiss-Momentary-Installation-Breadboard-Miniature/dp/B08DRB78MX/ref=sr_1_3?crid=17TAO0H99KUW7&keywords=tactile%2Bpush%2Bbutton%2Belegoo&qid=1691761958&sprefix=tactile%2Bpush%2Bbutton%2Belegoo%2Caps%2C87&sr=8-3&th=1"> Link </a> |
| L298N Motor Driver Controller Board Module | The Motor Driver was used to give power to the pumps that I used. This motor driver had two inputs for pumps, making it very efficient and effective because this way I didn't have to use two different Motor Driver Controllers. | $11.49 | <a href="(https://www.amazon.com/HiLetgo-Controller-Stepper-H-Bridge-Mega2560/dp/B07BK1QL5T/ref=sr_1_1_sspa?crid=27QK3JJ4TCF6K&keywords=L298N&qid=1691762009&sprefix=l298n%2Caps%2C102&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)https://www.amazon.com/HiLetgo-Controller-Stepper-H-Bridge-Mega2560/dp/B07BK1QL5T/ref=sr_1_1_sspa?crid=27QK3JJ4TCF6K&keywords=L298N&qid=1691762009&sprefix=l298n%2Caps%2C102&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)"> Link </a> |
| OLED Display Module | The OLED Display Module was used to display all the information from the sensors. | $14.99 | <a href="(https://www.amazon.com/Hosyond-Display-Self-Luminous-Compatible-Raspberry/dp/B09C5K91H7/ref=sr_1_1_sspa?crid=PMMRH6RM9US9&keywords=arduino+oled+display&qid=1691763129&sprefix=Arduino+Ol%2Caps%2C115&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)https://www.amazon.com/Hosyond-Display-Self-Luminous-Compatible-Raspberry/dp/B09C5K91H7/ref=sr_1_1_sspa?crid=PMMRH6RM9US9&keywords=arduino+oled+display&qid=1691763129&sprefix=Arduino+Ol%2Caps%2C115&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=11"> Link </a> |
