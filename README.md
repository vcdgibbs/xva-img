XVA-IMG
=======

**xva-img** ya compilado usando **RockyLinux 9.5**

Es un binario ejecutable. dejar en /usr/local/bin

Fuente: https://github.com/eriklax/xva-img

Compilación
=======

Para compilarlo use lo siguiente:

    sudo dnf install qemu-img
    sudo dnf install cmake g++
    sudo dnf install openssl-devel
    sudo dnf --enablerepo=devel install xxhash-devel
    sudo dnf install git

ya con esto puedo bajar la fuente con:

    git clone https://github.com/eriklax/xva-img

y finalmente compilar:

    sudo cmake .
    sudo make
    sudo make install

Una vez compilado, make install lo va a sejar en /usr/local/bin

Conversión
=======

Permite convertir discos XVA que vienen de Xen-NG, o Citrix Xen-Server, a RAW.  Una vez en RAW ya se pueden convertir a QCOW2 usando

    qemu-img convert -f raw -O qcow2 vdisk.raw vdisk.qcow2

Entonces, la ruta sería:

    mkdir xva_uncompressed
    tar -xf vdisk.xva -C xva_uncompressed

Ojo con los permisos de los archivos del directorio Ref:4244 (o el número Ref:xxx, podría ser otro diferente), es muy posible que estén con 000, debe ejecutarse un:

    chmod 664 xva_uncompressed/*

Luego seguir:

    xva-img -p xva_uncompressed/Ref:4244/ vdisk.raw

El _:4244_ podría ser cualquier otro número, revísese primero.

Y finalmente llevarlo a qcow2:

    qemu-img convert -f raw -O qcow2 vdisk.raw vdisk.qcow2

Ahora ese QCOW2 se puede subir al servicio de imágenes de Nutanix y crear una VM a partir de ella.


    
