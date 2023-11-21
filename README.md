#include <Servo.h>

const int si = 70;
const int sm = 70;
const int sd = 70;
const int SensorI = A6;
const int SensorD = A1;
const int SensorM = A5;
const int IRP = 9;
const int IR = 2;
const int IRA = 8;
const int Echo = 10;
const int Trigger = 11;
int Contador = 0;
int Modulos = 1;
int Lectura = 2;
int Rectas = 0;
int Porticos = 0;
int Tiempo = 0;
int Aleatorio = random (500, 1000);

Servo MotorI;
Servo MotorD;
Servo MotorAI;
Servo MotorAD;
Servo Brazo;

//Funciones
void Adelante(int Velocidad) {
MotorI.write(90 + Velocidad );
MotorD.write(92 - Velocidad);
MotorAI.write(90 + Velocidad );
MotorAD.write(92  - Velocidad);
}
void Atras(int Velocidad) {
MotorI.write(90 - Velocidad );
MotorD.write(90 + Velocidad);
MotorAI.write(90 - Velocidad );
MotorAD.write(90 + Velocidad);
}
void Izquierda(int Velocidad) {
MotorI.write(90 - Velocidad);
MotorD.write(90 - Velocidad);
MotorAI.write(90 - Velocidad);
MotorAD.write(90 - Velocidad);
}
void Derecha(int Velocidad) {
MotorI.write(90 + Velocidad);
MotorD.write(90 + Velocidad);
MotorAI.write(90 + Velocidad);
MotorAD.write(90 + Velocidad);
}
void Frenar() {
MotorI.write(90);
MotorD.write(90);
MotorAI.write(90);
MotorAD.write(90);
}

void Seguir() {
if ((analogRead(SensorI) > si) || (analogRead(SensorD) > sd)) {
if ((analogRead(SensorD) > sd)) {
MotorI.write(110);
MotorD.write(110);
MotorAI.write(145);
MotorAD.write(145);
}
if ((analogRead(SensorI) > si)) {
MotorI.write(70);
MotorD.write(70);
MotorAI.write(35);
MotorAD.write(35);
}
}
else {
Adelante(15);
}
}

void Modulo2() {
if ((digitalRead(IR) == LOW) && (Lectura == 2)) {
Frenar();
delay(100);
while (!((digitalRead(IRA) == LOW))) {
Derecha(20);
}
Izquierda(15);
delay(150);
Frenar();
delay(100);
while(!((digitalRead(IRP) == LOW))) {
Adelante(10);
}
delay(800);
Frenar();
delay(200);
Brazo.write(140);
delay(100);
Izquierda(20);
delay(400);
while (!(analogRead(SensorM) > sm)) {
Atras(10);
}
if (analogRead(SensorM) > sm) {
Adelante(15);
delay(800);
while (!((analogRead(SensorI) < si) && (analogRead(SensorM) > sm) && (analogRead(SensorD) < sd))) {
Izquierda(20);
}
Frenar();
delay(100);
}
}
if ((!(digitalRead(IR) == LOW)) || (digitalRead(IRP) == LOW)) {
if ((analogRead(SensorI) > si) || (analogRead(SensorD) > sd)) {
if (analogRead(SensorD) > sd) {
Derecha(15);
}
if (analogRead(SensorI) > si) {
Izquierda(15);
}
}
else {
Adelante(15);
}
}
}

void setup() {
Serial.begin(9600);
Frenar();
Contador = 0;
Modulos = 1;
Lectura = 2;
Porticos = 0;
Rectas = 0;
//Servos
MotorI.attach(4);
MotorD.attach(5);
MotorAI.attach(6);
MotorAD.attach(7);
Frenar();
Brazo.attach(3);
Brazo.write(180);
//IR's
pinMode(IRA, INPUT); 
pinMode(IR, INPUT);
//pinMode(IRP, INPUT);
//Ultrasonido
pinMode(Trigger, OUTPUT);
pinMode(Echo, INPUT);
digitalWrite(Trigger, LOW);
delay(1000);
}

void loop() {
Serial.println(analogRead(SensorD));
}
