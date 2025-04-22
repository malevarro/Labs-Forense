# LABORATORIO 1. INITIAL LIVE RESPONSE

- [LABORATORIO 1. INITIAL LIVE RESPONSE](#laboratorio-1-initial-live-response)
  - [Objetivos](#objetivos)
  - [Material a Utilizar](#material-a-utilizar)
  - [Marco Teórico](#marco-teórico)
    - [Comandos Nativos y MS-DOS](#comandos-nativos-y-ms-dos)
    - [Referencias a Nombre, Fecha y Hora del Sistema](#referencias-a-nombre-fecha-y-hora-del-sistema)
    - [Sesiones de Inicio](#sesiones-de-inicio)
    - [Marcas de Tiempo](#marcas-de-tiempo)
    - [Procesos en Ejecución](#procesos-en-ejecución)
    - [Interfaces de Red](#interfaces-de-red)
    - [Sockets y Aplicaciones](#sockets-y-aplicaciones)
    - [Conexiones de Red](#conexiones-de-red)
    - [Registro Cache DNS](#registro-cache-dns)
  - [Escenario](#escenario)
  - [Entregables del laboratorio](#entregables-del-laboratorio)
    - [Archivos de evidencia](#archivos-de-evidencia)
    - [Informe Técnico Forense](#informe-técnico-forense)
  - [Procedimientos](#procedimientos)
    - [Salida de comandos (MS-DOS / Powershell)](#salida-de-comandos-ms-dos--powershell)
    - [Información a recolectar](#información-a-recolectar)
    - [Sysinternals Suite](#sysinternals-suite)
      - [Características principales de Sysinternals Suite](#características-principales-de-sysinternals-suite)
      - [Herramientas destacadas](#herramientas-destacadas)
      - [Pasos para usar Sysinternals Suite](#pasos-para-usar-sysinternals-suite)
      - [Consejos adicionales Sysinternals Suite](#consejos-adicionales-sysinternals-suite)
    - [NirLauncher](#nirlauncher)
      - [Pasos para usar NirLauncher](#pasos-para-usar-nirlauncher)
      - [Consejos adicionales NirLauncher](#consejos-adicionales-nirlauncher)

## Objetivos

- Identificar las posibles evidencias volátiles dentro de un ambiente de sistema operativo Windows.
- Analizar y almacenar los resultados del levantamiento.
- Identificar los elementos importantes para la preservación y presentación de la evidencia.

## Material a Utilizar

Dentro del laboratorio se hará necesario la utilización de las siguientes herramientas y elementos:

- Comandos del sistema operativo Windows (Powershell o CMD)
- Sysinternals Suite
- Notepad++
- NirLauncher

## Marco Teórico

### Comandos Nativos y MS-DOS

MS-DOS (siglas de MicroSoft Disk Operating System, Sistema operativo de disco de Microsoft) es un sistema operativo para computadoras basado en x86. Fue el miembro más popular de la familia de sistemas operativos DOS de Microsoft, y el principal sistema para computadoras personales compatible con IBM PC en la década de 1980 y mediados de 1990, hasta que fue sustituida gradualmente por sistemas operativos que ofrecían una interfaz gráfica de usuario, en particular por varias generaciones de Microsoft Windows.
El sistema operativo Windows utiliza distintos intérpretes de comandos para procesar los comandos en el símbolo del sistema, como Command.com. Esto puede crear confusión cuando se utilizan los comandos nativos, o internos (por ejemplo, CHDIR, MKDIR, RMDIR etc.) en un símbolo del sistema.

### Referencias a Nombre, Fecha y Hora del Sistema

Uno de los principales datos en casos de investigación relacionados con equipos informáticos es la plena identificación de la maquina objeto de la investigación. Dentro de los sistemas operativos Windows, cada máquina tiene un identificador (nombre de maquina) que es proporcionado a la hora de la instalación de este.
Aun cuando este nombre pueda ser cambiado, tiene relación como un identificador de la máquina. Para ello, el sistema cuenta con los comandos hostname, date y time.

### Sesiones de Inicio

La mayoría de los sistemas Windows traen por defecto un inicio de sesión, donde por medio de un método de autenticación nivel 1 (algo que se sabe), es posible llegar acceder y manejar diferentes roles y responsabilidades.
Partiendo de este hecho, es posible llegar a que este sistema operativo refleje temas de seguridad como la autorización y autenticación para el acceso a diferentes recursos. Mediante el comando psloggedon.exe es posible llegar a determinar la sesión o cuenta de usuario en la cual se encuentra el sistema operativo funcionando.

### Marcas de Tiempo

Dentro de las características que tiene el almacenamiento digital, es posible llegar a obtener un registro de ciertas acciones que un usuario lleva a cabo al interactuar con los archivos. A estos se les llama MAC TIMES (o marcas de tiempo). Estas marcas de tiempo estarán directamente relacionadas con el tiempo configurado en la maquina donde los archivos son manipulados. Se llegan a tener 3 diferentes:

- Fecha y Hora de Modificación
- Fecha y Hora de Acceso
- Fecha y Hora de Creación

Estos llegan a ser supremamente importantes a la hora de establecer una línea de tiempo en un incidente.
Bastante bien se sabe que dichas fechas y horas no son la verdad absoluta porque un usuario puede llegar alterarlas con una manipulación incorrecta del sistema o simplemente con un cambio de hora y fecha del sistema.
Para tener un listado completo, es posible llegar a ejecutar el comando dir con alguna de las opciones para reflejar los resultados que se desean como se muestran a continuación:

- Fecha de modificación del archivo

```bat
dir /T:w /a /s /o:d “PATH o RUTA A ANALIZAR”
```

- Fecha de acceso (lectura, ejecución, impresión) al archivo

```bat
dir /T:a /a /s /o:d “PATH o RUTA A ANALIZAR”
```

- Fecha de creación del archivo

```bat
dir /T:c /a /s /o:d “PATH o RUTA A ANALIZAR”
```

Las diferentes opciones ofrecen un nivel de visualización de los resultados de tal manera que cada archivo analizado tendrá como información aparte de su nombre, fecha y hora; datos como el tamaño, tipo de archivo (si este tiende a ser un directorio o no) y ubicación.

### Procesos en Ejecución

Actualmente los sistemas operativos de Windows ofrecen un administrador de tareas en el cual el usuario puede llegar a visualizar los procesos que actualmente están en ejecución en la máquina. Sin embargo, programadas maliciosos como rootkits llegan a tender a ocultar información referente a ello. Mediante el comando pslist.exe es posible llegar a listar los procesos que actualmente están en ejecución en la máquina.

### Interfaces de Red

Una de las grandes entradas de información que poseen dichos sistemas operativos es realizado a través de las interfaces de red. Dentro de las redes, cada equipo tiene asignada una configuración determinada para el acceso al medio a través de IP (dirección de IP). Siendo así es necesario llegar a determinar la configuración de red que un equipo pueda tener en el momento de un incidente. Esta información puede llegar a cambiar en el momento en que sea desconectada o deshabilitada la interface de conexión. Mediante el comando ipconfig /all es posible llegar a visualizar la configuración que tiene cada una de las interfaces de red que posea la máquina.

### Sockets y Aplicaciones

Socket designa un concepto abstracto por el cual dos programas (posiblemente situados en computadoras distintas) pueden intercambiar cualquier flujo de datos, generalmente de manera fiable y ordenada. El término socket es también usado como el nombre de una interfaz de programación de aplicaciones (API) para la familia de protocolos de Internet TCP/IP, provista usualmente por el sistema operativo.

Los sockets de Internet constituyen el mecanismo para la entrega de paquetes de datos provenientes de la tarjeta de red a los procesos o hilos apropiados. Un socket queda definido por un par de direcciones IP local y remota, un protocolo de transporte y un par de números de puerto local y remoto. Mediante el comando fport.exe es posible llegar a visualizar los sockets que se encuentran en uso junto a que aplicación estará asociado.

### Conexiones de Red

De igual manera que las interfaces de red, es posible que a veces para un usuario sea transparente el concepto de las conexiones de red. Basándose en el modelo TCP/IP, un computador tiene 65535 puertos posible para establecer una conexión de red, por lo que necesitará una dirección IP configurada para realizar una conexión.
Aun así las conexiones que llegan a ser por protocolo TCP tienen a manejar un estado (listen, Established, Closing). Para listar las conexiones de red que tiene la máquina, es posible llegar a visualizarlas mediante el comando netstat con sus diferentes opciones.

### Registro Cache DNS

De igual manera que los servicios de red, uno de los principales utilizados en muchas conexiones referentes a red es el servicio DNS (Domain Name Service). Para que la maquina no tenga que realizar múltiples consultas sobre el mismo servicio requerido, el sistema operativo llega a mantener una memoria cache que almacena las consultas que han sido requeridas. Mediante el comando __ipconfig /displaydns__ es posible llegar a consultar las ultimas peticiones realizadas.

## Escenario

A usted como investigador forense lo han llamado para realizar el levantamiento de información de un equipo Windows, que se cree que ha sido infectado con un malware y por el cuál se cree que atacantes remotos han iniciado la extracción de información confidencial de la compañía.

## Entregables del laboratorio

### Archivos de evidencia

Debe entregar los documentos de texto con la salida de cada uno de los comandos realizados en el equipo y de los resultados exportados con las herramientas adicionales.
Al finalizar la recolección completa de todas las evidencias, debe tomar cada uno de los archivos creados y les debe realizar el calculo del hash con un algoritmo de SHA-256 y MD5. Se debe entregar otro archivo al final en donde se relacionen el nombre de los archivos y sus respectivos valores de hash.

__NOTA: si no se realiza la entrega del archivo de evidencias con sus hashes, el desarrollo del laboratorio no será válido.__

### Informe Técnico Forense

A usted como investigador forense no se le puede olvidar que debe realizar un informe en donde se detalle cada una de las acciones realizadas (paso a paso) en los equipos a fin de conservar las características de la evidencia vistas en clase.
Para realizar la entrega del informe se debe anexar un archivo de texto con el valor del hash del informe con un algoritmo de SHA-256 y MD5.

__NOTA: si no se realiza la entrega del archivo del informe técnico forense con su hash, el desarrollo del laboratorio no será válido.__

## Procedimientos

Sobre la máquina de Windows a analizar, realice un levantamiento de evidencia volátil con la ayuda de los elementos descritos a continuación:

__NOTA: Tenga en cuenta que los datos y acciones que realice sobre la maquina serán reflejados en resultado de la ejecución de los comandos.__

### Salida de comandos (MS-DOS / Powershell)

Debe realizar el levantamiento de información del equipo con la ayuda de los comandos de sistema operativo que ya vienen por defecto, no debe hacer uso de ningún software adicional. Con la ayuda de Internet investigue los comandos necesarios para obtener la información solicitada.

Debido a que la mayoría de los resultados son en texto, es posible llegar almacenar los resultados en archivos”. txt”. Con cada comando es necesario adicionar al final del comando el símbolo de mayor (>) seguido del nombre del archivo que quiere crear. En caso de querer concatenar resultados, deberá utilizar dos símbolos (>>) seguido del nombre del archivo al cual se quiere adicionar la información. Un ejemplo de ello sería:

```bat
echo "Para crear un archivo por comando"
comando > “nombre del archivo”txt
hostname > Logins.txt
```

```bat
echo "Para crear un archivo concatenando cada salida
comando >> “nombre del archivo”txt
hostname >> Logins.txt
echo ###################################### >> “Nombre de archivo”.txt
```

Si concatena la salida de los comandos, por favor incluir una línea que indique el cambio de comando con el siguiente comando:

```bat
echo ###################################### >> “Nombre de archivo”.txt
```

Si lo desea puede hacer uso de comandos Powershell de la misma manera para poder realizar el levantamiento de la información.

__Nota: Usted elige que lenguaje de comandos utilizar.__

### Información a recolectar

Usted debe investigar, encontrar y explicar los comandos de sistema operativo o herramientas de sistema que le permitan obtener y registrar los siguientes elementos del equipo a analizar:

- Nombre completo del equipo
- Fecha y hora del sistema, referenciando el uso horario
- Usuario con el que se está realizando el levantamiento de información
- Identificar Usuarios adicionales activos o creados en el sistema
- Registro de modificación, creación y tiempos de acceso de los archivos ubicados en las siguientes rutas
  - C:\Program Files
  - C:\Program Files (x86)
  - C:\Temp
  - C:\Users
  - Listado de puertos abiertos y de aplicaciones escuchando en dichos puertos.
  - Listado de las aplicaciones asociadas con los puertos abiertos
  - Tabla de procesos activos
  - Conexiones de red actual o reciente
  - Recursos compartidos
  - Tablas de rutas
  - Tabla de ARP
  - Cache de DNS
  - Verificar y registrar todos los sistemas de almacenamiento permanente que posee o que han sido conectados en el equipo alguna vez: Discos duros, disquetes, Memorias USB, cintas, y DVD – CD - RWs/ ROMs, etc.

### Sysinternals Suite

Sysinternals Suite es un conjunto de herramientas avanzadas para la administración, diagnóstico y solución de problemas en sistemas Windows. Desarrolladas por Mark Russinovich y Bryce Cogswell, estas utilidades son ampliamente utilizadas por profesionales de TI y desarrolladores para obtener información detallada sobre el funcionamiento interno de Windows.

#### Características principales de Sysinternals Suite

- Diagnóstico y Solución de Problemas: Incluye herramientas para monitorear, diagnosticar y solucionar problemas en sistemas Windows.
- Portabilidad: Las herramientas son portátiles y no requieren instalación, lo que permite ejecutarlas directamente desde una unidad USB.
- Actualizaciones Frecuentes: Las utilidades se actualizan regularmente para incluir nuevas funcionalidades y mejoras.

#### Herramientas destacadas

- Process Explorer: Proporciona información detallada sobre los procesos activos y permite identificar qué archivos y DLLs están abiertos o cargados.
- Process Monitor: Monitorea en tiempo real la actividad del sistema de archivos, el registro y los procesos.
- Autoruns: Muestra qué programas están configurados para ejecutarse automáticamente al inicio del sistema o al iniciar sesión.
- PsTools: Conjunto de herramientas de línea de comandos para la administración de sistemas, que incluye utilidades como PsExec, PsKill y PsList.
- Sysmon: Monitorea y registra la actividad del sistema para ayudar en la detección de intrusiones y análisis forense.

__Nota: el archivo llamado “sysinternals readme” le puede ayudar a determinar que herramientas puede usar.__

#### Pasos para usar Sysinternals Suite

1. __Descargar Sysinternals Suite:__
   - Vaya a la pagina de [Herramientas](Herramientas.md), y descargue Sysinternals Suite.

2. __Descomprimir el archivo:__
   - Descomprime el archivo ZIP en una carpeta de tu elección, por ejemplo, C:\SysinternalsSuite1.

3. __Ejecutar las herramientas:__
   - Abre la carpeta donde descomprimiste Sysinternals Suite y ejecuta las herramientas directamente desde allí. No requieren instalación.

#### Consejos adicionales Sysinternals Suite

- Sysinternals Live: Puedes ejecutar herramientas de Sysinternals directamente desde la web sin descargarlas manualmente. Usa la ruta <https://live.sysinternals.com/NOMBREHERRAMIENTA> en el Explorador de Windows.
- Documentación: Consulta la guía oficial de Sysinternals en Microsoft Learn para obtener información detallada sobre cada herramienta.

__OBJETIVO:__ Con la ayuda de la suite, usted debe realizar el levantamiento de información de los elementos indicados en el punto anterior, informando que programa utilizó para realizar el levantamiento de la información solicitada. Recuerde exportar los resultados e incluirlos en la evidencia.

### NirLauncher

NirLauncher es un paquete que incluye más de 200 utilidades portátiles gratuitas desarrolladas por NirSoft para Windows 1. Estas herramientas abarcan una amplia gama de funciones, como recuperación de contraseñas perdidas, monitoreo de red, visualización y extracción de cookies, caché y otra información almacenada por navegadores web, búsqueda de archivos en el sistema, y más.

Características principales de NirLauncher:

- Portabilidad: Puedes ejecutar NirLauncher desde una unidad USB sin necesidad de instalación.
- Seguridad: Todas las utilidades son completamente gratuitas y no contienen spyware, adware, malware ni software de terceros.
- Compatibilidad: Funciona en cualquier versión de Windows desde Windows 2000 hasta Windows 10, incluyendo sistemas x64.
- Configuración: La configuración de cada utilidad se guarda en un archivo .cfg en la unidad flash.
- Integración: Permite agregar más paquetes de software adicionales al paquete principal de NirSoft.

Utilidades destacadas:

- BlueScreenView: Analiza archivos de volcado de memoria y muestra información sobre los errores de pantalla azul.
- ChromePass: Recupera contraseñas almacenadas en Google Chrome.
- DNSQuerySniffer: Captura y muestra consultas DNS realizadas en el sistema.

Usar NirLauncher es bastante sencillo y permite acceder a una amplia gama de herramientas útiles para la administración y monitoreo de sistemas Windows. Aquí tienes una guía paso a paso para comenzar:

#### Pasos para usar NirLauncher

1. __Descargar NirLauncher:__
   - Vaya a la pagina de [Herramientas](Herramientas.md), y descargue la herramienta de NirLauncher.

2. __Descomprimir el archivo:__
   - Descomprime el archivo ZIP en una carpeta de tu elección, por ejemplo, `C:\NirLauncher`.
   - Dentro de la carpeta descomprimida, encontrarás una sub-carpeta llamada `NirSoft` que contiene todas las utilidades.

3. __Ejecutar NirLauncher:__
   - Abre la carpeta donde descomprimiste NirLauncher y ejecuta el archivo `NirLauncher.exe`.
   - Al abrir NirLauncher, verás una interfaz que lista todas las herramientas disponibles, organizadas por categorías.

4. __Navegar y ejecutar herramientas:__
   - Usa la interfaz de NirLauncher para navegar por las diferentes categorías de herramientas.
   - Cada herramienta tiene una breve descripción de su función. Haz clic en la herramienta que deseas usar para ejecutarla.

5. __Agregar paquetes adicionales:__
   - NirLauncher permite agregar colecciones de utilidades adicionales, como las herramientas de SysInternals.
   - Para agregar un nuevo paquete, descarga el archivo `.nlp` correspondiente y guárdalo en la carpeta de NirLauncher.
   - En NirLauncher, ve a `Launcher > Add Software Package` y selecciona el archivo `.nlp`.

#### Consejos adicionales NirLauncher

- __Antivirus__: Algunas herramientas de NirSoft pueden ser detectadas como amenazas por tu antivirus debido a su naturaleza de acceso profundo al sistema. Estas herramientas son seguras, pero es posible que necesites configurar tu antivirus para ignorar estas alertas.
- __Portabilidad__: Puedes copiar la carpeta de NirLauncher a una unidad USB y usar las herramientas en cualquier computadora sin necesidad de instalación.

__OBJETIVO:__ Con la ayuda de la suite, usted debe realizar el levantamiento de información de los elementos indicados en el punto anterior, informando que programa utilizó para realizar el levantamiento de la información solicitada. Recuerde exportar los resultados e incluirlos en la evidencia.
