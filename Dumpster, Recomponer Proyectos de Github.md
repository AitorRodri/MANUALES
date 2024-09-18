<h1 align="center">RECOMPONER PROYECTOS DE GITHUB</h1>

Cuando la máquina víctima tiene un repositorio git en un servidor web podemos recomponerlo en nuestra máquina local para poder adquirir información del proyecto.

![image](https://github.com/user-attachments/assets/8fd4ea5f-576e-4263-b3eb-0ad67aac685f)

<h2>GITDUMPER</h2>

La herramienta gitdumper nos permite traer todo el repositorio .git y recomponerlo.

`python3 git_dumper.py \*url\* \*asignarle un nombre\*`

Para ver los logs del proyecto:

`git log`

En los logs he localizado que ocurre algo sospechoso ya que se añadió un archivo, se modificó y luego se elimino

![image](https://github.com/user-attachments/assets/9e6eda35-a62c-4f81-a693-c0f2a433bcce)

Para obtener más detalles sobre los archivos añadidos en un commit específico, puedes usar el siguiente comando:

`git show \*commit id\*`

![image](https://github.com/user-attachments/assets/dcc77185-f7e3-4506-ac8d-2aff5b20ad63)

Como no nos da mucha información utilizaremos el siguiente comando:

`git ls-tree \*commit id\*`

![image](https://github.com/user-attachments/assets/b4c472cb-91bf-4127-b38c-d7632bf9fd99)

Podemos ver que hay un archivo secret, intentaremos leerlo con el siguiente comando:

`git show \*secret commit id\*`

![image](https://github.com/user-attachments/assets/17ca5d44-cc3a-4b5a-a10e-45b3937452db)

Como no nos deja ver el contenido del archivo vamos a intentar conseguir información en la siguiente ruta:

`http://\*ip\*/.git/logs/HEAD`

![image](https://github.com/user-attachments/assets/94d535be-f6b8-462e-b59c-40e589ac90d3)

Como podemos ver, hemos localizado el proyecto de github original, tenemos que clonarlo y ver si podemos ver el contenido del archivo secrets desde ahí.

`git clone \*url\*`

Hacemos lo mismo:

- `git log`
- `git ls-tree \*commit id\*`
- `git show \*secret commit id\*`

El archivo secret contiene unas credenciales

![image](https://github.com/user-attachments/assets/5a488bad-cd8e-42bf-b42e-a75c16ec9645)
