<br/>
<br/>
  </p>
<h1 align="center" font-size="3em">Aumentar capacidad de un disco en LVM (Logical Volume Manager)</h1>
Montar o aumentar la capacidad de un disco en un servidor.
Es importante tener en cuenta que desde que se instaló y configuró el servidor debió definirse con un volumen lógico para poder realizar este procedimiento.


LVM Storage Management se divide en 3 partes:

 - Physical Volumes (PV) – son los discos (ejemplo: /dev/sda, /dev,sdb, /dev/vdb etc)
 -  Volume Groups (VG) – los volúmenes físicos (PV) se combinan en grupos de volúmenes (VG). (ejemplo: my_vg = /dev/sda + /dev/sdb.)
 - Logical Volumes (LV) – un grupo de volúmenes (VG) se divide en volúmenes lógicos (LV) (ejemplo: my_vg divided into my_vg/data, my_vg/backups, my_vg/home, my_vg/mysqldb)
<br/>
<br/>
  </p>

# Identificar el disco que necesitamos aumentar su capacidad

    df -h

*Nos dará un listado de los discos y donde estan montados.*
  
```bash
Filesystem                      Size  Used Avail Use% Mounted on
devtmpfs                        4.0M     0  4.0M   0% /dev
tmpfs                           3.8G     0  3.8G   0% /dev/shm
tmpfs                           1.6G  150M  1.4G  10% /run
/dev/mapper/vg01-root            40G  3.0G   38G   8% /
/dev/mapper/vg01-tmp            8.0G   90M  8.0G   2% /tmp
/dev/mapper/vg01-opt_temp       2.0G   47M  2.0G   3% /opt/temp
/dev/mapper/vg01-home           5.0G   69M  5.0G   2% /home
/dev/sda2                      1014M  329M  686M  33% /boot
/dev/mapper/vg01-var            113G  2.5G  127G   2% /var
/dev/sda1                       511M  7.1M  504M   2% /boot/efi
/dev/mapper/vg01-var_log         10G  186M  9.9G   2% /var/log
/dev/mapper/vg01-var_tmp        8.0G   90M  8.0G   2% /var/tmp
/dev/mapper/vg01-var_log_audit  5.0G   95M  4.9G   2% /var/log/audit
tmpfs                           769M     0  769M   0% /run/user/1000
```
 *Nos figuraría de esta forma; usarmemos como ejemplo* **/dev/mapper/vg01-var**

# Ver la lista de particiones de todos los dispositivos de almacenamiento conectados al sistema.

    fdisk -l

Este comando nos mostrará la información de los discos. El disco principal figura con el tamaño total mientras que las particiones se muestran debajo. En este caso se agregó un disco de 16GB, como se muestra a lo ultimo, para sumarlos al disco /var.

```
Disk /dev/sda: 200 GiB, 214748364800 bytes, 419430400 sectors
Disk model: Virtual disk    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: B7B6D8A2-FBFC-4320-9C43-590275DAE9A5

Device       Start       End   Sectors   Size Type
/dev/sda1     2048   1050623   1048576   512M EFI System
/dev/sda2  1050624   3147775   2097152     1G Linux filesystem
/dev/sda3  3147776 419428351 416280576 198.5G Linux LVM

Disk /dev/mapper/vg01-var_log_audit: 5 GiB, 5368709120 bytes, 10485760 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
------------------------------------------------------------------------------
Disk /dev/sdb: 16 GiB, 17179869184 bytes, 33554432 sectors
Disk model: Virtual disk    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

# Volume Group

Nuestro disco de 16GB aun no tiene formato legible para el sistema por lo cual deberemos realizar unos pasos para poder particionarlo y formatearlo para extender el LVM actual, por lo cual debemos identificar el  "Volume Group":

    vgdisplay

Este comando nos mostrará el o los volmue groups, en este caso solo figura uno, el cual fue creado al instalar el equipo.

```
--- Volume group ---
  VG Name               vg01
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  10
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                9
  Open LV               8
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <198.50 GiB
  PE Size               4.00 MiB
  Total PE              50815
  Alloc PE / Size       50814 / 198.49 GiB
  Free  PE / Size       1 / 4.00 MiB
  VG UUID               ZTXiPi-BgZg-daN3-g5cr-VRGI-dT3s-asghNp
```

El siguente paso es crear un "Pysical Volume" para que el volumen **/deb/sdb** sea legible para el sistema como unidad de almacenamiento para LVM.

    pvcreate /dev/sdb

Si fue creado correctamente nos devolverá este mensaje:

```
Physical volume "/dev/sdb" successfully created.
```
Agregaremos el PV **/dev/sdb** al "Volume Group" **vg01** para extender su espacio.  

    vgextend vg01 /dev/sdb

- **vg01**:  *Volumen destino a extender*    
- **/dev/sdb**:  *Volumen que se le dio physical volume para poder agregarlo al volumen destino*

Una vez creado con correctamente nos devolverá este mensaje:
  
```
Volume group "vg01" successfully extended
```

# Extender o Redimencionar Logical Volume 

En este paso usaremos un comando para extender nuestro lv con el espacio que se le asignó (16GB). El dispositivo montado al /var es el que será usado en este ejemplo, previamente se visualizó con el comando df -h como /dev/mapper/vg01-var (Filesystem).

     lvresize -L+16G /dev/mapper/vg01-var

una vez usado el comando satisfactoriamente, nos devolvería este mensaje:
```
Size of logical volume vg01/var changed from 112.49 GiB (28798 extents) to 128.49 GiB (32894 extents).
  Logical volume vg01/var successfully resized.
```
En este paso también podria haberse usado lvextend, pero en el ejemplo se uso lvresize. Debajo se dará una breve explicació de cada uno.

 **lvextend**

- Propósito: Incrementar el tamaño de un volumen lógico.
- Funcionalidad: Solo puede aumentar el tamaño de un volumen lógico. No tiene la capacidad de reducir el tamaño del volumen.
- Sintaxis:

      lvextend -L +<size> <logical_volume>

**lvresize**

- Propósito: Redimensionar el tamaño de un volumen lógico, ya sea para aumentarlo o reducirlo.
- Funcionalidad: Puede aumentar o reducir el tamaño de un volumen lógico, y también puede establecer un nuevo tamaño absoluto.
- Sintaxis:

  Para aumentar el tamaño:

      lvresize -L +<size> <logical_volume>

  Para reducir el tamaño:

      lvresize -L -<size> <logical_volume>

  Para establecer un nuevo tamaño absoluto:

      lvresize -L <size> <logical_volume>

  *La opción -L en los comandos lvextend y lvresize se utiliza para especificar el tamaño al que deseas ajustar el volumen lógico. Dependiendo de cómo se use, puede significar agregar, reducir o establecer un tamaño absoluto para el volumen lógico*

# Realizar la configuración para que el nuevo espacio sea reconocido por el Filesystem

Para avanzar en este ultimo paso, primero debemos identificar el tipo de sistema de archivos (FSTYPE) 


    lsblk -f
```  
NAME                   FSTYPE      FSVER    LABEL UUID                                   FSAVAIL FSUSE% MOUNTPOINTS
sda                                                                                                     
├─sda1                 vfat        FAT32          1864-6066                               503.9M     1% /boot/efi
├─sda2                 xfs                        c2bbf86b-76ed-47c9-9fb6-35b40da09018    685.2M    32% /boot
└─sda3                 LVM2_member LVM2 001       hw7pWR-fmFm-piPx-cB2v-MeFk-bR8O-EJs283                
  ├─vg01-root          xfs                        c114484f-f99e-4c63-bfda-11c7a34dad9b       37G     7% /
  ├─vg01-var_tmp       xfs                        be546246-c181-42ad-b36b-c9831aecfe12      7.9G     1% /var/tmp
  ├─vg01-var_log       xfs                        c79e84a2-67f3-4afd-8634-f1549010254f      9.8G     2% /var/log
  ├─vg01-swap          xfs                        67bc8981-b22c-4558-a2c1-cac74d6a71ad                  
  ├─vg01-home          xfs                        de8d5e1d-6e5f-44e1-a74f-ce0198d442f9      4.9G     1% /home
  ├─vg01-opt_temp      xfs                        5432102c-ee73-41a7-b589-d268b538ac18      1.9G     2% /opt/temp
  ├─vg01-tmp           xfs                        b286c45b-9698-4fc0-bfb4-9230922a22d9      7.9G     1% /tmp
  ├─vg01-var_log_audit xfs                        66b4e84e-8f17-4474-ad8e-882c78a011aa      4.9G     2% /var/log/audit
  └─vg01-var           xfs                        d8642e23-a178-4eaf-8de5-32379763d2c4    110.1G     2% /var
sdb                    LVM2_member LVM2 001       w0bm4c-gdHy-zahE-44Wy-Wvnx-xXmd-1mYxqS                
└─vg01-var             xfs                        d8642e23-a178-4eaf-8de5-32379763d2c4    110.1G     2% /var
```

Para redimensionar un sistema de archivos XFS, se deberá usar el comando:

    xfx_growfs /var
```
  meta-data=/dev/mapper/vg01-var   isize=512    agcount=4, agsize=7372288 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=1 inobtcount=1 nrext64=0
data     =                       bsize=4096   blocks=29489152, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=14399, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 29489152 to 33683456  
```
Una vez finalizado el proceso verificamos que nuestro disco se encuentre con el nuevo espacio que le asignamos.

    df -h
```    
Filesystem                      Size  Used Avail Use% Mounted on
devtmpfs                        4.0M     0  4.0M   0% /dev
tmpfs                           3.8G     0  3.8G   0% /dev/shm
tmpfs                           1.6G  150M  1.4G  10% /run
/dev/mapper/vg01-root            40G  3.0G   38G   8% /
/dev/mapper/vg01-tmp            8.0G   90M  8.0G   2% /tmp
/dev/mapper/vg01-opt_temp       2.0G   47M  2.0G   3% /opt/temp
/dev/mapper/vg01-home           5.0G   69M  5.0G   2% /home
/dev/sda2                      1014M  329M  686M  33% /boot
/dev/mapper/vg01-var            129G  2.5G  127G   2% /var
/dev/sda1                       511M  7.1M  504M   2% /boot/efi
/dev/mapper/vg01-var_log         10G  186M  9.9G   2% /var/log
/dev/mapper/vg01-var_tmp        8.0G   90M  8.0G   2% /var/tmp
/dev/mapper/vg01-var_log_audit  5.0G   95M  4.9G   2% /var/log/audit
tmpfs                           769M     0  769M   0% /run/user/1000
```
    
