[Volver al Índice](../README.md)

# Repositorio sin subscripción

Editar `/etc/apt/sources.list` como administrador, sobre-escribiendo su contenido por el siguiente.

```bash
deb http://ftp.debian.org/debian bookworm main contrib
deb http://ftp.debian.org/debian bookworm-updates main contrib

# Proxmox VE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription

# security updates
deb http://security.debian.org/debian-security bookworm-security main contrib
```

Luego, ejecutar `apt update`. Si sigue habiendo errores relacionados a "enterprise pve" o similar, seguramente es porque tenemos un archivo molestando en `/etc/apt/sources.list.d`. En mi caso, para eliminar esto realicé lo siguiente:

```bash
ls /etc/apt/sources.list.d
```

La consola me informó que tenía un archivo, llamado `pve-enterprise.list`. Así que sólo resta eliminarlo, con el comando:

```bash
rm sources.list.d/pve-enterprise.list
```

Así, podremos realizar instalaciones de apps, parches de seguridad, etc; de forma gratuita (comprometiendo un poco la estabilidad, aunque nosotros ya hemos revisado que este conjunto de paquetes funcione de forma estable).