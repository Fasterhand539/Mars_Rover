Long Range Autonomous Rover 

Autonomous + Remote controlled rover equipped with GPS navigation  



Abstract 

The goal was to build a rover that can carry out Reconnaissance mission and collect various data from a distant location without relying on heavy network infrastructure. In order to navigate at long range, implemented LoRa module that can communicate up to 10km without the need of heavy hardware. To address autonomous navigation, combined GPS module with GYRO sensor to precisely move with the help of Latitude and Longitude. Lastly to move through rough terrain with a light body sought inspiration from a rocker-bogie system and made personalised pseudo Rocker-Bogie system with 6 wheels. 

 

Equipment list 

1. L298N H-Bridge Dual Motor Driver, Stepper Motor Driver 

2. N20 12V 200 RPM Micro Metal DC Gear Motor 

3. LM2596 DC to DC Step Down Buck Converter with Digital Tube Display 

4. SX1278 LoRa Module 433M 10KM Ra-02 Original Ai 

5. Raspberry Pi Pico With Soldered Headers  

6. GY-NEO-6M V2 Flight Control GPS Module New  

7. Lipo Battery 2500mAh 7.4V 

8. Ultrasonic Sonar Sensor HC-SR04 

9. DHT11



Use-cases 

Unknown area reconnaissance 

Small-payload delivery 

Perimeter patrol (remote area/Radioactive zone) 

Environmental observation 

Campus/industrial internal courier 

Night/low-visibility navigation aid 

 

Algorithm and Working Principle 
 
Sensors: GPS (NEO‑6M) for position, Compass (QMC5883L) for direction 

Stop rule: Stop when rover is within 2 meters of the target coordinate. 

1) Setup 

  1.Power on. 

  2.Calibrate the compass (rotate rover slowly in different directions) so headings are accurate. 

  3.Save the target GPS coordinate (lat_target, lon_target). 

2) Repeat this loop until we arrive 
Loop: 
  1.Read sensors 

    Get current GPS position: (lat_now, lon_now) 

    Get compass heading: heading (0–360°, where 0° = North) 

  2.Check distance to target 

    Compute distance = GPS_distance((lat_now, lon_now), (lat_target, lon_target)) 

    If distance ≤ 2 m → STOP motors → Arrived 

  3.Find Bearning (desired direction)  

    Compute bearing = GPS_bearing((lat_now, lon_now) → (lat_target, lon_target)) 

    Bearing is the angle direction from my position to the destination. 

  4.Comparing desired direction vs current direction  

    error = normalize(bearing − heading) so it stays between −180° and +180° 

    Interpretation: 

    error > 0 means target is to the right 

    error < 0 means target is to the left 

  5.Differential-drive movement decision 

    If |error| < 8° → Drive straight 

    Left motors forward, right motors forward (same speed) 

    Else if error > 0 → Turn right (pivot) 

    Left motors forward, right motors backward  

    Else (error < 0) → Turn left (pivot) 

    Right motors forward, left motors backward 

  6.Move in short steps, then re-check  

    Run the chosen motion for a short moment (example: 0.5–1.0 s) 

    Then loop back to step 1 (new GPS + compass readings) 

 

 

 
