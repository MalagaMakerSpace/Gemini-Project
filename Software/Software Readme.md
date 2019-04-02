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

TODO: Cálculos provisionales, suponiendo un diámetro de 6mm para la polea


Steps_per_mm = (32 \* 200) / (3.14 \* 6) ~= 340 steps/mm


Se establece con la línea:
> #define DEFAULT_AXIS_STEPS_PER_UNIT   { 200, 200, 800, 340 , 340}
Para poder definir de forma separada los pasos para cada extrusor, se descomenta la línea:
> #define DISTINCT_E_FACTORS


#### Finales de carrera

Los finales de carrera aún no están conectados. El conexionado se puede hacer de forma que:
* Normalmente esté abierto (Interruptor normal)(NO).
* Normalmente cerrado (NC).

Para mayor seguridad, lo ideal sería conectarlos en NC para que en caso de fallo de conexión, la máquina no sea capaz de girar, en lugar de no ser capaz de detectar el pulso de final de carrera.

Marlin por defecto utiliza la configuración NC, por lo que en caso de utilizar esta conexión no habría que invertir la lógica de los finales de carrera. Para ello, se debe asegurar que en el script no estén invertidos e “INVERTING” está en false.

#define X_MIN_ENDSTOP_INVERTING false

#define Y_MIN_ENDSTOP_INVERTING false

#define Z_MIN_ENDSTOP_INVERTING false

#define X_MAX_ENDSTOP_INVERTING false

#define Y_MAX_ENDSTOP_INVERTING false

#define Z_MAX_ENDSTOP_INVERTING false

#define Z_MIN_PROBE_ENDSTOP_INVERTING false


#### SENTIDO DE GIRO DE LOS MOTORES

Se debe considerar que el motor de la cama debe girar en sentido contrario al lógico, pues para avanzar, este motor debe moverse hacia atrás.

Como aún no están conectados, se pueden tomar 2 soluciones: 
* Conectar todos los motores en el mismo sentido y mediante código cambiar el sentido de giro (solución tomada en Marlin).
* Rotar el conector del motor deseado.

Para evitar posibles problemas en el conexionado, lo lógico sería conectar todos los motores de igual modo y mediante el código cambiar el sentido de giro.

Para ello, solo habrá que declarar como “true”: “#define INVERT_Y_DIR true“ (donde Y será el eje en cuestión).


#### AUTOLEVEL

Para implementar el autonivelado, se han descomentado las líneas:

"#define AUTO_BED_LEVELING_BILINEAR", "#define LCD_BED_LEVELING" y "#define PROBE_MANUALLY" 

Para un autonivelado bilinear y para poder controlar la altura de la cama al extrusor durante la calibración.

## To do

* Comprobar eje Z

~~* Comprobar "Invertir la lógica del final de carrera" e "Invertir sentido de giro del motor"~~
* Comprobar pasos por mm de cada eje (Comprobar que se mueve según lo calculado)
* Comprobar especialmente pasos por mm del extrusor.
~~* Configuración del auto nivelado de la cama~~
* Comprobar "X2_MAX_POS", para definir con exactitud esta medida.
~~* Decidir que modo de uso del eje X utilizar (FULL_CONTROL, AUTO_PARK o DUPLICATION).~~

## Pruebas

* 15/03/2019: Software compila correctamente, carga y permite mover eje Z.
