# Carrito Inteligente de Monitoreo para la detección de objetos

## Descripción

_Este proyecto consta de un carrito inteligente que integra un ESP32-CAM y un sensor ultrasonico para detectar objetos cercanos y enviar una foto a Telegram._

## Objetivos Generales 

Desarrollar un prototipo de carro inteligente para monitoreo y detección de objetos que permita identificar la presencia de objetos en áreas específicas, optimizando la seguridad y trazabilidad en entornos industriales como bodegas y almacenes.

### Objetivos Específicos

- Implementar una interfaz para el control del carrito a través de Wifi.
- Enviar notificaciones a través de la plataforma de Telegram cuando se detecte la presencia de objetos.

## Resultados Esperados

- Reducción de incidentes de seguridad mediante la detección de situaciones inusuales.
- Mejora en la toma de decisiones preventivas.
- Monitoreo constante mediante recolección de imágenes.


## Requisitos
- Raspberry Pi 4
- ESP32-CAM
- Sensor Ultrasónico
- Servomotor
- Kit Carrito 4WD
- Driver L298N
- Node-RED
- MQTT
- Python
- Baterias/Power Bank

## Instalación de Sensores
A continuación, se muestra como configurar el circuito para sensores en el carrito:
![Configuración del circuito 1](https://drive.google.com/uc?export=view&id=15fLQBwfkAwDtzfuxsUQsojh9bdheOIX5)


Circuito para el modulo ESP32-CAM:
![Configuración del circuito 2](https://drive.google.com/uc?export=view&id=1L-ORgN3-7PsYO9jrUKshu3ok84MmwiVG)

  
## Instalación y configuración del proyecto 
Estas instrucciones te permitiran instalar y configurar el proyecto:
1. Clona el repositorio:
   ```bash
   git clone https://github.com/LordCadeu/Carrito-Inteligente-De-Monitoreo.git

2. Navega al directorio del proyecto:
   ```bash
   cd carrito-inteligente
   
3. Instala las dependencias:
   ```bash
   npm install



## Instalación y configuración de Arduino IDE para ESP32-CAM

1. Instala las dependencias para ESP32 en Arduino IDE siguiendo [estas instrucciones](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html). _Las instrucciones han sido obtenidas del [repositorio](https://github.com/espressif/arduino-esp32) de Espressif para Arduino._
2. Instala los drivers del ESP32 para programarlo, usando los siguientes comandos:
> ### **⚠️ Importante:**
> 
> Para ejecutar estos comandos es importante tener instalado GIT.
> 
```bash
sudo usermod -a -G dialout $USER
wget https://bootstrap.pypa.io/get-pip.py
sudo apt-get install python3-distutils
sudo apt-get install python3-apt
mkdir -p ~/Arduino/hardware/espressif
cd ~/Arduino/hardware/espressif
git clone https://github.com/espressif/arduino-esp32.git esp32
cd esp32
git submodule update --init --recursive
cd tools
python3 get.py
```
3. Una vez configurado, abre Arduino IDE y selecciona la placa `ESP32` en `Herramientas > Placa > ESP32 > AI Thinker ESP32-CAM`.
4. Configura las siguientes caracteristicas:
   
![Configuración de características de la placa](https://drive.google.com/uc?export=view&id=1kHD4CXwOYup32zElOJl1UFbpypb1Lw_-)


#### Carga del Código:

1. Abre el sketch `CameraWebServerUltrasonico.ino` en el IDE de Arduino
2. Configura los parámetros de tu red WiFi, IP de la Raspberry Pi y el nombre del tópico de MQTT en el sketch.
3. Carga el código en la ESP32-CAM.
4. Guarda la IP generada, para configurar la conexión en Node-RED más adelante


## Configuración de Node-RED

### Instalación de Node-RED:
1. Instala Node-RED en tu Raspberry Pi siguiendo [estas instrucciones](https://nodered.org/docs/getting-started/raspberrypi).
2. Para mayor accesibilidad configura el inicio automático de Node-RED. Abre la terminal y ejecuta el siguiente comando:
```bash
sudo systemctl enable nodered.service
```


### Configuración del Flow en Node-RED:
1. Importa el flow `CAMUltrasonico.json` en Node-RED.
2. Asegúrate de tener instalados los siguientes nodos adicionales:
   - `node-red-dashboard`: Para la interfaz de control de monitoreo.
   - `node-red-contrib-telegrambot`: Para enviar mensajes a Telegram.
   - `node-red-contrib-ui-joystick`: Para configurar el joystick virtual.
   - `node-red-node-pi-gpio`: Para la configuración de pines GPIO de la Raspberry Pi.


## Configuración de los Nodos
El flujo del joystick se vera así:
> #### ⚠️ Nota:
> 
> Si conectaste conforme al circuito mostrado anteriormente, este flujo no requiere ningún cambio.
>
![Nodo Joystick](https://drive.google.com/uc?export=view&id=1SlbkJIzV_Y5nsaqykafrJ3jW41MpRZaa)


Flujo MQTT:
![Nodo MQTT y ESP32-CAM](https://drive.google.com/uc?export=view&id=1pkWTdG7wEtfFRdJ4lhcTubM9WBmmWGtd)

- Para configurarlo: 
1. Haz clic en el Nodo Template "ESP32-CAM":
   
![Configuración Cámara](https://drive.google.com/uc?export=view&id=1cFmeitIUm2DS3w1cASTMzBqo_tPY97_d)

Cambia la url por la designada en tu ESP32-CAM al cargar el código en Arduino IDE.

2. En el Nodo MQTT:

![Configuración Mqtt](https://drive.google.com/uc?export=view&id=18uHPqzY79aKesPvswjjCNYHxgttmsmtc)

En el apartado Topic, cambia al nombre del tópico que hayas creado.

3. Ahora ve al nodo HTTP request:

![Configuración url](https://drive.google.com/uc?export=view&id=1nPcnEwwb3JnD_kmKvOxvObTWbwwm7WJz)

Cambia la url por la designada en tu ESP32-CAM al cargar el código en Arduino IDE.

4. Por ultimo, para el nodo Write file: 

![Configuración Write file](https://drive.google.com/uc?export=view&id=1rIcNwGW4ruoimGUj3uLoqgKnA1Vu8no0)

Cambia la dirección al directorio donde se guardarán las fotos. 


### Envío de Notificaciones a Telegram:
Flujo Telegram:
> ### **⚠️ Importante:**
> 
> Antes de configurarlo, deberás crear un Bot en Telegram. Para más información, consulta el siguiente [apartado](https://sendpulse.com/latam/knowledge-base/chatbot/telegram/create-telegram-chatbot).
>
![Nodo Telegram](https://drive.google.com/uc?export=view&id=1AxLBTW0mbPid60VZyB2__tjFpKzE3WTO)


- Para configurarlo:
1. En el Nodo Telegram Sender:
   
![Configuración Bot](https://drive.google.com/uc?export=view&id=1xRq7QRayrZ6UHhDs9TOZZp_gnuSwhgLT)

Cambia el nombre del Bot, por el creado en Telegram.

2. Haz clic en editar y añade el token del Bot:
   
![Configuración TokenBot ](https://drive.google.com/uc?export=view&id=1Bbsu5phBAt7kNERbfOUDe1idY_2wRxW6)

3. Abre el nodo Watch file y añade la dirección donde se guardaron las fotos:

![Configuración TokenBot ](https://drive.google.com/uc?export=view&id=1p-17JlIfV2HOUYThMQ7Bt9XieH_m8aEv)

> **⚠️ Importante:**
> Asegúrate de configurar correctamente el entorno antes de ejecutar el proyecto.
> 

 
## Uso:

1. Accede al dashboard de Node-RED desde tu navegador en `http://<tu_ip_de_raspberry_pi>:1880/ui`.

![Vista Dashboard](https://drive.google.com/uc?export=view&id=1maa3yPvu2hxGT-C5u29EKql66BeggRKq)

2. Utiliza el joystick  y el slider en la interfaz para controlar el movimiento del carrito y de la cámara.
3. Monitorea desde de la interfaz.

![Vista Distancia](https://drive.google.com/uc?export=view&id=1RDivqjnibyzz6wmNpZ_P7obzaYD6cQrP)
![Vista Distancia2](https://drive.google.com/uc?export=view&id=1ciQBFPTMdOTsO2uHmH_BRyFJNcHP3BE8)

4. Recibe las notificaciones en Telegram

![Configuración TokenBot ](https://drive.google.com/uc?export=view&id=1P439zp3ty86hYFqsgo_H4HXCE9IzezKt)

## Funcionamiento

### Vista del Carrito:

![Vista Carrito](https://drive.google.com/uc?export=view&id=1RVF1NoQTFSUIVm7y7NbtMdiXCYd94EHb)

### Ejemplo de uso:
![Interfaz del Dashboard](https://drive.google.com/uc?export=view&id=1yDXHyg-rv4wxEwKaj0xDhi7v23-Mzmtu)

La interfaz mostrará el Joystick de movimiento, mientras que el Slider servirá para cambiar la vista de la cámara. Asimismo, se muestran los datos de distancia captada por el sensor.

![Movimiento Carrito y Cámara](https://drive.google.com/uc?export=view&id=1lWt3Lf004Ecdwx9--iPEXUQvUQArUAyh)


Ejemplo de movimiento del carrito y de la cámara. [Video de demostración](https://drive.google.com/file/d/1gK1cv0e9di2RRPNfv-FrCzitGafuj8BN/view?usp=sharing)


## Caso de uso:

### Identifición de objetos en áreas específicas para la optimización de la seguridad y trazabilidad en entornos industriales como bodegas y almacenes:
Permite implementar una solución innovadora para mejorar la seguridad y la logística en almacenes y bodegas. Equipado con una cámara ESP32-CAM y varios sensores, el carrito puede identificar intrusos o seguir objetos importantes. Si detecta algo sospechoso o inusual, envía una notificación inmediata a través de Telegram, mediante imágenes del objeto o intruso detectado.


## Servicios
- Monitoreo continuo de seguridad.
- Envío de notificaciones por medio de Telegram.
- Control remoto mediante Wifi.


## Créditos
- Aragón Díaz Yessica Gabriela
- Jaime Oliveros Diego Iván
- Luna Benítez Brian Hernán


## Agradecimientos
Este proyecto fue realizado en el marco del curso IoT Essentials Developer impartido por [Codigo IoT ](https://www.codigoiot.com/) y organizado por la [Asociación Mexicana del Internet de las Cosas](https://www.asociacioniot.org/).
