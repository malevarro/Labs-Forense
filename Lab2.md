# Laboratorio 2. In-Depth Response

- [Laboratorio 2. In-Depth Response](#laboratorio-2-in-depth-response)
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
    - [Levantamiento del registro de Windows](#levantamiento-del-registro-de-windows)
    - [Analizar registro de Windows](#analizar-registro-de-windows)
      - [Exterro Registry Viewer](#exterro-registry-viewer)
      - [Windows Registry Recovery](#windows-registry-recovery)
    - [Volcado de Memoria RAM](#volcado-de-memoria-ram)
      - [Exterro FTK Imager](#exterro-ftk-imager)
    - [Herramientas de análisis del archivo de memoria RAM](#herramientas-de-análisis-del-archivo-de-memoria-ram)
      - [Volatility Workbench](#volatility-workbench)
      - [Mandiant RedLine](#mandiant-redline)
      - [Analizando una memoria RAM](#analizando-una-memoria-ram)
        - [Lab 00](#lab-00)
        - [Lab 01](#lab-01)
        - [Análisis al detalle con RedLine](#análisis-al-detalle-con-redline)
      - [Análisis propio](#análisis-propio)

## Objetivos

- Identificar y utilizar herramientas dadas para sistemas operativos Windows
- Identificar las posibles evidencias volátiles, como no volátiles dentro de un ambiente de sistema operativo Windows.
- Utilizar herramientas forenses especializadas para el levantamiento de evidencia.
- Realizar el proceso de levantamiento de copia forense sobre dispositivos de almacenamiento
- Analizar y almacenar los resultados del levantamiento.

## Recursos a Utilizar

Dentro del laboratorio se hará necesario la utilización de las siguientes herramientas y elementos:

- Máquina objeto de análisis. Puede ser su equipo físico o alguna máquina virtual Windows que usted desee analizar.
- Dispositivo de almacenamiento externo USB (se recomienda no superior a 8GB7)
- FTK Imager
- Volatility
  - Version 3
  - Versión 2.6
  - Workbench 2.1
  - Workbench 3
- Mandiant RedLine
- Acelogix RegBak
- Exterro Registry Viewer
- Windows Registry Recovery WRR

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

Sobre la máquina de Windows a analizar, realice un levantamiento de evidencia volátil y no volátil. Tenga en cuenta que los datos y acciones que realice sobre la maquina serán reflejados en resultado de la ejecución de los comandos. Se seguirán los siguientes pasos:

- Levantamiento del registro de Windows.
- Levantamiento de una imagen de la memoria RAM.

### Levantamiento del registro de Windows

Para hacer una copia del registro se hace uso de la herramienta RegBak. Recuerde consultar la sección de [Recursos a Utilizar](#recursos-a-utilizar)

1. Se ejecuta el programa y se da en la opción de “New Backup”
2. En la siguiente ventana se especifica la ruta en donde se quiere exportar los archivos (Ej.: c:\temp), y en donde se puede indicar una descripción del Backup. Para iniciar el Backup se da click en el botón de “Start”
3. En la siguiente ventana se muestra el proceso de ejecución del Backup, una vez finalizado se debe dar click en el botón de “close”
4. Si todo esta correcto en la ruta especificada se encuentran los archivos de registro de la máquina

### Analizar registro de Windows

Para realizar el análisis del registro de Windows usaremos las herramientas de Exterro Registry Viewer y Windows Registry Recovery.Recuerde consultar la sección de [Recursos a Utilizar](#recursos-a-utilizar)

#### Exterro Registry Viewer

1. Descargue la herramienta y ejecute el proceso de instalación. Puede dejar las opciones por defecto.
2. Una vez instalada la herramienta, ejecútela entrando a la ventana principal. Cada vez que ingrese a la aplicación debe hacer clic en "No" y seguir ejecutando la aplicación en modo Demo.
3. En el menú file, cargamos en archivo mediante la opción “Open”.
4. Elegimos el archivo creado en el paso anterior desde el mismo campo de selección (Ej. NTUSER.dat, SYSTEM.dat)
5. Se despliega el listado de llaves de registro
6. Observer en cada uno de los archivos de registro las diferentes llaves o entradas. ¿Qué encuentra usted de interesante?

__Nota:__ Recuerde realizar la documentación de todos los pasos que ejecutó para el análisis.

#### Windows Registry Recovery

1. Descargue la aplicación. Descomprima y ejecute la versión del programa de acuerdo con su procesador.
2. En el menú file, cargamos en archivo mediante la opción “Open”.
3. Elegimos el archivo creado en el paso anterior desde el mismo campo de selección (Ej. NTUSER.dat, SYSTEM.dat)
4. Se despliega el listado de llaves de registro
5. Observer en cada uno de los archivos de registro las diferentes llaves o entradas. ¿Qué encuentra usted de interesante?

__Nota:__ Recuerde realizar la documentación de todos los pasos que ejecutó para el análisis.

__Pregunta:__ ¿Cuál aplicación considera usted más adecuada para realizar el análisis?

### Volcado de Memoria RAM

Al igual que los medios de almacenamiento como lo son discos duros, memorias USB, DVD/CD/Blu-ray y otros, la memoria RAM es un dispositivo de almacenamiento de información la cual tiende a ser volátil pero muy útil en casos de investigación. Dicha información hará referencia a la información que es accedida por programas durante su ejecución.

#### Exterro FTK Imager

Para realizar una copia (termino técnico utilizado es el volcamiento de memoria) se hará uso del programa Exterro FTK Imager.Recuerde consultar la sección de [Recursos a Utilizar](#recursos-a-utilizar)

1. Descargue la herramienta y ejecute el proceso de instalación. Puede dejar las opciones por defecto.
2. Ejecute el programa haciendo doble clic.
3. Ubique en la parte superior izquierda de la ventana la opción File, posterior hacer click sobre la opción Capture Memory. Luego, seleccione la ruta y el nombre del archivo en el que almacenara el volcado de memoria.Para efectos del laboratorio se recomienda que lo guarde como memdump.img. Seleccione la opción de Capture Memory. El tamaño de dicho archivo será directamente proporcional al tamaño de memoria RAM que posea la máquina. De ser requerido y como buena práctica, es recomendable seleccionar la parte de levantamiento del archivo de paginación.
4. Por último, es necesario que procese la información respectiva a la integridad del archivo para que sea tomada como evidencia.

### Herramientas de análisis del archivo de memoria RAM

Una vez generado el archivo de la memoria RAM, es posible cargar el archivo en alguna herramienta de análisis.

#### Volatility Workbench

Para poder analizar la información del volcado de la memoria RAM en un archivo es necesario hacer uso de la herramienta Volatility Workbench.

1. Descargue la herramienta y ejecute el proceso de instalación. Puede dejar las opciones por defecto.
2. Se ejecuta la herramienta y se despliega la ventana inicial de la aplicación.
3. Sobre la ventana se debe hacer clic cobre el botón “Browse Image” y allí se debe realizar la búsqueda del archivo de imagen que se obtuvo en procedimiento anterior
4. Una vez seleccionado el archivo se debe elegir el sistema operativo desde el cual se obtuvo la imagen, luego de ello se debe realizar clic en el botón de “Refresh Process List”.
5. El proceso se inicia y __se debe esperar a que finalice la tarea__
6. Al finalizar la tarea va a obtener el listado de todos los procesos encontrados en el equipo al momento de obtener la captura de la imagen
7. En el menú de command seleccionar las diferentes opciones que se listan y verificar la información que entrega

__Nota:__ Generar informe de las opciones que obtuvieron una salida exitosa

#### Mandiant RedLine

Otra herramienta que permite el análisis de la memoria RAM en un entrono gráfico es la herramienta de RedLine, producida y mantenida por Mandiant. Sobre esta herramienta se obtiene información similar a la herramienta anterior. Los pasos son los siguientes:

1. En un equipo de análisis forense se debe realiza la instalación de la herramienta con privilegios de administrador. Una vez instalada la aplicación ejecute la aplicación.
 __Nota:__ no olvide ejecutarla con privilegios de administrador.
2. En la nueva ventana de la aplicación se debe seleccionar la opción de Abrir de un archivo de imagen de memoria RAM.
3. En la siguiente ventana se debe seleccionar la ruta del archivo
4. En la siguiente ventana se debe indicar el nombre de la investigación y la ruta de trabajo en donde se dejara registro de la actividad de análisis.
5. Se debe esperar a que se cree todo el ambiente de análisis por parte de la herramienta
6. En la nueva ventana que se despliega, se debe seleccionar la opción de una fuente externa
7. Se despliega la ventana de análisis de la herramienta, ahora usted debe dirigirse a cada uno de los elementos que componen la herramienta y determinar que tipo de información se obtiene en cada una de las opciones o verificar para que nos sirve en una investigación forense.

#### Analizando una memoria RAM

En esta sección se indicará un archivo de una memoria que esta preparada para el análisis en un ambiente de laboratorio.

##### Lab 00

1. Ingrese a la parte inicial del reto [MemLabs Lab 0 - Never Too Late Mister](https://github.com/stuxnet999/MemLabs/tree/master/Lab%200)
2. Siga las instrucciones que allí se indican para realizar el análisis de la memoria.

##### Lab 01

1. Ingrese a la página del reto [MemLabs Lab 1 - Beginner's Luck](https://github.com/stuxnet999/MemLabs/tree/master/Lab%201)
2. Descargue el archivo indicado en la página para iniciar el reto.
3. Ingrese a la siguiente página para seguir las instrucciones. [MemLabs - Lab1](https://n1ght-w0lf.github.io/ctf%20writeups/memlabs-lab1/)
4. Siga las instrucciones que allí se indican para realizar el análisis de la memoria.

##### Análisis al detalle con RedLine

Realizar el laboratorio descrito en la plataforma de entrenamiento usada en la clase anterior.

1. Ingrese a la plataforma de [Tryhackme](https://https://tryhackme.com/)
2. Busque el laboratorio denominado __REDLINE__
3. Active en el laboratorio haciendo clic en el botón de __"Join Room"__
4. Resuelva todas preguntas que plantea la plataforma.

__Nota:__ Recuerde realizar la documentación de todos los pasos que ejecutó para el análisis.

#### Análisis propio

Teniendo en cuentas las herramientas adicionales que usted trabajo en los pasos anteriores

- Volatility Workbench
- RedLine

__Pregunta:__ ¿Es posible obtener los mismos resultados?. Justifique su respuesta y entregue evidencia que lo demuestre.
