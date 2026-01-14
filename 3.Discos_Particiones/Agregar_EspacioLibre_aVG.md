# addspaceVG
**Agregar espacio libre a un volume group**

Indentificar el disco con el espacio libre

    [root@sa-k8s-master-51 sa]# fdisk -l

    Disk /dev/sdb: 16 GiB, 17179869184 bytes, 33554432 sectors
    Disk model: Virtual disk    
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes 

Luego de indentificar el disco que en este caso son 16GB, debemos darle un volumen físico (Physical volume) a "/dev/sdb"
        
    [root@sa-k8s-master-51 sa]# pvcreate "/dev/sdb"
    Physical volume "/dev/sdb" successfully created.

identificamos el volume group que se encuentra creado*

    [root@sa-k8s-master-51 sa]# vgdisplay
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
En este caso el volume group es el "vg01"

    [root@sa-k8s-master-51 sa]# lsblk -f
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
La partición a usar será ***vg01-var***, también se puede ver el espación a reautilizar ***sdb*** 

Extender el vg01 junto con el volumen fisico nuevo que creamos anteriormente ***(/dev/sdb)***

    [root@sa-k8s-master-51 sa]# vgextend vg01 /dev/sdb
    Volume group "vg01" successfully extended
    [root@sa-k8s-master-51 sa]# vgs
    vgdisplay vg01
    VG   #PV #LV #SN Attr   VSize   VFree 
    vg01   2   9   0 wz--n- 214.49g 16.00g
    --- Volume group ---
    VG Name               vg01
    System ID             
    Format                lvm2
    Metadata Areas        2
    Metadata Sequence No  11
    VG Access             read/write
    VG Status             resizable
    MAX LV                0
    Cur LV                9
    Open LV               8
    Max PV                0
    Cur PV                2
    Act PV                2
    VG Size               214.49 GiB
    PE Size               4.00 MiB
    Total PE              54910
    Alloc PE / Size       50814 / 198.49 GiB
    Free  PE / Size       4096 / 16.00 GiB
    VG UUID               ZTXiPi-BgZg-daN3-g5cr-VRGI-dT3s-asghNp
El siguiente paso es asignarle el espacio al volumen de /var.

    [root@sa-k8s-master-51 sa]# lvresize -L+16G /dev/mapper/vg01-var
    Size of logical volume vg01/var changed from 112.49 GiB (28798 extents) to 128.49 GiB (32894 extents).
    Logical volume vg01/var successfully resized.
