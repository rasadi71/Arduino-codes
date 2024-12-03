#include <Wire.h>

#include <LiquidCrystal_I2C.h>

#include <IRremote.h>




const int rcvPin = 7;
IRrecv irrecv(rcvPin);
LiquidCrystal_I2C lcd(0x26, 16, 2); // Ensure this address is correct

void setup() {
  Serial.begin(9600);
  irrecv.enableIRIn(); // Start the receiver
  
  pinMode(3, OUTPUT);
  pinMode(4, OUTPUT);
  pinMode(5, OUTPUT);
  
  lcd.init(); 
  lcd.backlight();
       lcd.setCursor(0, 0);
  lcd.print("RED Button>on");
  
  lcd.setCursor(0, 1);
  lcd.print("FUNCTIONstop>OFF");
}

void loop() {
 

  if (IrReceiver.decode()) {
    auto value = IrReceiver.decodedIRData.decodedRawData;

    
    switch (value) {
      case 4278238976:
        Serial.println(value);  // Button 1
        digitalWrite(3, HIGH);
        digitalWrite(4, HIGH);
        digitalWrite(5, LOW);
        break;
      
      case 4244815616: // Template
        Serial.println(value);  // Button 
        digitalWrite(3, LOW);
        digitalWrite(4, LOW);
        digitalWrite(5, HIGH);
        break;
      
      default:
        Serial.println(value);     
    }

    IrReceiver.resume(); // Receive the next value  
  }
}
