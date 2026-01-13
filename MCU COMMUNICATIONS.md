# UART COMMUNICATION 
Universal Asynchronous Transmitter(UART).
Here we have to communicate with 2 microcontrollers. 
sender code
void setup() {
Serial.begin(9600);
}
void loop() {

Serial.println("ABHIIII");
delay(2000);

}

receiver code

void setup() {
  Serial.begin(9600);
}

void loop() {
  if (Serial.available()) {
    String abhi = Serial.readStringUntil('\n');
    Serial.println(abhi);
  }
}

There are 3 connections required. They are TX to RX , RX to TX and GND to GND. In this communication, TX (Transmit) sends serial data , RX(Receiver) reads data from another device. 
OUTPUT 
ABHIIII
ABHIIII
ABHIIII
.....

# I2C COMMUNICATION 

I2C (Inter-Integrated Circuit). It is best for short-distance.
This also uses 2 wires SDA AND SCL. 
SDA --> Serial data line.This is the line where the actual "message" (like your "ABHIIII" string) travels back and forth.
SCL --> Serial clock line. This clock ensures both boards are reading the data at exactly the same time.
IN THIS NANO THE PORT IS USUALLY  A4 FOR SDA(SERIAL DATA) AND A5 FOR SCL (SERIAL CLOCK ). 

MASTER (MCU A)
 #include <Wire.h>

void setup() {
  Wire.begin();              // Join I2C bus as MASTER
}

void loop() {
  Wire.beginTransmission(8); // Slave address
  Wire.write("Hi");
  Wire.endTransmission();
  delay(2000);
}

SLAVE(MCU B)

 #include <Wire.h>

void receiveEvent(int bytes) {
  while (Wire.available()) {
    char c = Wire.read();
    Serial.print(c);
  }
  Serial.println();
}

void setup() {
  Serial.begin(115200);
  Wire.begin(8);             // Join I2C bus as SLAVE with address 8
  Wire.onReceive(receiveEvent);
}

void loop() {
}
The connections is like 
A4 to A4 (SDA)
A5 to A5 (SCL)
GND to GND
5V to VIN 
we can change the slave address to any number between 1-127(0 and above 127 is reserved)

# SPI COMMUNICATION (1 MASTER 1 SLAVE)
SPI (Serial Peripheral Interface).This communication is faster than other communications. 
connections
D11 to D11 (MOSI)
D12 to D12 (MISO)
D13 to D13  (SCK)
D10 to D10  (SS)
GND to GND
SPI SIGNAL WIRES 
MOSI -- MASTER OUT SLAVE IN 
MISO -- MASTER IN SLAVE OUT 
SCK -- SERIAL CLOCK
SS -- SLAVE SELECT (CS -- CHIP SELECT) 
ss/cs works on low ie; slave active and listen to SCK/MOSI
If it is high it really means high impedance.
digitalWrite(10, LOW) -- SELECT slave
digitalWrite(10, HIGH)-- DESELECT slave

MASTER
 #include <SPI.h>

void setup() {
  pinMode(10, OUTPUT);
  digitalWrite(10, HIGH);
  SPI.begin();
}

void loop() {
  digitalWrite(10, LOW);
  SPI.transfer('H');
  SPI.transfer('i');
  SPI.transfer('\n');   // message end
  digitalWrite(10, HIGH);
  delay(1000);
}

SLAVE

 #include <SPI.h>

void setup() {
  Serial.begin(9600);
  pinMode(MISO, OUTPUT);    // Slave must set MISO
  SPCR |= _BV(SPE);         // Enable SPI
  SPI.attachInterrupt();    // Enable SPI interrupt
}

ISR (SPI_STC_vect) {
  char c = SPDR;            // Read received byte
  Serial.print(c);          // Print each byte
}

void loop() {
}
