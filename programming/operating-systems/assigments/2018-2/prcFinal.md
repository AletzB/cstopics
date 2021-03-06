---
layout: default
title: Práctica Final
permalink: /programming/operating-systems/assigments/2018-2/prcFinal
---

# Práctica Final

Éste laboratorio se basará en la práctica [Práctica 4](/cstopics/programming/operating-systems/assigments/prc4), el UOS (Useless Operating System) será convertido en el UOS (Useful operating system).

### Mejoras y adiciones al sistema

Se deberán agregar las siguientes características al USO que ya desarrollaron:

* La capacidad de enviar mensajes *broadcast*, es decir, que los envíe un nodo, y éste llegue a todos los demás nodos conectados al máster.
* Los nodos pueden estar ejecutándose en diferentes equipos, conectándose a través de TCP-IP.
* El máster seguirá siendo un proceso independiente, el cual será ejecutado una única vez para conectar los nodos correspondientes, sin embargo, los clientes ya no serán el mismo proceso. Cualquier programa podrá ser un cleinte UOS, siempre que incluya la librería correspondiente (*uos_client.h* y "uos_client.cpp"), la cual será escrita por ustedes, y debería tener funcinoes como por ejemplo:
    * *connect_to_master(IP_ADDRESS);*
    * *send_str_message(DESTINATION_NODE)*
    * *set_reception_callback(POINTER_TO_RECEPTION_FUNCTION)*

### Robot diferencial

Deben usar un robot diferencial que cuente con lo siguiente:

* Dos ruedas con motores independiente, y una rueda loca. Dependiendo del sentido de giro y la velocidad de cada motor, el robot se moverá hacia adelante, atrás, girará o se detendrá.
* Un sensor de distancia (ultrasónico, láser, o la tecnología deseada).
* Una cámara.

<div style="text-align:center">
  <img style="width:40%;" src ="/cstopics/assets/img/programming/os/robot_diferencial.jpg" />
  <div style="font-size:70%">Ejemplo de robot diferencial.</div>
</div>

Todos los periféricos (motores, sensor y cámara) deben ser conectados al sistema embebido utilizado (Raspberry o Beaglebone), haciendo las interfaces necesarias (puente h, buffer, etc), asegurándose de no dañar las entradas y salidas digitales. Todo se debe alimentar con fuentes que permitan el desplazamiento del robot (por ejemplo baterías), tener en cuenta el voltaje de alimentación de cada elemento.

### Implementación del USO con el robot diferencial

En la siguiente figura se sintetiza el sistema a implementar a partir de nodos de UOS:

<div style="text-align:center">
  <img style="width:70%;" src ="/cstopics/assets/img/programming/os/UOS.png" />
  <div style="font-size:70%">Esquema a los nodos a implementar.</div>
</div>

Cada nodo debe ser implementado y se debe subscribir al máster a través de la librería *uos_client*, escrita por ustedes.

Explicación del funcionamiento de cada nodo:

***Nodos en el robot***
* *motors_controller:* debe recibir un mensaje de tipo *num_array*, el debe contener dos números que representan el movimiento de cada motor [motor_derecho, motor_izquierdo], donde 1.0 representa movimieto hacia adelante, -1.0 hacia atrás, y 0.0 detenido.
* *sensor_controller:* envía un mensaje tipo *num* con un 1.0 si hay un objeto frente al robot, o un 0.0 si no lo hay. Éste mensaje debe ser de tipo *broadcast*.
* *camera_controller:* espera un mensaje de tipo *str* con la palabra 'SHOT', cuando lo recibe, toma una foto y la envía por *ssh* al computador.

***Nodos en el computador***
* *controller:* realiza toda la lógica explicada más adelante (no se debe ejecutae en simultánea con *manual_controller*).
* *manual_controller:* a través de teclas, controla manualmente el sentido de los motores de forma independiente (no se debe ejecutar en simultánea con *controller*).
* *gui:* Gui en Qt que muestre:
  * El estado los dos motores.
  * El estado del sensor.
  * La última foto tomada.
  * Demás variables que considere relevantes.

El sistema debe cumplir los siguientes requisitos:
* Se puede apagar cualquier nodo (cerrar el proceso), sin que los demás nodos fallen, simpemente su función se deja de ejecutar.

## Nodo *controller*

Este nodo debe realizar el siguiente funcionamiento en el robot:

El robot debe avanzar hacia adelante, cuando el sensor encuentre un obstáculo, debe tomar una foto y enviarla, luego evade el obstáculo girando hacia una dirección aleatoria, y vuelve a avanzar hacia adelante.

## Recomendaciones

* Estudiar y aplicar apuntadores a funciones, para implementar *callbacks* cuando lleguen mensajes.
