SSH PORTFORWARDING

<https://tryhackme.com/r/room/badbyte>

![image](https://github.com/user-attachments/assets/2ef93306-22b8-43f3-a77e-6da0437d435e)

En este ejemplo hemos conseguido acceso por SSH a “Blue”. Hay otro servidor llamado “Red” que tiene abierto el puerto 80 y 22 pero el 80 se bloquea por un firewall.

Lo que podemos hacer es crear un tunel con ssh que utilice otro puerto para que todo lo que se solicite por el navegador viaje por el tunel ssh, y no por el puerto 80 que se bloque por el firewall.

Lo primero que hay que hacer es abir un tunel dinamico por un puerto, por ejemplo 1337.

- `ssh *nombre*@*ip* -i id_rsa -D 1337`

Luego cambiamos la configuración de /etc/proxychanges4.conf. Comentamos socks4 y añadimos la línea de socks5

![image](https://github.com/user-attachments/assets/d25ba077-5cf8-480d-9cc6-f12cedab9dd4)


Ahora cuando hagamos “proxychanges nmap -sT 127.0.0.1” es como si hiciéramos nmap a la máquina “Blue” a través del túnel ssh, sin ningún firewall de por medio.

![image](https://github.com/user-attachments/assets/2c67727b-ae3a-4aca-a922-4ee75375d0d0)


Para poder ver el contenido del puerto 80 desde nuestro kali por el puerto 8080 a través del túnel ssh:

- `ssh *nombre*@*ip* -i id_rsa -L 8080:127.0.0.1:80`
  
![image](https://github.com/user-attachments/assets/47c8b299-938d-4ce0-88a6-3b5e0015334c)


Con wpscan y con nmap podemos mostrar los plugins que tiene instalado.

![image](https://github.com/user-attachments/assets/5fad6b84-5ee8-4756-9fe7-efff86006555)


Con wpscan no encontramos nada pero con nmap encontramos dos plugins que son vulnerables.

![image](https://github.com/user-attachments/assets/e53c14b8-dd4f-4a1c-bb87-dc2f367c73ec)


Duplicator: puedes ver los archivos de la máquina víctima.

Wp-file-manager: acceso a la maquina victima. En metasploit podemos encontrarlo buscando su CVE.
