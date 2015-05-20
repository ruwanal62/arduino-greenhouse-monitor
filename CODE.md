
```
/*####################################################################
 FILE: dht11_functions.pde - DHT11 Usage Demo.
 VERSION: 2S0A

 PURPOSE: Measure and return temperature & Humidity. Additionally provides conversions.

 LICENSE: GPL v3 (http://www.gnu.org/licenses/gpl.html)
 GET UPDATES: https://www.virtuabotix.com/

      --##--##--##--##--##--##--##--##--##--##--
      ##  ##  ##  ##  ##  ##  ##  ##  ##  ##  ##
      ##  ##  ##  ##  ##  ##  ##  ##  ##  ##  ##
      | ##  ##  ##  ##  ##  ##  ##  ##  ##  ## |
      ##  ##  ##  ##  ##  ##  ##  ##  ##  ##  ##
      ##  ##  ##  ##  ##  ##  ##  ##  ##  ##  ##
      | ##  ##  ##  ##  ##  ##  ##  ##  ##  ## |
      ##  ##  ##  ## DHT11 SENSOR ##  ##  ##  ##
      ##  ##  ##  ##  ##FRONT ##  ##  ##  ##  ##
      | ##  ##  ##  ##  ##  ##  ##  ##  ##  ## |
      ##  ##  ##  ##  ##  ##  ##  ##  ##  ##  ##
      ##  ##  ##  ##  ##  ##  ##  ##  ##  ##  ##
      | ##  ##  ##  ##  ##  ##  ##  ##  ##  ## |
      ##  ##  ##  ##  ##  ##  ##  ##  ##  ##  ##
      ##  ##  ##  ##  ##  ##  ##  ##  ##  ##  ##
      --##--##--##--##--##--##--##--##--##--##--
          ||       ||          || (Not    ||
          ||       ||          || Used)   ||
        VDD(5V)   Readout(I/O)          Ground

  HISTORY:
  Joseph Dattilo (Virtuabotix LLC) - Version 2S0A (27 May 12)
  -Rewritten to with more powerful Versalino functionality
  Joseph Dattilo (Virtuabotix LLC) - Version 0.4.5 (11/11/11)
  -Made Library Arduino 1.0 Compatible
  Joseph Dattilo (Virtuabotix LLC) - Version 0.4.0 (06/11/11)
  -Fixed bugs (squish)
  Mod by Rob Tillaart - Version 0.3 (28/03/2011)
  Mod by SimKard - Version 0.2 (24/11/2010)
 George Hadjikyriacou - Original version (??)
#######################################################################*/


#include <dht11.h>

#include <LiquidCrystal.h>
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

dht11 DHT11;

void setup()
{
  lcd.begin(16, 2);
  DHT11.attach(A5);
  Serial.begin(9600);
  Serial.println("DHT11 TEST PROGRAM ");
  Serial.print("LIBRARY VERSION: ");
  Serial.println(DHT11LIB_VERSION);
  pinMode(10, OUTPUT); 
}

void loop()
{
  Serial.println("\n");

  int chk = DHT11.read();
  
  /*
  Serial.print("Read sensor: ");
  switch (chk)
  {
    case 0: Serial.println("OK"); break;
    case -1: Serial.println("Checksum error"); break;
    case -2: Serial.println("Time out error"); break;
    default: Serial.println("Unknown error"); break;
  }
  */
  lcd.clear();  
  /* °F */
  Serial.print("Temp: ");
  Serial.print(DHT11.fahrenheit(), 2);
  Serial.println(" F");  /* °F */
  lcd.print("Temp: ");
  lcd.print(DHT11.fahrenheit(), 2);
  lcd.print(" F");
  lcd.setCursor(0, 1);
  
  Serial.print("Dew Point: ");
  Serial.print(DHT11.dewPointFast(), 2);
  Serial.println(" F");
  lcd.print("Dew Pt: ");
  lcd.print(DHT11.dewPointFast(), 2);
  lcd.print(" F");
  digitalWrite(10, LOW);
  delay(2000);

  lcd.clear(); 
  Serial.print("Humidity : ");
  Serial.print((float)DHT11.humidity, 0);
  Serial.println("%");
  lcd.print("Humidity: ");
  lcd.print((float)DHT11.humidity, 0);
  lcd.print("%");
  lcd.setCursor(0, 1);
  
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
   
  delay(2000);
}
```