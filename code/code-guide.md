<div align="center">
  
# MOCHI - Guía de código

</div>

## Descripción

Código fuente para el control de un robot minisumo autónomo desarrollado en el entorno de Arduino utilizando la PCB Xmotion V3 de JSUMO con microcontrolador Arduino Leonardo. El programa gestiona la lectura de sensores infrarrojos para la localización del oponente, sensores de línea QTR para evitar salir del dohyo, un módulo de inicio para el control de arranque de seguridad y la calibración de motores de corriente directa con el objetivo de crear una rutina autónoma competitiva.

---

## Especificaciones Hardware 

* **Placa de Control:** Xmotion V3 PCB (JSUMO)
* **Motores:** 2x Motores DC de 750 RPM
* **Alimentación:** Batería LiPo 2S (800 mAh)
* **Sensores de Oponente:** 4x Sensores Infrarrojos (IR) digitales (`outer_left_IR`, `front_left_IR`, `front_right_IR`, `outer_right_IR`)
* **Sensores de Línea:** 2x Sensores QTR analógicos (`left_QTR`, `right_QTR`)
* **Módulo de Inicio:** 1x Starter IR/RF (`starter`)

---

## Estrategia Implementada

De las estrategias analizadas, se configuró y activó la **Estrategia #2**:
* **Comportamiento:** Mantiene una velocidad media constante en la trayectoria de exploración hasta que algún sensor de infrarrojos detecte al oponente.
* **Ventaja:** Al detectar presencia enemiga, incrementa la potencia enviada a los motores para aprovechar la inercia del vehículo y la tracción de las ruedas durante la embestida.

---

## 🔌 Asignación de Pines (Pinout)

| Componente | Pin Arduino / Xmotion | Tipo de Señal |
| :--- | :--- | :--- |
| **Sensor IR Izquierdo Externo** | `0` | Digital Input |
| **Sensor IR Frontal Izquierdo** | `1` | Digital Input |
| **Sensor IR Frontal Derecho** | `2` | Digital Input |
| **Sensor IR Derecho Externo** | `4` | Digital Input |
| **Módulo Starter** | `A0` | Digital Input |
| **Sensor QTR Izquierdo** | `A5` | Analog Input |
| **Sensor QTR Derecho** | `A4` | Analog Input |

---

## ⚙️ Arquitectura del Software

```
// Robot specifications:
// 4 IR sensors
// 2 QTR sensors
// 2 750 rpm DC motors
// 1 starter
// 2s 800 mah lipo battery
// Xmotion V3 PCB from JSUMO

#include <xmotionV3.h> // xmotion library

int calib_left = 170;
int calib_right = 255;

int outer_left_IR = 0;
int front_left_IR = 1;
int front_right_IR = 2;
int outer_right_IR = 4;
int starter = A0;
int left_QTR = A5;
int right_QTR = A4;

int no_opp_cycles = 0;        // count of cycles in which no opp was detected
const int search_count = 400; // time until robot starts search mode
const int search_turn  = 100;

// Motor calibration
void motor_calibration (int speed) {
  // Velocity scale depending the required %
  int left = map(speed, 0, 100, 0, calib_left);
  int right = map(speed, 0, 100, 0, calib_right);
  xmotion.MotorControl(left, right);
}

void setup() {
  Serial.begin(9600); // Establishes speed of transmission 
  xmotion.StopMotors(100); // Stopping motors for resetting
  
  // Declare inputs and outputs
  pinMode(outer_left_IR, INPUT);
  pinMode(front_left_IR, INPUT);
  pinMode(front_right_IR, INPUT);
  pinMode(outer_right_IR, INPUT);
  pinMode(left_QTR, INPUT);
  pinMode(right_QTR, INPUT);
  pinMode(starter, INPUT);
}

void loop() {
  while (digitalRead(starter) == 0) { 
    xmotion.UserLed2(200); 
  }

  while(1) {
    // Line sensors --- Stay inside the dohyo    
    if (analogRead(left_QTR) < 70 && analogRead(right_QTR) > 326) {  
      motor_calibration(-35); 
      delay(500); 
      xmotion.Left0(8, 15);
    } 
    else if (analogRead(left_QTR) > 350 && analogRead(right_QTR) < 60) { 
      motor_calibration(-35); 
      delay(500); 
      xmotion.Right0(9, 15);
    }
    else if (analogRead(left_QTR) < 70 && analogRead(right_QTR) < 60) { 
      motor_calibration(-40); 
      delay(500);
      xmotion.Left0(9, 15);
    }
    else {
      // Opponent sensors --- Attack
      bool front_left = digitalRead(front_left_IR) == 1;
      bool front_right = digitalRead(front_right_IR) == 1;
      bool outer_left = digitalRead(outer_left_IR) == 1;
      bool outer_right = digitalRead(outer_right_IR) == 1;

      if (front_left && front_right) {
        xmotion.ArcTurn(18,69,2);
      }
      else if (front_left && !front_right) {
        xmotion.ArcTurn(10,4,2);
      } 
      else if (front_right && !front_left) {
        xmotion.ArcTurn(30,30,2);
      }
      else if (front_left && outer_left) {
        xmotion.ArcTurn(9,42,2);
      }
      else if (front_right && outer_right) {
        xmotion.ArcTurn(16,24,2);
      }
      else if (outer_left && !outer_right) {
        xmotion.Left0(60,5);
      }
      else if (outer_right && !outer_left) {
        xmotion.Right0(45,5);
      }
      // No opponent detected --- Search mode
      else if (!front_left && !front_right && !outer_left && !outer_right) {
        no_opp_cycles++;

        if (no_opp_cycles < search_count) {
          xmotion.StopMotors(20);
        }
        else if (no_opp_cycles < search_count + search_turn) {
          xmotion.Right0(20,5);
        }
        else {
          no_opp_cycles = 0;
        }
      }
    }
  }
}
```
