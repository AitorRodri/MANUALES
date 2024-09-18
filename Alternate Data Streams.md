<h1 align="center">Alternate Data Streams (ADS)</h1>

## **Introducción**

**Alternate Data Streams (ADS)** es un atributo de archivo del sistema de archivos NTFS (New Technology File System). Fue diseñado para proporcionar compatibilidad con el sistema de archivos HFS (Hierarchical File System) de MacOS.

## **NTFS y ADS**

NTFS es el formato de archivos por defecto en las instalaciones de Windows. Cuando creamos un archivo con formato NTFS, que contenga cualquier extensión, habrá dos tipos de datos distintos:

1. **Data Stream:** Contiene los datos que hay en el interior del archivo. Por ejemplo, si es un archivo .txt, el "data stream" sería el contenido que tenemos escrito en el .txt.
2. **Resource Stream:** Contiene los metadatos del archivo. Por ejemplo: quién creó el archivo .txt, cuándo se creó, etc.

## **Uso Malicioso de ADS**

Los atacantes pueden usar ADS para esconder código malicioso o ejecutables en el interior de archivos legítimos con la intención de evadir la detección de antivirus.

##

## **Puesta en Práctica**

### **Creación y Manipulación de ADS**

Abrimos la terminal en nuestro sistema Windows con permisos de administrador.

Creamos un archivo .txt llamado test.txt con Notepad y le agregamos el siguiente comando:

- `notepad text.txt:secret.txt`

Lo que hemos hecho es esconder el archivo secret.txt dentro de test.txt. Cuando abrimos por primera vez test.txt, en realidad estamos accediendo a secret.txt para introducir el contenido que no se verá en test.txt. Cuando guardamos y cerramos test.txt, ese texto no será visible en el archivo principal.

Para acceder al contenido de secret.txt debemos utilizar el siguiente comando en la terminal:

- `notepad text.txt:secret.txt`

### **Uso de ADS con Ejecutables**

Para esconder un ejecutable, como payload.exe, hacemos lo siguiente:

- `type payload.exe > windowslog.txt:payload.exe`

Esto creará un archivo windowslog.txt vacío con payload.exe escondido detrás de él.

Para ejecutar el payload.exe, primero tenemos que crear un acceso simbólico en C:\\Windows\\System32 y lo llamaremos wupdate.exe. Cuando hagamos clic en él, se ejecutará el payload:

- `mklink wupdate.exe \*ruta_absoluta_windowslog.txt:payload.exe\*`
