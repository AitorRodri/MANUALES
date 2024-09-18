<h1 align="center">Pasar de una Shell Limitada a una Terminal Interactiva</h1>

Cuando obtenemos una shell limitada (marcada por $), es útil convertirla en una terminal interactiva completa para facilitar la ejecución de comandos y la navegación. Aquí hay varios métodos para hacerlo:

## **Usando Python**

- `python3 -c 'import pty;pty.spawn("/bin/bash")'`
- `CTL+Z`
- `stty raw -echo; fg`
- `export TERM=xterm`

## **Usando el comando script**

- `script /dev/null -c bash`
- `CTL+Z`
- `stty raw -echo; fg`
- `export TERM=xterm`

## **Shell interactiva directa**

- `/bin/bash -i`
