<h1 align="center">INFORMACIÓN POST-EXPLOTACIÓN</h1>

# **WINDOWS**

### Información del Sistema

- **systeminfo**: Muestra información detallada sobre el sistema, incluyendo el nombre del sistema, la versión del sistema operativo, la configuración del hardware, la configuración de red y más.

### Usuarios y Grupos

- **whoami /priv**: Muestra los privilegios del usuario actual.
- **query user**: Lista los usuarios que han iniciado sesión en el sistema.
- **net users**: Lista los usuarios locales del sistema.
- **net user &lt;user&gt;**: Muestra información detallada sobre un usuario específico.
- **net localgroup**: Lista los grupos locales del sistema.
- **net localgroup &lt;groupname&gt;**: Muestra información detallada sobre un grupo específico.
- **wmic qfe get Caption,Description,HotFixID,InstalledOn**: Obtiene información sobre los hotfixes (actualizaciones rápidas) instalados en el sistema.

### Información de Red

- **ipconfig /all**: Proporciona una salida detallada que incluye información sobre adaptadores de red físicos y virtuales.
- **route print**: Muestra la tabla de enrutamiento del sistema.
- **arp -a**: Muestra la tabla de caché ARP, que mapea direcciones IP a direcciones MAC.
- **netstat -ano**: Muestra estadísticas detalladas de la red, incluyendo conexiones activas y puertos escuchando.
- **netstat -antp**: Lista servicios escuchando en puertos abiertos.
- **netsh firewall show state**: Muestra el estado del Firewall de Windows.
- **netsh advfirewall show allprofiles**: Muestra la configuración del Firewall Avanzado de Windows para todos los perfiles de red.

### Procesos y Servicios

- **net start**: Lista los servicios que están actualmente en ejecución.
- **wmic service list brief**: Muestra una lista de servicios instalados en el sistema.
- **tasklist /SVC**: Muestra una lista de procesos en ejecución, incluyendo los servicios asociados a cada proceso.
- **schtasks /query /fo LIST /v**: Muestra una lista de tareas programadas en el sistema.

### Enumeración Automática

**JAWS-ENUM**: Un script en PowerShell para Windows que enumera usuarios, servicios, puertos e interfaces.

- Clona el repositorio de GitHub: [JAWS](https://github.com/411Hall/JAWS.git)
- Crea una carpeta en C: llamada temp, sube el archivo .ps1 con Meterpreter y ejecútalo:
  - powershell.exe -ExecutionPolicy Bypass -File .\\jaws-enum.ps1 -OutputFilename JAWS-Enum.txt
- Descarga el archivo JAWS-Enum.txt generado y revisa su contenido.

## LINUX

### Información del Sistema

- **hostname**: Muestra el nombre del sistema.
- **cat /etc/issue**: Muestra información sobre la distribución de Linux y su versión.
- **cat /etc/\*release**: Muestra información detallada sobre la versión y la distribución de Linux.
- **uname -a**: Muestra información del sistema, incluyendo el tipo de kernel y la arquitectura del hardware.
- **uname -r**: Muestra la versión del kernel.
- **lscpu**: Muestra información detallada sobre la CPU del sistema.
- **free -h**: Muestra el uso de la memoria RAM del sistema.
- **df -h**: Muestra el espacio en disco y su uso.
- **df -ht ext4**: Filtra la salida de df para mostrar solo los sistemas de archivos del tipo ext4.
- **lsblk | grep sd**: Muestra una lista de dispositivos de almacenamiento que contienen "sd" en su nombre.
- **dpkg -l**: Lista todos los paquetes instalados en el sistema.
- **env**: Muestra las variables de entorno del usuario.

### Usuarios y Grupos

- **groups &lt;user&gt;**: Muestra los grupos a los que pertenece un usuario específico.
- **groups**: Lista todos los grupos en el sistema.
- **cat /etc/passwd**: Muestra información sobre los usuarios del sistema.
- **who**: Muestra información sobre los usuarios actualmente conectados.
- **last**: Muestra un historial de inicio de sesión.
- **lastlog**: Muestra la última vez que cada usuario inició sesión en el sistema.

### Información de Red

- **ip a s**: Muestra información detallada sobre las interfaces de red.
- **cat /etc/networks**: Muestra la configuración de red del sistema.
- **cat /etc/hosts**: Muestra la tabla de hosts del sistema.
- **cat /etc/hostname**: Muestra el nombre del host del sistema.
- **cat /etc/resolv.conf**: Muestra la configuración de resolución de nombres del sistema.
- **arp -a**: Muestra la tabla de ARP, que mapea direcciones IP a direcciones MAC.

### Procesos y Cronjobs

- **ps aux**: Muestra información detallada sobre todos los procesos en ejecución.
- **crontab -l**: Muestra las tareas programadas para el usuario actual.
- **cat /etc/crontab**: Muestra las tareas programadas a nivel de sistema.

###

### Enumeración Automática

**LinEnum**: Un script en Linux para enumerar información del sistema comprometido.

- Clona el repositorio de GitHub: [LinEnum](https://github.com/rebootuser/LinEnum.git)
- Crea una carpeta en el sistema y sube el script con Meterpreter, luego dale permisos de ejecución y ejecútalo:
  - ./Linenum.sh
  - Descarga el archivo .txt generado y revisa su contenido.

## METERPRETER

### Información del Sistema

- **sysinfo**: Muestra información detallada sobre el sistema comprometido, como el sistema operativo, arquitectura del sistema, nombre del host, etc.
- **getuid**: Muestra el ID de usuario actual bajo el cual se está ejecutando Meterpreter en el sistema comprometido.
- **getprivs**: Muestra los privilegios que tiene el proceso Meterpreter actual en el sistema comprometido.

### Procesos

- **ps**: Lista los procesos en ejecución en el sistema comprometido.
- **pgrep &lt;processName&gt;**: Busca y muestra el ID del proceso para un nombre de proceso específico.
- **migrate &lt;PID&gt;**: Mueve el proceso Meterpreter actual a otro proceso en ejecución en el sistema con el ID de proceso especificado.

### Información de Red

- **ifconfig**: Muestra la configuración de red de la interfaz actual en el sistema comprometido.
- **netstat**: Muestra estadísticas de red, como conexiones activas, tablas de enrutamiento, estadísticas de protocolo, etc.
- **route**: Muestra y manipula la tabla de enrutamiento del kernel en el sistema comprometido.
- **arp**: Muestra la tabla de caché ARP del sistema comprometido, que mapea direcciones IP a direcciones MAC.

## **MÓDULOS DE POST-EXPLOTACIÓN EN METASPLOIT**

### **Enumeración de Archivos de Configuración**

- **search enum_configs**: Lista los archivos de configuración disponibles en el sistema. Los archivos en verde están disponibles y accesibles, mientras que los que fallan no existen.

### **Información del Entorno**

- **use post/multi/gather/env**: Enumera información sobre el entorno del sistema, incluyendo la versión de Debian y el kernel.
- loot para ver la información más ordenada.

### **Configuración de Red y Servicios**

- **search enum_network**: Enumera la configuración de SSH, red, firewall y DNS.
- loot para ver la información más ordenada.

### **Protecciones del Sistema**

- **search enum_protections**: Enumera protecciones de Linux como SELinux e iptables.
- notes para ver la información más ordenada.

### **Información del Sistema**

- **search enum system**: Enumera detalles sobre la distribución y el kernel del sistema.
- loot para ver la información más ordenada.

### **Comprobación de Contenedores y Máquinas Virtuales**

- **search checkcontainer**: Comprueba si el sistema comprometido está ejecutándose dentro de un contenedor Docker.
- **search checkvm**: Comprueba si el sistema comprometido está ejecutándose dentro de una máquina virtual.

### **Historial y Sesiones de Usuarios**

- **search enum_users_history**: Enumera el historial de los usuarios en el sistema.
- loot para ver la información más ordenada.
- **search enum_loged_on_users**: Enumera los usuarios que han iniciado sesión en el sistema.

### **Enumeración de Equipos en el Dominio**

- **search enum_computers**: Si el sistema forma parte de un dominio, enumera los equipos conectados a esa red.

### **Parches Instalados**

- **search enum_patches**: Lista los parches instalados en el sistema y sus fechas.
