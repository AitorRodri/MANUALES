<h1 align="center">REVERSE SHELL</h1>

Una **reverse shell** (o shell inversa) es una técnica utilizada en la ciberseguridad para obtener acceso remoto a un sistema objetivo. A diferencia de una shell tradicional, en la que un atacante se conecta directamente al sistema objetivo, en una reverse shell el sistema comprometido se conecta de vuelta al atacante. Esto se usa a menudo para eludir firewalls y sistemas de detección que permiten conexiones salientes pero no entrantes.

# Donde adquirir las reverse shells

<https://www.revshells.com/>

<https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet>

# Ejemplo de explotación a través de una reverse shells

Una vez que tenemos ejecucion de comandos remota, para poder acceder de forma remota y escalar privilegios utilizaremos el siguiente comando en la maquina victima para que netcat pueda interceptar la conexión:

1) Existe una ruta en el kali que contiene exploits para realizar la reverse shell:

- /usr/share/webshells

2) Una vez ahí buscamos el que queremos utilizar y modificamos el archivo con los parámetros que queremos

3) Se lo envias a la maquina victima (por ejemplo por ftp)

4) Desde el kali nos ponemos a la escucha con netcat

- nc -lvnp \*puerto especificado en el exploit\*

5) Si se ha lanzado de forma correcta obtendremos la siguiente shell en la terminal:
![image](https://github.com/user-attachments/assets/307cb1b1-bc33-414b-b7ed-8831deeac6d5)
