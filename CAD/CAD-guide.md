<div align="center">

# MOCHI - Guía Mecánica

</div>

## Descripción

Este modelo de MOCHI se fundamenta en el diseño mecánico y la estructura CAD, optimizada para cumplir rigurosamente con los límites de 10x10 cm y 500 g. Aquí se justifica la selección de materiales, la distribución de componentes y los procesos de fabricación empleados, cuidando los siguientes aspectos:

- El frente del robot está diseñado para que la cuchilla y el fleje proporcionen un deslice  más fácil del robot oponente sobre el robot.
- La altura de los sensores IR debe ser la adecuada para que no se detecte el dohyo al iniciar el match.
- Se concentra el peso en la parte forntal del robot para mayor inercia y mejor posicionamiento de la navaja sobre el dohyo (lo más asentanda posible).
- La electrónica (cables de los IR, QTR y motores) se matiene oculta completamente dentro del robot, mostrando únicamente la PCB, para evitar accidentes.
- La distancia entre los sensores de línea y la superficie del dohyo consta de 5 mm para una mejor detección.
- EL robot alcanza los 450 g.

El robot se subdivide de la siguiente manera:

- **Chasis:** Consiste en una sola pieza de impresión 3D, dejando el espacio correspondiente a la placa de latón debajo del mismo, al igual que el espacio correspondiente a cada componente de la electrónica.
- **Electrónica:** PCB, sensores infrarrojos (IR), sensores de línea (QTR), motores y pila de lítio (Lipo).
- **Piezas maquinadas:** Placa de latón de 1/4" (maquinada en fresadora) y fleje de acero al carbono (corte láser).
- **Tornillería:** Tornillería M3 para la fijación de componentes electrónicos y piezas maquinadas.

---

## Chasis y Piezas Maquinadas


<img width="435" height="355" alt="image" src="https://github.com/user-attachments/assets/59ac6c6a-f4bd-40d9-9dff-c355fa9853ea" />

<img width="379" height="355" alt="image" src="https://github.com/user-attachments/assets/6d1f7146-c55c-453d-8903-b12dac7ca92e" />

<img width="410" height="260" alt="image" src="https://github.com/user-attachments/assets/15cf89d3-6a2d-4235-a190-d3a7e1f1571c" />

<img width="405" height="260" alt="image" src="https://github.com/user-attachments/assets/369c0913-5af7-4493-bba4-2af40ca463d2" />

> [!NOTE]
> El chaflán en la parte posterior de la placa de latón tiene como objetivo reducir la altura de la placa para que pueda colocarse lo más adelante posible debajo del chasis impreso y concentrar el peso ahí.

> [!NOTE]
> Los espacios para los sensores infrarrojos están diseñados con un ángulo específico de abertura para tener un mayor alcance de visión hacia los lados.

> [!NOTE]
> El soporte de la PCB es parte del chasis, simplemente para colocarla sobre el. **Área de mejora: adaptar el diseño para hacer una tapa para la PCB que se pueda atornillar.**

<img width="373" height="241" alt="image" src="https://github.com/user-attachments/assets/866f3937-f7df-4271-8c88-1e63725b4aec" />

> [!NOTE]
> La navaja se encuentra adherida a la impresión 3D, en la cual el borde es redondeado, esto con la finalidad de que la impresión se dañe lo menor posible, así como un mejor posicionamiento de la(s) navaja(s).

---

| Componente | Material | Manufactura | Cantidad | Peso | Especificaciones |
| :--- | :--- | :--- | :---: | :---: | :--- |
| Chasis Principal | PLA | Impresión 3D | 1 | 34.14 g | PLA 3.75 mm · Tolerancias de 0.3 mm · Tornillería sin tolerancias |
| Placa para peso | Latón | Fresadora | 1 | 225.66 g | Calibre 1/4" |
| Fleje | Acero al carbono | Corte Láser | 1 | 23.43 g | Calibre 18 |

---

## Electrónica

| Componente | Cantidad | Peso | Especificaciones |
| :--- | :---: | :---: | :--- |
| JS04F Digital distance sensor | 4 | 4 g | Detecta objetos desde 40 cm · 3.3 V a 5 V (15 mA) · Cable de 15 cm · 12.5 x 17.7 x 11.5 mm |
| Core Dc Motor JS16661 | 2 | 21 g | 6 V (120 mA) · 750 RPM  ·  (hasta 15 V)  · Corriente stall 3.5 A |
| QTR-1RC Infrared Sensor | 2 | NA |  5 V, 20-25 mA · 7.62 x 12.7 x 2.54 mm |
| PCB XMotion V3 | 1 | 13 g | Controlador · 80 x 30 x 7 mm · [`electronics-info.md`](/electronics/electronics-info.md)

