<h1 align="center">EXPLOTAR VULNERABILIDADES EN WINDOWS</h1>

<h2>**EXPLOTARLO CON ETERNALBLUE**</h2>

EternalBlue permite la ejecución de código remota a la máquina víctima, lo que se puede conseguir una reverse shell o una sesión de meterpreter. Además, con privilegios altos.

**Afecta a:**

-WINDOWS VISTA

-WINDOWS 7

-WINDOWS SERVER 2008

-WINDOWS 8.1

-WINDOWS SERVER 2012

-WINDOWS 10

-WINDOWS SERVER 2016



Se puede comprobar a través de metasploit si es vulnerable a eternal blue con:

`search eternal blue`

Ahí encontraremos un scaner que valora si es vulnerable a eternablue y tendremos otro modulo que ejecuta la explotación

<h2>**EXPLOTARLO CON BLUEKEEP**</h2>

Al igual que EternalBlue, permite la ejecución de código remota a la máquina victima, lo que se puede conseguir una reverse shell o una sesión de meterpreter. Además, con privilegios altos.

**Afecta a:**

-WINDOWS XP
-WINDOWS VISTA
-WINDOWS 7
-WINDOWSS SERVER 2008 & R2

Se puede comprobar a través de metasploit si es vulnerable a eternal blue con:

`search bluekeep`

Ahí encontraremos un scaner que valora si es vulnerable a eternalblue y tendremos otro modulo que ejecuta la explotacion (ESTE MODULO SOLO FUNCIONA PARA MAQUINAS DE 64BITS). Tambien tendremos que especificar el SO del target con:

`show targets`
`set TARGET \*numero del SO al que corresponde\*`
