# Software Section

## Objectives

* Que funcione

## Progress

### Basic Configuration
En el script “Configuration.h”:
* Se define la placa base como RUMBA tomando “MOTHERBOARD 80”.
* Se descomenta la línea "#define REPRAP_DISCOUNT_SMART_CONTROLLER" para activar el LCD.
* Se seleccionan los tipos de sensores térmicos termopares.
	* Según se describe en la documentación, los termistores usados en los extrusores son “de 100 KOhm en resistencia sensible con NTC de 25ºC”. Tal y se indica en el propio script que se está modificando, el valor que se toma para este tipo es “1”. Por ello se marca un 1 en “TEMP_SENSOR_0” y  en "TEMP_SENSOR_1".

### Dual extruder and X axis

En el fichero "Configuration.h":
* Se define el número de extrusores como 2.
* Se descomenta la línea "#define USE_XMAX_PLUG" para usar el final de carrera final del eje X.

En el fichero "Configuration_adv.h":
* Se descomenta la línea "#define DUAL_X_CARRIAGE"
* Para establecer "X2_MIN_POS", se mide la distancia que ocupa el primer extrusor tocando el final de carrera. Como valor final se establece 80 algo superior al real, para asegurarse de que no se tocan.
* Para establecer "X2_MAX_POS", se mide la distancia entre los dos cabezales cuando están "aparcados".
* Como modo de uso se mantiene el que viene por defecto: "DXC_FULL_CONTROL_MODE", cediendo todo el control al gcode.


### Microstep calculation

Componentes relevantes:
* Driver DRV8825 con 32 micropasos
* Motor de 200 pasos por vuelta (1.8º/paso)
* Correa GT2 para ejes X e Y
* Polea 16 dientes para ejes X e Y
* Husillo T8x8 para eje Z (8mm/vuelta)

Usaremos la fórmula:
>	Steps_per_mm = steps_per_revolution / mm_per_revolution

#### Axis X and Y

Steps_per_mm = (32 \* 200) / (16 \* 2) = 200 steps/mm

#### Axis Z

Steps_per_mm = (32 \* 200) / 8 = 800 steps/mm

#### Extruders
————————————————————————————————
TODO: Cálculos provisionales, suponiendo un diámetro de 6mm para la polea
————————————————————————————————

Steps_per_mm = (32 \* 200) / (3.14 \* 6) ~= 340 steps/mm


Se establece con la línea:
> #define DEFAULT_AXIS_STEPS_PER_UNIT   { 200, 200, 800, 340 , 340}
Para poder definir de forma separada los pasos para cada extrusor, se descomenta la línea:
> #define DISTINCT_E_FACTORS

## To do

* Comprobar eje Z
* Comprobar "Invertir la lógica del final de carrera" e "Invertir sentido de giro del motor"
* Comprobar pasos por mm de cada eje
* Comprobar especialmente pasos por mm del extrusor.
* Configuración del auto nivelado de la cama
* Comprobar "X2_MAX_POS", para definir con exactitud esta medida.
* Decidir que modo de uso del eje X utilizar (FULL_CONTROL, AUTO_PARK o DUPLICATION).

## Pruebas

* 15/03/2019: Software compila correctamente, carga y permite mover eje Z.
