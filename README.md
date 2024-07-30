# Carrito inteligente de monitoreo para la detección de objetos

## Descripción

Este proyecto busca desarrollar un prototipo de carrito inteligente capaz de detectar objetos y movimientos, ofreciendo una solución innovadora para mejorar la seguridad y la logística en almacenes y bodegas. Equipado con una cámara ESP32-CAM y varios sensores, el carrito puede identificar intrusos o seguir objetos importantes. Si detecta algo sospechoso o inusual, envía una notificación inmediata a través de Telegram, incluyendo imágenes del objeto o intruso detectado.

## Objetivos Generales

Desarrollar un prototipo de carro inteligente para monitoreo y detección de objetos y movimientos que permita identificar la presencia de objetos en áreas específicas, optimizando la seguridad y trazabilidad en entornos industriales como bodegas y almacenes.

## Objetivos Específicos

- Implementar una interfaz para el control del carrito a través de Wifi o Bluetooth.
- Enviar notificaciones a través de la plataforma de Telegram cuando se detecte la presencia de objetos.
- Integrar alertas visuales y sonoras mediante Buzzer y LED cuando se detecten situaciones inusuales o de interés.

## Productos

### Hardware

- **Carrito Inteligente**: Estructura móvil equipada con ruedas y motores controlados por el driver L298N.
- **Cámara ESP32-CAM**: Captura de imágenes y video en tiempo real, procesamiento básico de imágenes y detección de objetos.
- **Raspberry Pi 4**: Centro de control del sistema, gestionando la lógica del monitoreo, almacenamiento de datos, y envío de notificaciones. Conectividad a internet y a la red local.
- **Sensores Ultrasónicos**: Medición de distancia y detección de obstáculos.
- **Actuadores**: Buzzer para emisión de alertas sonoras y LEDs para señales visuales de alerta. Driver L298N para el control de los motores del carrito.

### Software

- **Firmware para ESP32-CAM**: Captura de imágenes y detección de objetos utilizando TensorFlow Lite o una librería similar. Comunicación con la Raspberry Pi 4 y envío de datos de detección.
- **Sistema Operativo y Aplicaciones en Raspberry Pi 4**:
  - Node-RED: Orquestación de los flujos de datos, incluyendo la recepción de datos de la ESP32-CAM, procesamiento y envío de notificaciones a Telegram.
  - MQTT: Comunicación entre la ESP32-CAM y la Raspberry Pi.
  - Python: Manejo de la lógica de detección y control de actuadores (buzzer y LEDs).
- **Conexión Bluetooth o WiFi**: Permite el control del carrito desde un dispositivo móvil.
- **Almacenamiento y Gestión de Datos**: Base de Datos Local para almacenamiento de imágenes y datos de detección para análisis posterior.

## Servicios

- Monitoreo continuo de seguridad.
- Envío de notificaciones por medio de Telegram.
- Control remoto mediante Wifi o Bluetooth.

## Resultados Esperados

- Reducción de incidentes de seguridad mediante la detección de situaciones inusuales.
- Mejora en la toma de decisiones preventivas.
- Monitoreo constante mediante recolección de imágenes.

## Instalación

Sigue estos pasos para instalar y configurar el proyecto:

1. Clona el repositorio:
   ```bash
   git clone https://github.com/LordCadeu/Carrito-Inteligente-De-Monitoreo.git

2. Navega al directorio del proyecto:
   ```bash
   cd carrito-inteligente
   
3. Instala las dependencias:
   ```bash
   npm install

## Uso

### ESP32-CAM

#### Configuración de Arduino IDE:

1. Instala el soporte para ESP32 en Arduino IDE siguiendo [estas instrucciones](https://github.com/espressif/arduino-esp32#installation-instructions).
2. Abre Arduino IDE y selecciona la placa `ESP32 Wrover Module` en `Herramientas > Placa > ESP32 Wrover Module`.
3. Instala la librería `ESP32-CAM` desde el gestor de librerías.

#### Carga del Código:

1. Abre el sketch `carrito_inteligente.ino` que se encuentra en la carpeta `arduino`.
2. Configura los parámetros de tu red WiFi en el sketch.
3. Carga el código en la ESP32-CAM.

### Node-RED

#### Instalación de Node-RED:

1. Instala Node-RED en tu Raspberry Pi siguiendo [estas instrucciones](https://nodered.org/docs/getting-started/raspberrypi).

#### Configuración del Dashboard:

1. Importa el flow `carrito_inteligente_flow.json` que se encuentra en la carpeta `node-red`.
2. Asegúrate de tener instalados los siguientes nodos adicionales:
   - `node-red-dashboard`: Para la interfaz de control del joystick.
   - `node-red-contrib-telegrambot`: Para enviar mensajes a Telegram.

#### Uso del Joystick en el Dashboard:

1. Accede al dashboard de Node-RED desde tu navegador en `http://<tu_ip_de_raspberry_pi>:1880/ui`.
2. Utiliza el joystick en la interfaz para controlar el movimiento del carrito.

#### Envío de Notificaciones a Telegram:

1. Configura el nodo de Telegram con tu bot token y chat ID.
2. Asegúrate de que los nodos de detección de objetos estén correctamente configurados para enviar mensajes a Telegram cuando se detecte un objeto.

    




## Créditos
  . Aragón Díaz Yessica Gabriela
  . Jaime Oliveros Diego Iván
  . Luna Benítez Brian Hernán
