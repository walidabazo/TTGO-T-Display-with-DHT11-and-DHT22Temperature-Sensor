# Project ESP32 development using the TTGO T-DISPLAY
# TTGO-T-Display-with-DHT11-and-DHT22Temperature-Sensor


This tutorial shows how to use the DHT11 and DHT22 temperature and humidity sensors with the TTGO T-DISPLAY using Arduino IDE.
We will go through a quick introduction to these sensors


      /**************************************************************************
        TTGO T-Display ST7789 OLED based on Adafruit example
        https://github.com/adafruit/Adafruit-ST7735-Library/blob/master/examples/graphicstest/graphicstest.ino
       **************************************************************************/
        #include <Adafruit_GFX.h>    // Core graphics library
        #include <Adafruit_ST7789.h> // Hardware-specific library for ST7789
        #include <SPI.h>
        #include <DHT.h>

       
          // pinouts from https://github.com/Xinyuan-LilyGO/TTGO-T-Display
          #define TFT_MOSI 19
          #define TFT_SCLK 18
          #define TFT_CS 5
         #define TFT_DC 16
         #define TFT_RST 23
         #define TFT_BL 4

         #define DHTPIN 17
         #define DHTTYPE DHT11

         // constructor for data object named tft 
         Adafruit_ST7789 tft = Adafruit_ST7789(TFT_CS, TFT_DC, TFT_MOSI, TFT_SCLK, TFT_RST);
         DHT dht(DHTPIN, DHTTYPE);
         void setup(void) 
         {
  
           Serial.begin(9600); 
           Serial.print(F("Hello! ST77xx TFT Test"));
           pinMode(TFT_BL, OUTPUT);      // TTGO T-Display enable Backlight pin 4
           digitalWrite(TFT_BL, HIGH);   // T-Display turn on Backlight
           tft.init(135, 240);           // Initialize ST7789 240x135
           Serial.println(F("Initialized"));
           tft.setRotation(1);
           dht.begin();

          }

         void loop() 
         {
           // tft print function!
           tftPrintTest();
           delay(4000); 
         }


         void tftPrintTest() {

      int h = dht.readHumidity();
      int h = dht.readHumidity();
      //Read the moisture content in %.
      int t = dht.readTemperature();
     //Read the temperature in degrees Celsius
     float f = dht.readTemperature(true);
     // true returns the temperature in Fahrenheit

     if (isnan(h) || isnan(t) || isnan(f)) {
     Serial.println("Failed reception");
     return;
     //Returns an error if the ESP32 does not receive any measurements
     }

       Serial.print("Humidite: ");
       Serial.print(h);
       Serial.print("%  Temperature: ");
       Serial.print(t);
       Serial.print("°C, ");
       Serial.print(f);
       Serial.println("°F");
   
       tft.setTextWrap(false);
       tft.fillScreen(ST77XX_BLACK);
       tft.setCursor(0, 20);
       tft.setTextColor(ST77XX_YELLOW);
       tft.setTextSize(6);
       tft.print(h);
       tft.setTextColor(ST77XX_YELLOW);
       tft.setTextSize(6);
       tft.println(" % ");
       tft.setTextColor(ST77XX_GREEN);
       tft.setTextSize(7);
       tft.print(t);
       tft.setTextColor(ST77XX_GREEN);
       tft.setTextSize(7);
       tft.print(" ");
       delay(1500);
 
          
        }
