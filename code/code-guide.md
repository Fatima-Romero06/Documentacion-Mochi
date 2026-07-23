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

##  Asignación de Pines (Pinout)

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

## Arquitectura del Software
