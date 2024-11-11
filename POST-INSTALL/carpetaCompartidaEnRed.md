# Introducción

Me bajé la imágen de TrueNAS, y no tenía forma fácil de pasarsela al servidor, así que voy a crear un "Samba share" (porque uso Windows en mi máquina de la oficina).
Aunque el impulso sería editar el archivo `/etc/samba/smb.conf`, las buenas prácticas pedirán pasos previos.

# Instalación de Samba

Raro, pero en mi caso `samba` no está instalado. Para ello, ejecutamos `apt`:

```bash
apt install samba
```

Revisá si `apt` borra algún paquete crítico con esa instrucción, pero no debería y si sucede deberías abortar. En mi caso, instalar `samba` no es problema, y ocupará unos 62MB en disco.

# Creación del volúmen

Es mejor crear un dataset para esta carpeta pública. Eso podemos hacerlo así, suponiendo que el volúmen es `wd-1tb-3` (más información en [discos, datasets y volúmenes](./discosDatasetsYVolumenes.md)):

```bash
zfs create wd-1tb-3/public
```

Bien, si realizamos `zfs list`, debería figurar nuestro nuevo dataset.

# Compartir el volúmen

Editamos `/etc/samba/smb.conf` y escribimos lo siguiente:

```bash
[Public]
path = /wd-1tb-3/public
browsable = yes
writable = yes
guest ok = yes
create mask = 0666
directory mask = 0777
force user = nobody
```

## Explicación:

- **path**: Especifica la ruta del directorio que deseas compartir.
- **browsable**: Permite que el recurso sea visible en la red.
- **writable**: Permite que los usuarios escriban en el recurso compartido.
- **guest ok**: Permite el acceso sin necesidad de autenticación.
- **create mask**: Define los permisos de los archivos creados (0666 significa que todos pueden leer y escribir).
- **directory mask**: Define los permisos de los directorios creados (0777 significa que todos pueden leer, escribir y acceder).
- **force user**: Establece el usuario que poseerá los archivos creados en el recurso compartido.

Sólo resta darle permisos a la carpeta, evitando la ejecución:

```bash
chmod 766 /wd-1tb-3/public
```

# Reiniciar Samba y aplicar cambios

Reiniciamos con:

```bash
systemctl restart samba
```

Samba leerá el `/etc/samba/smb.conf` al (re)iniciarse y deberíamos poder acceder a la carpeta compartida desde Windows, yendo a `\\ip.del.servidor.proxmox\Public`.