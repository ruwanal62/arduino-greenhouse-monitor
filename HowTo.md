# Introduction #

So before we start, you will need some materials that are listed below. All the links on where to purchase them, costs, and data sheets are in the Materials page.

  * Wires
  * 1 Mega Ohm Resistor
  * Arduino Uno
  * Microtivity LCD Module
  * Virtuabotix DHT11 Sensor
  * Piezo Speaker

# Set-up #

The first step is acquiring all your materials. Subsequently, it is pretty straightforward from here. The three main things that we will focus on now is operating the Microtivity LCD Module, using the Virtuabotix DHT11 Sensor, and then incorporating everything together with a Piezo Speaker and a Moisture Sensor.

### Microtivity LCD Module ###

The most important thing to do first before you start using your Microtivity LCD Module is to solder the included 16-pin female header to the unit. I had initially tried just using the 16-pin male header and discovered that there was no full contact between the different pins and I could not get the Microtivity LCD Module to correctly function.

Now there are several tutorials on how to set up the Microtivity LCD Module, but the one that I found to be most helpful was the one provided by Arduino. Now there is a slight modification to the actual wiring of the Microtivity LCD Module, so I have included a schematic below that is identical to my setup. It is important to note that the last two pins on the right are for the backlight display, and that there is an included diode you should use for a voltage drop.

![http://arduino-greenhouse-monitor.googlecode.com/files/Schematic.jpg](http://arduino-greenhouse-monitor.googlecode.com/files/Schematic.jpg)

Using the Liquid Crystal library included with the Arduino software is by far the easiest way to function the Microtivity LCD Module. Here is a little sample code of how I used the Liquid Crystal library to display text on the Microtivity LCD Module. The actual code can be accessed in the Downloads section or in the Code page. What is more unique about that code is that there is a delay of 2 seconds before the next round of information is displayed. The function _lcd.clear() is used to clear the display information for the next round of display information._

```
lcd.print("Temp: ");
  lcd.print(DHT11.fahrenheit(), 2);
  lcd.print(" F");
  lcd.setCursor(0, 1);
```

http://arduino.cc/en/Tutorial/LiquidCrystal

![http://arduino-greenhouse-monitor.googlecode.com/files/2013-04-21%2015.36.14.jpg](http://arduino-greenhouse-monitor.googlecode.com/files/2013-04-21%2015.36.14.jpg)

### Virtuabotix DHT11 Sensor ###

The Virtuabotix DHT11 Sensor is a very easy sensor to use, especially with the libraries that you may download for it. The wiring is very simple as well, it is just important to notice that the DTA pin of the Virtuabotix DHT11 Sensor is connected to the Arduino Analog Pin 5. The NC pin on the Virtuabotix DHT11 Sensor is left alone. VDD naturally goes to +5V provided by the Arduino and the GND goes to ground of the Arduino.

In my code, I had essentially used the classic example code that Virtuabotix and supplied and modified it. Now one of the larger modifications I had performed in the code was in the libraries that Virtuabotix provided.

In the dht11.h file, you must create a new double variable.

` double dewPoints(); `

http://arduino-greenhouse-monitor.googlecode.com/files/dht11.h

Then, you must create a new keyword in the keywords.txt file.

` dewPoints	KEYWORD2 `

http://arduino-greenhouse-monitor.googlecode.com/files/keywords.txt

The last part is the Dew Point Calculation and then translating that into Fahrenheit. This is done as shown below in the dht11.cpp file.

```

double dht11::dewPoints()
{
	read();//make sure the temp has been read

	double A0= 373.15/(273.15 + temperature);
	double SUM = -7.90298 * (A0-1);
	SUM += 5.02808 * log10(A0);
	SUM += -1.3816e-7 * (pow(10, (11.344*(1-1/A0)))-1) ;
	SUM += 8.1328e-3 * (pow(10,(-3.49149*(A0-1)))-1) ;
	SUM += log10(1013.246);
	double VP = pow(10, SUM-3) * humidity;
	double T = log(VP/0.61078);   // temp var
	double DEWW = (241.88 * T) / (17.558-T);
	return 1.8 * DEWW + 32;
}
```

http://arduino-greenhouse-monitor.googlecode.com/files/dht11.cpp

All of these files are included in the Downloads Section.

### Wrapping it all together ###

So now we know how to incorporate the Microtivity LCD Module and the Virtuabotix DHT11 Sensor. Now it is time to incorporate the Moisture Sensor and the Piezo Speaker.

For the Piezo Speaker it is important to declare the Arduino Digital Output Pin as an Output and indicate which one. In our case according to the code, it is Pin 10. Using an _analogWrite(10,128);_ we can turn the Piezo Speaker on for a short while until we re-write it and turn it off _digitalWrite(10, LOW);_.

The Moisture Sensor is very easy to implement, and is essentially along the same lines of a voltage divider. The first step is to connect a resistor to +5V source. Then, attach a lead from a single wire to ground and then using a separate second wire, connect it to the other end of the resistor. At this point, attach a wire that intersects the voltage divider after the resistor to, (in our case), Arduino Analog Pin 1. It may be helpful to twist the two wires together at the end to keep a consistent distance between the two leads!

![http://arduino-greenhouse-monitor.googlecode.com/files/2013-04-21%2015.35.45.jpg](http://arduino-greenhouse-monitor.googlecode.com/files/2013-04-21%2015.35.45.jpg)

From this point, it is helpful to determine what sort of ranges yield your values. It is really different on each type of resistor you may choose to use and it also depends on the type of moisture that you'd like to maintain. The code of this displayed below.

```
  int moisture = analogRead(A1);
  Serial.print("Moisture: ");
  Serial.print(moisture);
  
  if (moisture <= 59){
    lcd.print("Soil is too wet!");
    analogWrite(10,128);
  }
  else if ( moisture >= 60 && moisture <= 350 ){
    lcd.print("Soil is great!:)");
  }
  else if ( moisture >= 351 ){
    lcd.print("Soil is too dry!");
    analogWrite(10,128);
  }
```

As you can see, if the soil is too dry or too wet the Piezo Speaker is turned on.

And now you are done!!!!

<a href='http://www.youtube.com/watch?feature=player_embedded&v=MjGPdN2qjmk' target='_blank'><img src='http://img.youtube.com/vi/MjGPdN2qjmk/0.jpg' width='425' height=344 /></a>

![http://arduino-greenhouse-monitor.googlecode.com/files/2013-04-21%2015.35.27.jpg](http://arduino-greenhouse-monitor.googlecode.com/files/2013-04-21%2015.35.27.jpg)

