[Volver al Índice](./README.md)

# Introducción

Tuve que ejecutar un comando `dd`, para clonar todo el filesystem de un servidor viejo que usa CentOS 7, para poder montarlo en una VM dentro de Proxmox. Esto se hizo para dar de baja ese viejo servidor (que nos costaba plata), y montarlo en una VM dentro de una máquina ya existente en la oficina de la calle Libertad.

# Instalar la herramienta

Lo natural, sería ejecutar como `sudo` (spoiler: no lo hagas):

```bash
apt install qemu-utils
```

El problema es que ésto nos informa que algunos paquetes serán **removidos**:

```bash
root@pve:~# apt install qemu-utils
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages will be REMOVED:
libcrypt-openssl-bignum-perl libcrypt-openssl-rsa-perl libcrypt-ssleay-perl libio-socket-ssl-perl liblwp-protocol-https-perl libnet-dbus-perl libnet-ssleay-perl libproxmox-acme-perl libpve-access-control libpve-apiclient-perl libpve-cluster-api-perl libpve-cluster-perl libpve-common-perl libpve-guest-common-perl libpve-http-server-perl libpve-storage-perl librados2-perl libwww-perl libxml-parser-perl libxml-twig-perl proxmox-ve pve-cluster pve-container pve-firewall pve-ha-manager pve-manager pve-qemu-kvm qemu-server spiceterm
```

¡Y así es como rompemos Linux! Por no leer 😂 Evidentemente hay un conflicto de dependencias, por eso `apt` intenta resolver eliminándolas. Yo, usualmente, añado el parámetro `-y` al comando de `apt install`, pero de ahora en más no lo volveré a hacer, por lo menos en entornos corporativos. 
Moraleja: **siempre** leer los cambios que propone `apt` o cualquier software. No demos al botón *Siguiente* indiscriminadamente.

# Convertir imágenes a *qcow2*

Bueno, resulta que `qemu-img` ya está instalado en Proxmox, así que podemos convertir la imágen directamente.

1. Hagamos un `cd` hasta el directorio dónde está la imágen. Yo la moví a `/wd-1tb-3/iso_images`, así que:

```bash
cd /wd-1tb-3/iso_images
```

2. Un `ls` para poder ver el archivo en cuestión. Mi imágen se llama `vps.img`.
3. Ahora podemos proceder con la conversión:

```bash
qemu-img convert -p -f raw -O qcow2 vps.img mailserver-vps.qcow2
```

## Explicación de parámetros:

- `-p`: Muestra el progreso de la conversión.
- `-f raw`: Especifica que el formato de entrada (vps.img) es raw (se extrajo de un `dd`, así que es sin procesar).
- `-O qcow2`: Define que el formato de salida será *qcow2* (compatible con las máquinas virtuales de Proxmox, que utilizan `qemu`).
- `vps.img`: Es el archivo de origen en formato .img.
- `mailserver-vps.qcow2`: Es el nombre del archivo de salida en formato .qcow2.