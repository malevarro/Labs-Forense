# Laboratorio 2. Full Live Response

- [Laboratorio 2. Full Live Response](#laboratorio-2-full-live-response)
  - [Objetivos](#objetivos)
  - [Recursos a Utilizar](#recursos-a-utilizar)
  - [Lecturas Complementarias](#lecturas-complementarias)
  - [Marco Teórico](#marco-teórico)
    - [Memoria RAM](#memoria-ram)
    - [Imagen Forense](#imagen-forense)
  - [Escenario](#escenario)
  - [Entregables de laboratorio](#entregables-de-laboratorio)
    - [Archivos de evidencia](#archivos-de-evidencia)
    - [Informe Técnico Forense](#informe-técnico-forense)
  - [Procedimiento](#procedimiento)
    - [Análisis al detalle con RedLine](#análisis-al-detalle-con-redline)
    - [Reto 1 RedLine](#reto-1-redline)
    - [Live Response Automation](#live-response-automation)
      - [LiveResponse Cedarpelta](#liveresponse-cedarpelta)
      - [Wintriage](#wintriage)
    - [Obtener Archivos de una imagen forense](#obtener-archivos-de-una-imagen-forense)
    - [Análisis de la imagen forense](#análisis-de-la-imagen-forense)
      - [Carga de la Imagen en Autopsy](#carga-de-la-imagen-en-autopsy)
      - [Data Carving con Autopsy](#data-carving-con-autopsy)
    - [Análisis de Metadatos con FOCA](#análisis-de-metadatos-con-foca)
    - [Proceso de Sanitización](#proceso-de-sanitización)

## Objetivos

- Utilizar herramientas forenses especializadas para el levantamiento de evidencia.
- Realizar el proceso de levantamiento de copia forense sobre dispositivos de almacenamiento
- Analizar y almacenar los resultados del levantamiento.

## Recursos a Utilizar

Dentro del laboratorio se hará necesario la utilización de las siguientes herramientas y elementos:

- Máquina objeto de análisis. Puede ser su equipo físico o alguna máquina virtual Windows que usted desee analizar.
- Dispositivo de almacenamiento externo USB (se recomienda no superior a 8GB7)
- LiveResponse Cedarpelta
- Wintriage
- FTK Imager
- Volatility
  - Version 3
  - Versión 2.6
  - Workbench 2.1
  - Workbench 3
- Mandiant RedLine
- Autopsy
- DiskDigger

__NOTA:__ Recuerde que la información para descarga de cada una de las herramientas aquí listadas se encuentra en la página de [Herramientas](Herramientas.md)

## Lecturas Complementarias

UNIX / LINUX Forensics Analysis – Chris Progue – Capítulos 3, 6 y 7.

Windows Forensics Analysis – Harlan Carvey – Capitulo 1 [pag 1 – 62].

[FTK Imager User Guide](https://d1kpmuwb7gvu1i.cloudfront.net/Imager/4_7_1/FTKImager_UserGuide.pdf)

[Exterro Registry Viewer Manual](https://d1kpmuwb7gvu1i.cloudfront.net/RegistryViewer_UG.pdf)

## Marco Teórico

### Memoria RAM

La memoria de acceso aleatorio (Random Access Memory, RAM) se utiliza como memoria de trabajo de computadoras para el sistema operativo, los programas y la mayor parte del software. En la RAM se cargan todas las instrucciones que ejecuta la unidad central de procesamiento (procesador) y otras unidades del computador. Se denominan «de acceso aleatorio» porque se puede leer o escribir en una posición de memoria con un tiempo de espera igual para cualquier posición, no siendo necesario seguir un orden para acceder (acceso secuencial) a la información de la manera más rápida posible. Durante el encendido de la computadora, la rutina POST verifica que los módulos de RAM estén conectados de manera correcta. En el caso que no existan o no se detecten los módulos, la mayoría de las tarjetas madres emiten una serie de sonidos que indican la ausencia de memoria principal. Terminado ese proceso, la memoria BIOS puede realizar un test básico sobre la memoria RAM indicando fallos mayores en la misma.

### Imagen Forense

Cuando un equipo electrónico es identificado como sospechoso y contener posiblemente evidencia electrónica, es imperativo seguir un estricto conjunto de procedimientos para garantizar una extracción (admisible) de cualquier evidencia que pueda existir en el equipo sujeto. La primera cosa a recordar es la "regla de oro de la evidencia digital" -Nunca, de ninguna manera, modificar el medio original, si es posible-. Por lo tanto, antes de que ocurra cualquier análisis de datos, por lo general tiene sentido crear una copia exacta, flujo de bits de los medios de almacenamiento original que existe en el equipo sujeto. Una imagen forense, se refiere a veces una imagen ghost1 del medio de almacenamiento. Estas imágenes espejo o imagen fantasma no siempre generan una
imagen forense cierto. Lo mismo es cierto para la clonación de un disco duro. Una imagen forense puede incluir una o varias unidades de disco duro, medios de almacenamiento externo, CD (s), unidad (s) Zip o DVD (s), además de muchos otros tipos de medios de almacenamiento que existen en la actualidad. Es necesario que cuando hablamos de una imagen forense hacemos referencia a una copia bit a bit de todos los sectores del dispositivo de almacenamiento. La creación de una imagen forense es un proceso muy detallado. Si usted no tiene los adecuados conocimientos de un profesional capacitado, puede comprometer seriamente sus posibilidades de obtener pruebas admisibles como resultado de sus esfuerzos de descubrimiento. Además, para evitar acusaciones de adulteración de pruebas o expoliación, es una práctica recomendada que las imágenes sean realizadas por una tercera parte objetiva. Protocolos sugeridos para el levantamiento de una imagen forense se pueden encontrar dentro de las pautas estandarizadas por las instituciones y organizaciones como el Departamento de Justicia (DOJ) y el Instituto Nacional de Estándares y Tecnología (NIST). Como buen comienzo es siempre necesario asegurarse de que la integridad de todas las pruebas se mantiene, se establece la cadena de custodia, y todos los valores de hash pertinentes están documentados. Una vez que se completa la imagen, cualquier buena herramienta debe generar una huella digital de los medios de comunicación adquiridas, también conocido como un hash. Un proceso de generación de hash implica el examen de todos los 0s y 1s de que existen en los sectores examinados. La alteración de un solo 0 a 1 hará que el valor hash resultante sea diferente. Tanto la copia original y de las pruebas se analizan para generar una fuente y destino hash. Suponiendo que ambos coincidan, podemos estar seguros de la autenticidad de la unidad de disco duro copiado.

__Imagen Ghost:__ Una imagen del sistema, llamada también imagen Ghost o Ghost a causa de un programa bastante conocido, es una copia de respaldo de todo el contenido de una partición (incluso de un conjunto de particiones). Ninguna distinción es hecha en el contenido. Se puede decir que una imagen del sistema es una "copia fiel" de la partición en un instante t (siendo t la hora del respaldo).

El estándar de la industria para la imagen actualmente recomienda el uso del algoritmo MD5. El creador del MD5, Ronald L. Rivest del MIT, describe el algoritmo de la siguiente manera: [El algoritmo MD5] toma como entrada un mensaje de longitud arbitraria y produce como salida una "huella digital" de 128 bits o "compendio del mensaje" de la entrada. . . El algoritmo MD5 está destinado a aplicaciones de firma digital, donde un archivo de gran tamaño debe ser "comprimido" de una manera segura antes de ser cifrado con una clave privada (secreta) bajo un sistema de cifrado de clave pública como RSA.

La afirmación anterior simplemente dice que el MD5 es un excelente método para verificar la integridad de los datos. Un valor MD5 obtenida a partir de la imagen del disco duro debe coincidir con el valor del disco duro original. Incluso la más pequeña modificación en un disco duro, por ejemplo, la adición de una coma a un documento de MS Word sería enormemente cambiar el valor hash MD5 resultante.

Si bien puede parecer plausible para utilizar el personal de TI interno para mostrar una imagen de un disco duro sospechoso, tener en cuenta las posibles consecuencias. La contratación de expertos en informática forense de terceros asegurará un manejo seguro de las pruebas. Un experto cualificado seguirá estándares de la industria para evitar la expoliación y ayudará a refutar la acusación de sabotaje por parte de un miembro del personal interno que conozca la persona clave (s) conectada a la caja. Un experto de terceros también establecerá una cadena de custodia que garantice otra capa de protección a la evidencia.

## Escenario

A usted como investigador forense lo han llamado para realizar el levantamiento de información de un equipo Windows, que se cree que ha sido infectado con un malware y por el cuál se cree que atacantes remotos han iniciado la extracción de información confidencial de la compañía.

## Entregables de laboratorio

### Archivos de evidencia

Debe entregar los archivos de imagen obtenidos y los documentos de texto con la salida de cada uno de los comandos realizados en el equipo y de los resultados exportados con las herramientas adicionales.

Al finalizar la recolección completa de todas las evidencias, debe tomar cada uno de los archivos creados y les debe realizar el cálculo del hash con un algoritmo de SHA-256 y MD5. Se debe entregar otro archivo al final en donde se relacionen el nombre de los archivos y sus respectivos valores de hash.

__NOTA:__ si no se realiza la entrega del archivo de evidencias con sus hashes, el desarrollo del laboratorio no será válido

### Informe Técnico Forense

A usted como investigador forense no se le puede olvidar que debe realizar un informe en donde se detalle cada una de las acciones realizadas (paso a paso) en los equipos a fin de conservar las características de la evidencia vistas en clase.

Para realizar la entrega del informe se debe anexar un archivo de texto con el valor del hash del informe con un algoritmo de SHA-256 y MD5.

__NOTA:__ si no se realiza la entrega del archivo del informe técnico forense con su hash, el desarrollo del laboratorio no será válido

## Procedimiento

### Análisis al detalle con RedLine

Realizar el laboratorio descrito en la plataforma de entrenamiento usada en la clase anterior.

1. Ingrese a la plataforma de [Tryhackme](https://https://tryhackme.com/)
2. Busque el laboratorio denominado __REDLINE__
3. Active en el laboratorio haciendo clic en el botón de __"Join Room"__
4. Resuelva todas preguntas que plantea la plataforma.

__Nota:__ Recuerde realizar la documentación de todos los pasos que ejecutó para el análisis.

### Reto 1 RedLine

Realizar el laboratorio descrito en la plataforma de entrenamiento usada en la clase anterior.

1. Ingrese a la plataforma de [Tryhackme](https://https://tryhackme.com/)
2. Busque el laboratorio denominado __REvil Corp__
3. Active en el laboratorio haciendo clic en el botón de __"Join Room"__
4. Resuelva todas preguntas que plantea la plataforma.

### Live Response Automation

Con la ejecución del siguiente procedimiento, se espera realizar el levantamiento de información de manera automática de los elementos necesarios para una investigación forense, haciendo uso de las herramientas de LiveResponse Cedarpelta y Wintriage. El uso de herramientas automatizadas nos ayudan a ejecutar los procedimientos de manera más rápida y a evitar errores humanos en el levantamiento de la información.

#### LiveResponse Cedarpelta

1. Realice la descompresión del archivo LiveResponseCollection-Cedarpelta.zip en la ruta de trabajo.
2. En el interior de la carpeta se encontrará una serie de carpetas para la ejecución del levantamiento de información según el sistema operativo, para este caso del laboratorio, haremos uso de la carpeta de Windows.
3. En el interior de la carpeta, ejecuta el archivo exe que se encuentra en el interior. Nota: Recuerde ejecutar la herramienta con privilegios de administrador del equipo.
4. En la nueva ventana que se despliega, se debe seleccionar el tipo de levantamiento de información que se desea realizar:
   - Secure complete: realiza un levantamiento de la información del equipo que incluye la salida de comandos para la información volátil, una imagen de la memoria RAM y una imagen del disco duro
   - Memory Dump: Esta opción la información volátil por medio de comandos y una imagen de la memoria RAM
   - Triage: realiza un levantamiento de la información volátil por medio de comandos del sistema operativo y extracción de logs.
5. Para el caso del laboratorio, se realizará la selección de la opción de Triage, y luego se da clic en el botón de “Run…”, sin embargo, si usted desea realizar el levantamiento de la imagen de la memoria RAM y de algún medio de almacenamiento del equipo selecciones las opciones correspondientes.
__NOTA:__ Cuidado con seleccionar el disco duro base de su equipo, si se equivoca puede llevar el espacio total de su disco duro y producir falla en el equipo. recuerde que este procedimiento es bajo su responsabilidad.
6. Se abre una nueva ventana de la consola de comandos, es necesario esperar a que termine el procedimiento
7. Al finalizar el procedimiento, le indica que presione una tecla
8. En la carpeta de ejecución de la herramienta se crea una carpeta con todos los elementos extraídos
9. Verifique la carpeta llamada LiveResponseData
10. Analice la información obtenida y resalte los archivos

#### Wintriage

1. Realice la descompresión del archivo Wintriage en la ruta de trabajo.
2. Por favor lea el archivo que se llama "Leeme_primero_anda.txt"
3. Descargue y configure las aplicaciones auxiliares que le indica el archivo dentro de la carpeta tools.
4. Seleccione la opción del ejecutable que corresponda a su arquitectura de procesador
5. La primera vez, en la ventana de advertencia de Windows haga clic en la opción de mas información y luego en la opción de "Ejecutar de todas formas"
6. En la ventana de la aplicación seleccione la opción de "triage"
7. En la carpeta de ejecución de la herramienta se crea una carpeta con todos los elementos extraídos
8. Verifique la carpeta que tiene la parte inicial del nombre del equipo y la fecha (Ej.: VM-ALEJO20250429175815)
9. Analice la información obtenida y resalte los archivos
10. Si usted desea al finalizar valide las demás opciones que posee la herramienta

### Obtener Archivos de una imagen forense

Este procedimiento le permitirá extraer los archivos que se encuentran en una imagen forense, de acuerdo con el estado del elemento de origen, se puede llegar obtener la restauración de archivos que se encuentran borrados.

1. Ejecutar la herramienta de DiskDigger con privilegios de administrador
2. En la herramienta, hacer clic en la pestaña de avanzado y luego en el botón de “Escanear imagen de disco”
3. Seleccionar el archivo la copia de trabajo forense que se desea cargar y analizar
4. En la nueva ventana se selecciona la opción de “Cavar más profundo” y haga clic en el botón de siguiente
5. Luego haga clic en el botón de “seleccionar todos” para escanear todo tipo de archivos en la imagen
6. La aplicación inicia el análisis de la imagen, en la parte inferior se evidencia el avance de la carga de los archivos
7. Al finalizar el análisis total la herramienta indica la finalización
8. Se puede navegar en las pestañas de los diferentes tipos de archivos, una vez veamos un archivo de interés y que se desee extraer se da clic sobre el archivo y se selecciona la opción de “Guardar archivo como”
9. Se debe indicar la ruta en donde serán exportados los archivos
10. Seleccionar la opción de continuar sin registar
11. Como el archivo ha sido borrado y se pudieron perder algunos bytes, se debe ingresar un valor estimado del tamaño del archivo
12. Si el archivo es menor al tamaño estimado, aparecerá la siguiente advertencia, en donde se puede dar clic en el botón de “ignorar”
13. Aparece una nueva ventana con los archivos extraídos.

__Objetivo:__ obtenga un par de archivos recuperados de la imagen de la memoria USB obtenida en el laboratorio anterior. Debe mostrar que el archivo ha sido borrado de la memoria y se pudo recuperar con este procedimiento.

### Análisis de la imagen forense

#### Carga de la Imagen en Autopsy

1. Se realiza la ejecución de la herramienta Autopsy
2. En la ventana de ejecución se selecciona la opción de crear un nuevo caso
3. En la nueva ventana se ingresa el nombre del caso y la carpeta de trabajo del análisis de la imagen forense
4. En la siguiente ventana se ingresa el número del caso y el nombre del examinador de la imagen forense
5. Una vez creado el caso, es necesario adiciona el archivo de imagen forense a la herramienta. Para esta acción se selecciona la opción de “add data source”
6. Aparece una nueva ventana en donde se indica la ruta del archivo de la imagen, y se indica la zona horaria de operación, para este caso es la de Colombia
7. En la siguiente ventana se habilitan las opciones de análisis de la imagen, y habilitar el análisis del espacio sin asignar de la imagen.
8. Se inicia el proceso de carga e indexación de la imagen en la herramienta de Autopsy
9. Al finalizar la carga de la imagen se puede observar que se reconocen los diferentes volúmenes presentes en el dispositivo original

#### Data Carving con Autopsy

Con la ayuda de la herramienta de Autopsy, realizar búsquedas sobre la imagen forense que ha sido cargada en la herramienta y obtener los siguientes tipos de archivos:

- .doc
- .jpg
- .wav
- .xls
- .pdf
- .gif
- .mov
- .wmv
- .ppt
- .zip

De los resultados obtenidos en la búsqueda por medio de la herramienta obtener una copia del archivo y obtener los siguientes datos:

- Nombre del archivo
- Ruta de ubicación del archivo
- Hash MD5 y SHA256
- Ubicación del archivo en disco
- Identificar si es un archivo activo o fue borrado

__NOTAS:__

1. Para más información de cómo realizar búsquedas con Autopsy verificar el documento “Autopsy3_ReYDeS.pdf” que le ha sido entregado, página 24.
2. Recuerde que todo procedimiento realizado debe ser documentado completamente.

### Análisis de Metadatos con FOCA

Se debe realizar la instalación de la herramienta FOCA y luego realizar la creación de un proyecto de análisis, una vez hecho estos pasos seguir los siguientes pasos para cargar todos los documentos encontrados previamente en las imágenes forenses y obtener los metadatos de cada uno

1. Se realiza la ejecución de la herramienta FOCA, con el fin de verificar los metadatos del archivo, e identificar si hay más información oculta.
2. Sobre la ventana de la aplicación, en el menú de la izquierda se selecciona la opción de metadata->Documents
3. En la sección de la ventana del lado derecho, damos clic derecho y seleccionamos la opción de “Add File”
4. En la nueva ventana, ubicamos el archivo en la ruta del análisis que se está realizando y se selecciona la opción de abrir
5. Una vez cargado el archivo en la herramienta se selecciona y con clic derecho seleccionamos la opción de “Extract Metadata”
6. Luego de esta tarea, en el menú de la izquierda se ubica el tipo de archivo, y el nombre del archivo, en la sección de la derecha aparece indicado el contenido de los metadatos del archivo
7. Sobre la descripción de los metadatos se hace clic derecho y se selecciona la opción de “Export data to file”
8. En la ventana que aparece se indica la ruta y el nombre del archivo en donde se desea guardar esta información. Se decide guardar esta información ya que se observa un nombre de usuario que puede ser de utilidad.
9. Se verifica la creación de un nuevo archivo en la ruta de trabajo
10. Se abre el archivo para verificar el contenido

### Proceso de Sanitización

A continuación, se detalla el proceso de sanitización de medios de almacenamiento usados para el análisis y almacenamiento de la evidencia digital 

1. Se verifican los dispositivos USB instalados y reconocidos en el equipo
2. El administrador de dispositivos reconoce las unidades de almacenamiento
3. Se realiza la ejecución de la herramienta DiskWipe 1.7 para dar inicio al proceso de sanitización de las unidades de almacenamiento
4. Se ubica la ruta del ejecutable
5. Se realiza la ejecución de la herramienta con permisos de administrador
6. Se inicia la aplicación y se recolecta la información de los elementos de almacenamiento utilizados
7. Se muestra la información del medio de almacenamiento ubicado en la letra Que usted haya encontrado previamente
8. Sobre la herramienta de DiskWipe se inicia el proceso de borrado seguro del dispositivo de almacenamiento en la letra de la unidad. la primera opción es elegir el tipo de sistema de archivos que se desea utilizar en el medio de almacenamiento. Para este caso se selecciona NTFS.
9. En la siguiente ventana es necesario escoger el tipo de borrado seguro que se desea realizar en el dispositivo. Se seleccionó un tipo de borrado seguro de 7 pasadas, ya que el medio de almacenamiento no ha sido utilizado previamente.
10. Para iniciar el proceso, en la última ventana es necesario confirmar el borrado de los datos del medio de almacenamiento
11. Se observa el inicio del borrado seguro del dispositivo de almacenamiento. Es necesario esperar a que finalice la tarea de borrado seguro. La primera fase del borrado realiza el formateo de acuerdo al sistema de archivos elegido, para este caso NTFS
12. Finalizada la configuración del sistema de archivos, se da inicio al proceso de borrado seguro del medio de almacenamiento
13. Se realiza el borrado seguro de la unidad
14. Por medio de la herramienta de DiskWipe se verifica el contenido de la unidad de almacenamiento y se observa que todo ha sido clareado
