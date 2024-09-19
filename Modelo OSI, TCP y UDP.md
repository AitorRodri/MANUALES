<h1 align="center">MODELO OSI</h1>

![image](https://github.com/user-attachments/assets/7ffe43e2-a68a-4dcc-9e86-4c7c9ccd7e97)

<h2 align="center">PROTOCOLO TCP</h2>

## **CARACTERÍSTICAS**

Orientado a Conexión:

- TCP establece una conexión entre el remitente y el receptor antes de intercambiar datos. Esta conexión es un circuito virtual que garantiza una transferencia de datos fiable y ordenada.

Fiabilidad:

- TCP garantiza la entrega fiable de datos. Logra esto mediante mecanismos como los acuses de recibo (ACK) y la retransmisión de paquetes perdidos o corruptos. Si un segmento de datos no recibe acuse de recibo, TCP reenvía automáticamente el segmento.

Transferencia de Datos Ordenada:

- TCP asegura que los datos se entreguen en el orden correcto. Si los segmentos de datos llegan fuera de orden, TCP los reordena antes de pasarlos a la aplicación de nivel superior.

## **COMO SE REALIZA LA CONEXIÓN TCP**

![image](https://github.com/user-attachments/assets/829fdafe-2840-4c59-8d7a-427954fef97d)

## **Proceso:**

1- SYN: El cliente le manda un paquete diciendo que quiere establecer una conexión

2- SYN-ACK: El servidor le responde diciendo que también quiere establecer una conexión y le dice que todos los paquetes han llegado correctamente (ACK)

3- ACK: El cliente le responde que ha recibido todos los paquetes

<h2 align="center">PROTOCOLO UDP</h2>

## **CARACTERÍSTICAS**


**No Orientado a Conexión:**

- UDP no establece una conexión entre el remitente y el receptor antes de intercambiar datos. No hay un proceso de establecimiento de conexión, y los datagramas se envían sin verificar la preparación del receptor.

**Fiabilidad:**

- UDP no garantiza la entrega fiable de datos. No utiliza mecanismos de acuse de recibo (ACK) ni retransmisión de paquetes perdidos o corruptos. Los datagramas se envían sin asegurar que hayan llegado correctamente o en el orden correcto.

**Transferencia de Datos No Ordenada:**

- UDP no garantiza el orden de entrega de los datos. Los datagramas pueden llegar en un orden diferente al que fueron enviados. No hay mecanismos en UDP para reordenar los datagramas en caso de que lleguen desordenados.
