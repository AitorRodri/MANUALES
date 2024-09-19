<h1 align="center">Reconocimiento activo</h1>

El reconocimiento pasivo trata de recopilar la mayor información posible interactuando directamente con el objetivo

# **DNS ZONE TRANSFERS**

**\- Qué información contiene un servidor dns?**

![image](https://github.com/user-attachments/assets/8ded5480-57ee-4deb-9d0b-c07cdebd049a)

# **DNSENUM**

Es una herramienta de kali linux que se utiliza para saber la dirección ip, servidor web, aliases… que tiene asociado un nombre de dominio.

**Uso:**

- `dnsenum ine.com`

![image](https://github.com/user-attachments/assets/06a65dd2-fdbe-4459-8df9-9c11690ac94e)

# **DIG**

Es un comando que se utiliza para realizar consultas dns. El tipo de consulta “axfr” se utiliza para intentar copiar todo ese libro de direcciones (la zona) desde un servidor DNS.

**Uso:**

- `dig axfr @\*nombre_servidor_dns\* \*dominio\*`

![image](https://github.com/user-attachments/assets/ec5ba6fa-0e3d-4fa5-961f-1e5b975167ee)

# **FIERCE**

Es una herramienta de enumeración de DNS utilizada para identificar y mapear subdominios y otros registros DNS de un dominio objetivo.

**Uso:**

- `fierce -dns ine.com`

# **NMAP**

Es una herramienta que se utiliza para descubrir los equipos de una red y los puertos abiertos que tiene un equipo.

**Uso:**

**Para descubrir los equipos activos de la red en la que nos encontramos podemos lanzar una combinación de paquetes con un ping a través del protocolo ICMP:**

- `sudo nmap -sn 192.168.11.0/24`

**Windows suele bloqueado la entrada de algunos paquetes del protocolo ICMP por lo que podemos podemos probar a enviar paquetes solamente TCP SYN para establecer la conexión (Con -sn solamente le decimos que no haga un escaneo de puertos):**

`sudo nmap -sn -PS 192.168.11.0/24 —> La que suele ser mejor`

**También se pueden enviar paquetes TCP ACK, solo que en algunos casos suele estar configurado para bloquear los paquetes que contiene la flag ACK**

- `sudo nmap -sn -PA 192.168.11.0/24`

**También se pueden enviar paquetes solamente ICMP echo request**

- `sudo nmap -sn -PE 192.168.11.0/24`

**Si no se envia ningun paquete es porque estamos en una “ethernet local network”, por lo que hay que especificarle el --send-ping:**

- `sudo nmap -sn -PE 192.168.11.0/24 --send-ping`

**Para escanear los puertos abiertos del protocolo UDP podemos utilizar:**

- `sudo nmap -sU -Pn 192.168.11.20`

**Para descubrir los puertos abiertos de un equipo:**

- `sudo nmap 192.168.11.20 -sCV -sS -T4 -vvv -O --osscan-guess -Pn -p- --open -oN scan.txt`

**Cuando quieres lanzar un script a algun servicio con nmap puedes buscarlos en:**

- `ls /usr/share/nmap/scripts|grep *mongodb*`

**Para saber lo que hace el script:**

- `sudo nmap --script-help=*nombre del script*`

**Ejemplo de Host Discovery:**

- `sudo nmap -sn -PS *red*`

**Ejemplo de escaneo de puertos:**

- `sudo nmap *ip* -sS -Pn -O --osscan-guess -sVC -T4 -p- -vvv -sCV -n -oN scan.txt`

**Para saber si el equipo al que estamos haciendo un escaneo de puertos está detrás de un firewall podemos utilizar -sA. Nos mostrará “filtered” si hay firewall.**

- `sudo nmap -p- -sA -Pn *ip*`

**Para evadir el firewall con nuestro escaneo de puertos podemos fragmentar los paquetes que enviamos con -f:**

- `sudo nmap -p- -sS -Pn -f *ip*`

**Tambien puedes hacerte pasar por otra IP para que sea menos sospechoso el escaneo, por ejemplo, hacerte pasar por la puerta de enlace. Es una forma de poder evadir el firewall:**

- `sudo nmap -sS -Pn -f -p- -D *ip_spoofing* *ip_escaneo*`

**Tambien se puede cambiar cual es el puerto que esta solicitando el escaneo. Para hacer creer a los que monitorizan la red que el dns de la puerta de enlace esta enviando paquetes a todos los puertos de la maquina victima:**

- `sudo nmap -sS -Pn -f -p- -g 53 -D *ip_spoofed* *ip_escaneo*`

# **NETDISCOVER**

Se utiliza para descubrir los equipos activos de la red en la que nos encontramos:

**Uso:**

- `sudo netdiscover -i eth0 -r 192.168.11.0/24`

![image](https://github.com/user-attachments/assets/284a02cd-cce7-45ae-a40c-8ce85a435eeb)

# **fping**

**Uso:**

- `fping -agq 192.168.11.0/24`

- Muestra las ips de nuestra red privada a traves de un ping
- \-a: Para que solo se muestren maquinas activas
- \-g: Para que genere el ping
- \-q: quiet, para que no se muestren pings fallidos

# **ZENMAP**

Es una herramienta de windows parecida a nmap, realiza el “host discovery” de una red y escaneo de puertos y servicios de cada host

# **Arp-scan**

- `arp-scan -I enp0s3 --localnet`

- Forma para conocer los equipos que estan dentro de mi red privada y sus direcciones mac
