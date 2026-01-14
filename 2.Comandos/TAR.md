<br/>
<br/>
  </p>
<h1 align="center" font-size="3em">Comando TAR</h1>
Comando tar con sus parametro y sus posibles combinaciones
<br/>
<br/>
  </p>

# Sintaxis básica
 
    tar [opciones] -f archivo.tar [archivos o directorios]
 

# Parametros
# Opciones comunes del comando `tar`

El comando `tar` se utiliza en sistemas Unix y Linux para archivar y, opcionalmente, comprimir archivos y directorios. A continuación se presenta un cuadro con las opciones más comunes y una breve descripción de cada una.

| Opción   | Descripción                                                              |
|---------|--------------------------------------------------------------------------|
| `-c`    | Crear un nuevo archivo tar.                                                |
| `-x`    | Extraer archivos de un archivo existente.                                  |
| `-t`    | Listar el contenido del archivo.                                           |
| `-f`    | Especificar el nombre del archivo resultante.                              |
| `-v`    | Modo detallado (muestra los archivos procesados).                          |
| `-p`    | Preservar permisos y atributos de los archivos.                            |
| `-z`    | Comprimir usando gzip (`.tar.gz`).                                          |
| `-j`    | Comprimir usando bzip2 (`.tar.bz2`).                                        |
| `-J`    | Comprimir usando xz (`.tar.xz`).                                            |
| `-C`    | Cambiar el directorio antes de extraer o crear el archivo.                  |
| `--exclude` | Excluir archivos o directorios específicos al crear el archivo.       |

## Ejemplos de uso

### Crear un archivo tar sin compresión
```
tar -cvpf archivo.tar /ruta/carpeta
```

### Crear un archivo tar.gz con compresión gzip
```
tar -cvpzf archivo.tar.gz /ruta/carpeta
```

### Extraer un archivo tar.gz
```
tar -xvzf archivo.tar.gz
```

### Listar el contenido de un archivo tar.gz
```
tar -tvf archivo.tar.gz
```

### Excluir archivos o carpetas al crear el tar
```
tar -cvf archivo.tar /ruta/carpeta --exclude="/ruta/carpeta/privado"
```



