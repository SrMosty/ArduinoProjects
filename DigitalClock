#include <SPI.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <BluetoothSerial.h>

#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels
#define OLED_RESET     -1 // Reset pin # (or -1 if sharing Arduino reset pin)
#define SCREEN_ADDRESS 0x3C ///< See datasheet for Address; 0x3D for 128x64, 0x3C for 128x32
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

BluetoothSerial SerialBT;
int temp = 0;
int hours = -1;
int minutes = -1;
int month = -1;
int day = -1;
unsigned long initialMillis = 0;
unsigned long initialSeconds = 0;

void setup() {
  Serial.begin (115200);
  SerialBT.begin("ESP32_Reloj");

  Serial.setTimeout(10);
  Wire.begin();
  display.cp437(true);

  if (!display.begin(SSD1306_SWITCHCAPVCC, SCREEN_ADDRESS))
  {
    Serial.println(F("SSD1306 no pudo inicializar."));
    while (true);
  }
}

void loop() 
{
  int config = 0;
  if (SerialBT.available())
  {
    display.clearDisplay();
    while (config < 3)
    {
      while (hours == -1)
      {
        SerialBT.println("Indique la hora actual:");
        delay(10000);        
        hours = SerialBT.readString().toInt();
        if (hours < 0 || hours > 24)
        {
          SerialBT.println("Error al indicar la hora");
          hours = 0;
        }
        else
          config++;
      }

      while (minutes == -1)
      {
        SerialBT.println("Indique los minutos actuales:");
        delay(10000);
        minutes = SerialBT.readString().toInt();
        if (minutes < 0 || minutes > 60)
        {
          SerialBT.println("Error al indicar los minutos");
          minutes = 0;
        }
        else
          config++;
      }
      int seconds = 0;
      config++;
      initialSeconds = hours * 3600 + minutes * 60 + seconds;
      initialMillis = millis();
    }

    while (month == -1)
      {
        SerialBT.println("Indique el mes:");
        delay(10000);        
        month = SerialBT.readString().toInt();
        if (month <= 0 || month > 12)
        {
          SerialBT.println("Error al indicar el mes");
          month = -1;
        }
        else
          config++;
      }

    while (day == -1)
      {
        SerialBT.println("Indique el dia:");
        delay(10000);        
        day = SerialBT.readString().toInt();
        if (day <= 0 || day > 31)
        {
          SerialBT.println("Error al indicar el dia");
          day = -1;
        }
        else
          config++;
      }
  }
  else
  {
    display.clearDisplay();
    display.println("Waiting...");
  }

  display.clearDisplay();

  unsigned long currentMillis = millis();
  unsigned long currentSeconds = initialSeconds + ((currentMillis - initialMillis) / 1000);
  unsigned long hours = currentSeconds / 3600;
  unsigned long minutes = (currentSeconds % 3600) / 60;
  unsigned long seconds = (currentSeconds % 3600) % 60;

  String hour = (hours < 10 ? "0" : "") + String(hours);
  String min = (minutes < 10 ? "0" : "") + String(minutes);
  String sec = (seconds < 10 ? "0" : "") + String(seconds);

  //Hora
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(6, 2);
  display.setTextSize(2, 4);
  display.println(hour);
  display.fillRect(30, 10, 4, 5, WHITE);
  display.fillRect(30, 21, 4, 5, WHITE);
  display.setCursor(36, 2);
  display.setTextSize(2, 4);
  display.println(min);
  display.setCursor(46, 34);
  display.setTextSize(1, 2);
  display.println(sec);
  display.display();

  //Dia
  display.setCursor(32, 50);
  display.setTextSize(1, 2);
  if (day > 0 && day <= 31)
  {
    if (month == 1 && day == 31 && hour == "23" && min == "59" && sec == "59")
    {
      month++;
      day = 1;
    }
    else if (month == 2 && day == 28 && hour == "23" && min == "59" && sec == "59")
    {
      month++;
      day = 1;
    }
    else if (month == 3 && day == 31 && hour == "23" && min == "59" && sec == "59")
    {
      month++;
      day = 1;
    }
    else if (month == 4 && day == 30 && hour == "23" && min == "59" && sec == "59")
    {
      month++;
      day = 1;
    }
    else if (month == 5 && day == 31 && hour == "23" && min == "59" && sec == "59")
    {
      month++;
      day = 1;
    }
    else if (month == 6 && day == 30 && hour == "23" && min == "59" && sec == "59")
    {
      month++;
      day = 1;
    }
    else if (month == 7 && day == 31 && hour == "23" && min == "59" && sec == "59")
    {
      month++;
      day = 1;
    }
    else if (month == 8 && day == 31 && hour == "23" && min == "59" && sec == "59")
    {
      month++;
      day = 1;
    }
    else if (month == 9 && day == 30 && hour == "23" && min == "59" && sec == "59")
    {
      month++;
      day = 1;
    }
    else if (month == 10 && day == 31 && hour == "23" && min == "59" && sec == "59")
    {
      month++;
      day = 1;
    }
    else if (month == 11 && day == 30 && hour == "23" && min == "59" && sec == "59")
    {
      month++;
      day = 1;
    }
    else if (month == 12 && day == 31 && hour == "23" && min == "59" && sec == "59")
    {
      month = 1;
      day = 1;
    }
    else if (hour == "23" && min == "59" && sec == "59")
      day++;
  }
  display.println(day);
  //Mes
  display.setCursor(25, 34);
  display.setTextSize(1, 2);
  if (month > 0 && month <= 12)
  {
    if (month == 1)
      display.println("ENE");
    if (month == 2)
      display.println("FEB");
    if (month == 3)
      display.println("MAR");
    if (month == 4)
      display.println("ABR");
    if (month == 5)
      display.println("MAY");
    if (month == 6)
      display.println("JUN");
    if (month == 7)
      display.println("JUL");
    if (month == 8)
      display.println("AGO");
    if (month == 9)
      display.println("SEP");
    if (month == 10)
      display.println("OCT");
    if (month == 11)
      display.println("NOV");
    if (month == 12)
      display.println("DIC");
  }
  display.display();
  delay(1000);
}
