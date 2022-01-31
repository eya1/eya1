#include <Servo.h> 
#include <SoftwareSerial.h>
SoftwareSerial mySerial(10, 11); // RX, TX
Servo myservo;   
int pos = 0;
char value;
#include <Adafruit_Sensor.h>
#include <LiquidCrystal.h>
#include <SPI.h>
#define ldr_pin A5
int light = 0;
int VO = 4;
int RS = 5;
int E = 6;
int D4 = 7;
int D5 = 8;
int D6 = 9;
int D7 = 12;
int hum;
int humidity_sensor_pin = A0;
LiquidCrystal lcd (RS, E, D4, D5 ,D6, D7);
int water_pump_pin = 3;
int water_pump_speed = 255;

void setup() {

  lcd.begin(16,2); 
  analogWrite(VO, 50);
  Serial.begin(9600);

   myservo.attach(13); 
}

void loop() {

  if (mySerial.available())  
  Serial.write(mySerial.read());
  
  if (Serial.available())  
  mySerial.write(Serial.read());
  
 int light = map(analogRead(ldr_pin), 1023, 0, 50, 0);
 int hum = map(analogRead(humidity_sensor_pin), 0, 1023, 100, 0);
 
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Regando");  
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Light:");
    lcd.print(light);
    lcd.print("%");
    lcd.setCursor(0,1);
    lcd.print("Groud_humidity");
    lcd.print(hum);
    lcd.print("%");
   
      
if (Serial.available() > 0){
  
 if( hum <= 20 && value == 'p'  ) {
  
 digitalWrite(water_pump_pin, HIGH)
 Serial.println("Irrigate");
 analogWrite(water_pump_pin, water_pump_speed);
 delay (3000);
 
 }
 
 else{
 digitalWrite(water_pump_pin, LOW);
 Serial.println("Do not irrigate");
 
}
 }
delay (300);
if (Serial.available() > 0){

     value= Serial.read();  
     Serial.println(value);
     if (value == 'c')
      for (pos = 0; pos <= 180; pos += 1) { 
    // in steps of 1 degree
       myservo.write(pos);              
       delay(15);                       
  }
  for (pos = 180; pos >= 0; pos -= 1) {
      myservo.write(pos);              
  }
}
}
