<br/>
<br/>
  </p>
<h1 align="center" font-size="3em">Comando SCP</h1>
Comando SCP con sus parametro y sus posibles combinaciones
<br/>
<br/>
  </p>

# Sintaxis básica
 
    spc [opciones] [archivo] [usuario@servidor:ruta/destino]
 

# Parametros
# Opciones comunes del comando `tar`

`scp` (Secure Copy Protocol) es una herramienta de línea de comandos que permite copiar archivos de forma segura entre un host local y un host remoto o entre dos hosts remotos, utilizando el protocolo SSH para la transferencia y autenticación.

## ⚙️ Parámetros comunes

| Opción  | Descripción |
|---------|------------|
| `-r`    | Copia recursivamente directorios y archivos. |
| `-P <puerto>` | Especifica el puerto SSH a utilizar. |
| `-i <archivo>` | Usa un archivo de clave privada para autenticación. |
| `-C`    | Habilita la compresión de datos. |
| `-v`    | Modo detallado (verbose), muestra el progreso. |
| `-o <opción>` | Permite especificar opciones de SSH, como `-o StrictHostKeyChecking=no`. |

##  Ejemplos de uso

###  Copiar un archivo del local a un servidor remoto:
```sh
scp archivo.txt usuario@servidor:/ruta/destino/
```

###  Copiar un directorio completo de forma recursiva:
```sh
scp -r carpeta usuario@servidor:/ruta/destino/
```

###  Copiar un archivo desde un servidor remoto al equipo local:
```sh
scp usuario@servidor:/ruta/archivo.txt .
```

###  Copiar usando un puerto SSH específico:
```sh
scp -P 2222 archivo.txt usuario@servidor:/ruta/destino/
```

###  Copiar entre dos servidores remotos:
```sh
scp usuario1@servidor1:/ruta/archivo.txt usuario2@servidor2:/ruta/destino/
```

###  Usar autenticación con clave privada:
```sh
scp -i ~/.ssh/id_rsa archivo.txt usuario@servidor:/ruta/destino/
```

---

📌 **Nota:** Para que `scp` funcione, el servidor de destino debe tener habilitado el acceso SSH.


📖 Documentación oficial de `scp`: [OpenSSH scp](https://man.openbsd.org/scp)



