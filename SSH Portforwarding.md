SSH PORTFORWARDING

<https://tryhackme.com/r/room/badbyte>

![image](https://github.com/user-attachments/assets/2ef93306-22b8-43f3-a77e-6da0437d435e)

En este ejemplo hemos conseguido acceso por SSH a “Blue”. Hay otro servidor llamado “Red” que tiene abierto el puerto 80 y 22 pero el 80 se bloquea por un firewall.

Lo que podemos hacer es crear un tunel con ssh que utilice otro puerto para que todo lo que se solicite por el navegador viaje por el tunel ssh, y no por el puerto 80 que se bloque por el firewall.

Lo primero que hay que hacer es abir un tunel dinamico por un puerto, por ejemplo 1337.

- ssh \*nombre\*@\*ip\* -i id_rsa -D 1337

Luego cambiamos la configuración de /etc/proxychanges4.conf. Comentamos socks4 y añadimos la línea de socks5

![image](https://github.com/user-attachments/assets/e8998ef2-888b-445c-93d3-1d23d0c616e4)


Ahora cuando hagamos “proxychanges nmap -sT 127.0.0.1” es como si hiciéramos nmap a la máquina “Blue” a través del túnel ssh, sin ningún firewall de por medio.

![image](https://github.com/user-attachments/assets/b5756fac-6537-43fc-beb6-89b5c0b8de1e)


Para poder ver el contenido del puerto 80 desde nuestro kali por el puerto 8080 a través del túnel ssh:

- ssh \*nombre\*@\*ip\* -i id_rsa -L 8080:127.0.0.1:80
  
![image](https://github.com/user-attachments/assets/5b67dff9-80ca-458e-a1a7-468d10f29146)


Con wpscan y con nmap podemos mostrar los plugins que tiene instalado.

![image](https://github.com/user-attachments/assets/e4870de6-fd33-45b9-b349-b102e5f89fe0)


Con wpscan no encontramos nada pero con nmap encontramos dos plugins que son vulnerables.

![image](https://github.com/user-attachments/assets/59c9859d-2fe6-4e3f-bfd4-83abc6938ab9)


Duplicator: puedes ver los archivos de la máquina víctima.

Wp-file-manager: acceso a la maquina victima. En metasploit podemos encontrarlo buscando su CVE.
