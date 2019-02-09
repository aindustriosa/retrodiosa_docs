# Control de la televisión desde el portátil.
## Problema
Al arrancar la recreativa tenemos que encender el portátil (con salida VGA) y la pantalla (una televisión Polaroid Definia). Durante el uso de la recreativa necesitaremos cambiar el volumen del sonido.
Al estar la televisión dentro del mueble de la recreativa no tenemos acceso ni a la botonera ni a la interfaz de infrarrojos.

## Posibles soluciones
### Configurar la pantalla a través del cable HDMI (no vale)
Las televisiones actuales incorporan en la conexión HDMI el protocolo CEC, que permite inercambiar comandos entre los equipos conectados.
En el mundo Linux existe libcec y cec-utils, que nos facilitan el enviar comandos como encender o apagar la pantalla, cambiar la fuente (a VGA) y subir o bajar el volumen. Desgraciadamente, esta televisión no tiene tal funcionalidad. Esto lo hemos visto en el manual y hemos comprobado conectando un portátil con HDMI a la TV y usando "cec-client -l", a lo que nos ha respondido:

```
libCEC version: 4.0.2, compiled on Linux-4.4.0-101-generic ... , features: P8_USB, DRM, P8_detect, randr
Found devices: NONE
```

### Configurar la pantalla puenteando la botonera con un arduino
La idea consiste en usar relays de estado sólido para puentear los pulsadores de la botonera de la televisión y controlarlo desde un arduino.

### Configurar la pantalla puenteando el interfaz IR con un arduino
La idea es parecida a la anterior, pero usando la interfaz de infrarrojos emulando el mando a distancia (el cual disponemos).
Esta idea es mejor que la anterior, porque necesitaremos menos hardware extra y tendríamos acceso al menú de la televisión si lo necesitáramos.

#### Desarrollo
Básicamente, hemos seguido estos pasos:

En primer lugar, no sabemos cuales son los comandos del mando a distancia, por los que tenemos que averiguarlos:
1. Pinchar un GPIO del Arduino a la salida del sensor de IR de la televisión.
2. Grabar los comandos del mando a distancia usando uno de los ejemplos de la librería IRremote de Arduino.

Para reproducir los comandos, conectamos el Arduino en el mismo lugar que en el paso 1 pero, esta vez, con un divisor de tensión, ya que la salida digital del Arduino es de 5 V y la placa es 3,3 V
3. Cargar el sketch https://github.com/aindustriosa/retrodiosa_sw/blob/master/tv_control/arduino_sketch/retrodiosa_IR_bypass.ino 
4. Conectar el Arduino al portátil.
5. Se puede usar el script de https://github.com/aindustriosa/retrodiosa_sw/blob/master/tv_control/pc_command_line/tv_control.py o 'echos' a /dev/ttyUSB0 para enviar los comandos a la televisión.

El sketch de Arduino intenta encender la TV cuando se alimenta, así la pantalla se enciende antes de que el portátil se lo ordene al arrancar la recreativa.
