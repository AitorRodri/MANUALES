<h1 align="center">RECONOCIMIENTO PASIVO</h1>

Para recopilar la máxima información posible de un dominio sin interactuar con él, podemos utilizar las siguientes herramientas:

# **WHOIS**

![image](https://github.com/user-attachments/assets/919b2e7e-9f82-42f5-9d8f-f1b9b94e08f5)

# **NSLOOKUP**

Para saber a qué IP corresponde un nombre de dominio podemos utilizar esta herramienta con los siguientes parámetros:

`nslookup -type=A tryhackme.com 1.1.1.1`

Otros parametros:  

![image](https://github.com/user-attachments/assets/8dc675b5-5c2d-44f2-9e24-c83771667fe2)

# **DIG**

Funciona igual que nslookup, es una alternativa. Uso:

`dig tryhackme.com A`

![image](https://github.com/user-attachments/assets/88f59325-3b47-41ef-8219-0277b28d96c5)

# **DNSDUMPSTER**

Es una herramienta online que muestra información de un dominio: El servidor DNS que utiliza, el servidor MX (de correo), registros TXT del sistema y subdominios.

Los registros txt se utilizan para almacenar información de texto que puede ser utilizada para una variedad de propósitos, como la verificación de propiedad de dominio y la configuración de políticas de correo electrónico.

# **SHODAN**

Es otra herramienta online que se utiliza para recopilar información.

![image](https://github.com/user-attachments/assets/5a4e641f-9b08-4976-8f97-0eb5451cabc6)

# **RESUMEN:**

![image](https://github.com/user-attachments/assets/482ac3a4-41bc-4b1b-b5fd-b8bfbde29cdd)
