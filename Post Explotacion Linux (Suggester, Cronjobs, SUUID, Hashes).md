<h1>Linux Kernel Exploits (Privilege escalation)</h1>

## **SUGGESTER**

En linux al igual que en Windows, podemos utilizar la herramienta de suggester para realizar la escalada de privilegios. El proceso es igual que el windows.

## **CRONJOBS MISS CONFIGURATIONS**

Caso práctico:

Estamos con el usuario student con bajos privilegios. En la carpeta home del usuario student hay un archivo solo puede leer y escribir root, por lo que parece interesante

![image](https://github.com/user-attachments/assets/f374d975-0b3f-424a-9969-c433083bf216)

Para saber si ese archivo está asociado a cualquier tarea programada:

`grep -rnw /usr /home/student/message`

![image](https://github.com/user-attachments/assets/411e098f-0ea7-40d5-8c4a-ba11c8692473)

Como podemos ver, el script copy.sh hace una copia del archivo /message a la carpeta tmp. Vamos a la carpeta tmp y vamos que el archivo no contiene nada interesante:

![image](https://github.com/user-attachments/assets/8f79ddc5-cf8e-4c38-bd39-a3943fe55864)

Pero podemos utilizar el script copy.sh para escalar privilegios a root ya que tenemos permisos:

![image](https://github.com/user-attachments/assets/a7ce2eb3-4f01-4c96-8415-166337665d22)

Para escalar a root lo que tenemos que hacer es añadir una linea de código al script copy.sh.

![image](https://github.com/user-attachments/assets/6fa6edaa-a190-4fe3-9fb2-b6d8c9f8ace9)

Le añadiremos lo siguiente:

`printf '#!/bin/bash\\necho "student ALL=NOPASSWD:ALL" >> /etc/sudoers' > /usr/local/share/copy.sh`

![image](https://github.com/user-attachments/assets/6e7b1bde-66db-459f-a327-cf545e0ef78d)

## **SUUID**

Caso práctico:

Tenemos acceso al usuario student y en su directorio podemos ver dos archivos, greetings y welcome. Sobre greetings no tenemos accesos pero sobre welcome podemos ejecutar como root. Al ejecutarlo solo vemos un texto que da la bienvenida.

![image](https://github.com/user-attachments/assets/38fdc237-cdd6-431c-af43-4913c6a39a8c)

Para saber más sobre el archivo welcome podemos ver los metadatos del archivo. Ahí vemos como nombre al archivo greetings

![image](https://github.com/user-attachments/assets/c35d3749-1f26-49d0-a669-b5ac44fee600)

Lo que hace el archivo welcome es ejecutar el archivo greetings. Entonces, para conseguir un bash como root tenemos que borrar el archivo greetings y crear uno nuevo con el mismo nombre para invocar la terminal, podemos copiar el bash original y llamarlo greetings:

`cp /bin/bash /home/student/greetings`

Cuando ejecutemos el archivo welcome se nos abrirá una terminal con permisos de administrador.

**PASSWORD HASHES**

Toda la información sobre las cuentas de usuario se almacena en /etc/passwd. Las contraseñas cifradas de los usuarios se encuentran en /etc/shadow, que solo se puede acceder con el usuario root.

Para saber que algoritmo es el que ha cifrado la contraseña tenemos que fijarnos en el valor que tiene el $:

![image](https://github.com/user-attachments/assets/c604971b-c3f8-4c7e-98ae-afd469c1b1ff)

Cuando conseguimos el hash de un usuario podemos utilizar la herramienta de meterpreter hashdump que desencripta las contraseñas hasheadas de una sesión de meterpreter

También podemos copiar la linea completa donde aparece la contraseña de root hasheada con el usuario y pegarla en un archivo para desencriptarlo con john teniendo en cuenta el formato que puede tener viendo la tabla de arriba
