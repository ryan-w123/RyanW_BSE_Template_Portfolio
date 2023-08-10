Plant Monitor 

Watering plants is growing to be a hard task that not many can keep up with. Allow me to demonstrate my journey of a plant monitor project that makes life a lot easier; it went from an incomplete circuit that had to overcome the frustrations of loose wires and the ESP8266(WIFI) chip, all the way to a circuit that's efficient and simple. Now, the push of a button allows the water pump to send water to a plant, and all plant information(room temperature, humidity, intensity of light, and soil moisture) is sent to a screen that you can view. This project taught me that there's always an alternate path that we can take to reach a solution/goal which will make the project all the more unique and stronger. 


| Ryan W. | Greenwich High School | Mechanical Engineer | Incoming Sophomore |

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/r4cs4SHCONk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


The path of a WIFI-driven project has completely changed its trajectory during my Milestone 2. After I realized that I wouldn't be able to use a WIFI connection to display the information, I had to switch up my way of displaying all the information; so I used an OLED screen, which very clearly displays all the necessary information(room temperature, light, humidity, and soil moisture), meaning that for the base project, I added sensors for each of these parts. Furthermore, I added a button that is able to control the pump that I've added for the water delivery to the plant. Also, the code had to be drastically changed, as well as the addition of many new libraries because now I'm sending the information to the OLED screen instead of BLYNK. This was another big challenge that came into account because it was tough to figure out and make sure that everything was changed correctly, especially with a new coding language. Before my final milestone, I need to be able to map out each of the values to a relative change for the regular human to be able to understand it. What's so surprising about this idea of mapping out the values, as well as in general the whole project, is how simple it all seems until it becomes so complex and is a much tougher task to complete. 

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/mhgGyL9YesA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

In milestone 1, I created a connection between the ESP8266, my Arduino(R3) board, and the Blynk application. The ESP8266 acts as a middleman between the communication of the Arduino R3 board and the Blynk application. The ESP chip is a wifi chip that allows the connection between the circuit and the R3 board, allowing the R3 board to deliver the necessary information such as soil moisture information to be viewed on the Blynk application. In my future milestones, I'd like to use these features to be automated using the information from the circuit, which will then determine when the water pump needs to be on or off. Yet, one of the challenges occurring very frequently is the physical cable connections between the R3 board and the ESP causing larger problems with the functionality of the actual ESP wifi connection. In the future, I might have to stop using the ESP8266 due to its recurring problems, so either I'll go around this problem, or figure out how to solve this problem for future milestones. 


# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

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
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
