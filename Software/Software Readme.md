# Software Section

## Objectives

* ...

## Progress


En el script “Configuration.h”:
- En el inicio se configura el baudrate, inicialmente a 250000, pero se cambia a 115200 para mayor estabilidad (según dice https://www.my-home-fab.de/en/documentations/reprap-firmware/installing-marlin-firmware-on-rumba-board)
- Se define la placa base como RUMBA tomando “MOTHERBOARD 80”.
    - Se descomenta la línea referente a “Full Graphic Display Controller (DOT Matrix 128x64)”  // ==> REMEMBER TO INSTALL U8glib to your ARDUINO library folder: http://code.google.com/p/u8glib/wiki/u8glib #define REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLERd
- Se seleccionan 2 extrusores y se definen los tipos de sensores térmicos termopares. Según se describe en la documentación, los termistores usados en los extrusores son “de 100 KOhm en resistencia sensible con NTC de 25ºC”. Tal y se indica en el propio script que se está modificando, el valor que se toma para este tipo es “1”. Por ello se marca un 1 en “TEMP_SENSOR_0” y  
- 




————————————————————————————————

PENDIENTE:


	- Comprobar la Z.
	- Comprobar “Invertir la lógica del final de carrera” y “Invertir sentido de giro del motor”



Cálculo de los pasos por milímetro:

- El driver utilizado es DRV8825 -> 32 micropasos
- Motor = 200 pasos a 1.8º/paso
- Correa GT2
- Polea 16 piños

Como nuestro controlador da 200 pasos por vuelta, y el driver DRV8825 tiene 32 micropasos
200*32=6400 micropasos para dar una vuelta completa

Si nuestra polea tiene 16 dientes y cada diente avanza 2mm: 16 dientes*2mm=32mm por vuelta

Nuestro que 6400 pasos (que es 1 vuelta) avanza 32 mm:
Pasos por milímetro: 6400 pasos/32mm=200 pasos/mm


HUSILLO

El la varilla del husillo tiene una métrica 8 (M8) el cual tiene un avance de 8 mm/vuelta, por lo que:

Pasos/mm=6400/8=800 pasos/mm

Para la polea del extrusor, hemos supuesto que mide 6mm de diámetro, el perímetro sería 2*pi*(d/2)=pi*d=pi*6=18.85mm
Por lo que los pasos por milímetro serán 6400/18.85=340.