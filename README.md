# Smart-Gas-Leakage-Detection-System
Detect Fire Harmful Gases And Smoke Using MQ 2 Sensor And Glow The Led, Off The Regulator, On The Exhaust Fan, And Make Sound Of Buzzer
   Author-dnyandip jamdhade





    #include <Servo.h>

    #define GAS_SENSOR_PIN   A0
    #define BUZZER_PIN       8
    #define FAN_PIN          10
    #define SERVO_PIN        9

    Servo regulatorServo;

    int sensor_value;
    const int GAS_THRESHOLD = 400;  // adjust this after calibration

    void setup() {
     Serial.begin(9600);
     pinMode(BUZZER_PIN, OUTPUT);
     pinMode(FAN_PIN, OUTPUT);

     regulatorServo.attach(SERVO_PIN);
     // Put regulator in “ON” position initially:
     regulatorServo.write(5);   // adjust angle so regulator is ON

     // Warm‑up for sensor:
     Serial.println("Warming up MQ2 sensor...");
    delay(20000); // 20 seconds warm‑up (or more if needed)
    Serial.println("Ready.");
    }

    void loop() {
    sensor_value = analogRead(GAS_SENSOR_PIN);
    Serial.print("Sensor Value = ");
    Serial.println(sensor_value);

    if (sensor_value > GAS_THRESHOLD) {
    // Alarm state
    digitalWrite(BUZZER_PIN, HIGH);
    digitalWrite(FAN_PIN, HIGH);

    // Turn regulator OFF (servo moves to a different angle)
    regulatorServo.write(130);  // adjust angle for OFF
    }
    else {
    // Normal state
    digitalWrite(BUZZER_PIN, LOW);
    digitalWrite(FAN_PIN, LOW);

    // Regulator ON
    regulatorServo.write(5);   // adjust angle for ON
    }

    delay(1000);  // maybe slower loop; adjust as needed
    }
  
