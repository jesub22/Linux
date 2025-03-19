<br/>
<br/>
  </p>
<h1 align="center" font-size="3em">Commands 2 Work</h1>
Some concetps and commands to work on linux
<br/>
<br/>
  </p>

# Terminal
Interfaz, no grafica, de linea de comandos que permite entrada y salida de datos con comandos de texto. Estos comandos ejecutan tareas como instalar y desinstalar softwares, crear y eliminar directorios y ficheros, configuraciones del sistema.
  
# Directorio y Ficheros

Un directorio es una estructura que organiza ficheros (archivos) en una jerarquía. Cada archivo en el sistema está ubicado en un directorio específico, y los directorios pueden contener otros directorios (subdirectorios) y archivos. El directorio raíz del sistema se representa por una barra inclinada (/). 


En la línea de comandos, puedes navegar entre directorios utilizando comandos como "cd" (change directory) seguido del nombre del directorio al que deseas acceder. Por ejemplo, para cambiar al directorio /home/usuario, utilizarías el comando:


    cd /home/usuario/fichero.txt

Los directorios son una parte fundamental del sistema de archivos en Linux y son utilizados para organizar y estructurar los archivos de manera lógica y coherente.


# Sistemas de ficheros a nivel de formato

Los sistemas de archivos a nivel de formato se refieren a la manera en que los datos se organizan, almacenan y gestionan en dispositivos de almacenamiento como discos duros, unidades de estado sólido (SSD), tarjetas de memoria y otros medios de almacenamiento.


- ##### **EXT4:** ***Es el sistema de archivos por defecto en muchas distribuciones de Linux. Es una versión mejorada del sistema de archivos Ext3, con mejoras en el rendimiento y la capacidad de manejar volúmenes de almacenamiento de gran tamaño.*** ######
- ##### **EXT3:** ***Es una versión anterior de Ext4 y es compatible con muchas distribuciones de Linux. Aunque no es tan rápido como Ext4, es más estable y tiene soporte para diario (journaling) para la recuperación de datos en caso de un fallo del sistema.***
-  ##### **XFS (X File System):** ***Es un sistema de archivos de alto rendimiento que se utiliza comúnmente en entornos empresariales y de servidor. Ofrece características avanzadas como escalabilidad, administración de volumen integrada y una buena tolerancia a fallos.***
  - ##### **BTRSF (B-tree File System):** ***Es un sistema de archivos moderno que incluye características avanzadas como instantáneas (snapshots), compresión, deduplicación y administración de volumen integrada. Aunque es poderoso, aún se considera en desarrollo activo y puede no ser tan estable como otros sistemas de archivos.***




# Shell
Programa cuya función principal es la de interpretación de comandos, usando los recursos del sistema para lograr la acción deseada. El shell actúa como una interfaz entre el usuario y el núcleo del sistema operativo, permitiendo al usuario iniciar programas, gestionar archivos y directorios, configurar el sistema y realizar una variedad de tareas administrativas utilizando comandos de texto.

  - ###### Bash (Bourne Again SHell): *Es el shell predeterminado en la mayoría de las distribuciones de Linux. Es una versión mejorada del shell original de Unix, el Bourne Shell (sh), y ofrece características adicionales como completado de tabulación, historial de comandos, variables de entorno y control de flujos.*

  - ###### Zsh (Z Shell): *Es otro shell popular que ofrece características avanzadas como autocompletado mejorado, corrección ortográfica interactiva, y temas y complementos personalizables.*

  - ###### Fish (Friendly Interactive SHell): *Es un shell diseñado para ser más fácil de usar para usuarios principiantes, con características como autocompletado predictivo, sugerencias contextuales y una sintaxis más intuitiva.*

- ###### Dash: *Es un shell minimalista que se centra en la eficiencia y la velocidad de ejecución. Se utiliza a menudo como el shell de inicio rápido en sistemas Linux para reducir los tiempos de arranque.*

Cada shell tiene sus propias características y sintaxis, pero todos cumplen la misma función básica de permitir a los usuarios interactuar con el sistema operativo a través de comandos de texto. Los usuarios pueden elegir el shell que mejor se adapte a sus necesidades y preferencias personales.

 
# Prompt

"prompt" se refiere específicamente al indicador de línea de comandos que muestra el sistema operativo para indicar que está esperando una entrada del usuario. Por lo general, este prompt muestra información como el nombre de usuario, el nombre del sistema y la ruta del directorio actual.


    usuario@hostname:~$

Donde:

   - "usuario" es el nombre de usuario actual.
   - "hostname" es el nombre del sistema.
   - "~" es la abreviatura del directorio de inicio del usuario.
   - "$" es el símbolo del prompt, que indica que el sistema está listo para aceptar comandos del usuario.

El prompt puede personalizarse para mostrar información adicional o para adaptarse a las preferencias del usuario.

# Variables de Entorno

Una variable de entornos son valores dinamicos que controlan la forma en que se ejecuntan los procesos dentro del sistema. Estas variables son un mecanismo para almacenar información que puede ser usada por aplicaciones y scripts para adaptar su comportamiento según el entorno en el que se ejecutan. 

**Ejemplos de variables de entorno comunes**

- PATH: Contiene una lista de directorios en los cuales el sistema busca los ejecutables cuando se introduce un comando en la línea de comandos.
- HOME: Representa el directorio principal del usuario.
- USER: El nombre de usuario actual.
- SHELL: La ruta al intérprete de comandos predeterminado del usuario.
- LANG: Define la configuración regional y el idioma predeterminado del sistema.

Para ver las variables de entorno del sistema usaremos el comando 'env' o 'printenv', estos comandos nos enlistará las que haya configuradas. Podremos ver el valor de una variable especifica con el comando 'echo' seguido de un $ y el nombre de la variable en mayusculas.

    env

    echo $VARIABLE_NOMBRE

**Características de las Variables de Entorno**

  - Globalidad: Están disponibles para todos los procesos del sistema.
  - Persistencia: Pueden ser configuradas para persistir entre reinicios del sistema.
  - Seguridad: Permiten almacenar información sensible (como claves de API) fuera del código fuente.

**Defiinir una variable nueva**

    export VARIABLE_NOMBRE="valor"
     
**Eliminar Variable**

    unset $VARIABLE

**Ejemplos**

    Export ElGoaT="Messi"

    echo $ElGoAT
    Messi
