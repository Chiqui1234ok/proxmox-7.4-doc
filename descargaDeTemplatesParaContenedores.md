[Volver al Índice](./README.md)

# Descarga del template para nuestro contenedor

Antes que nada, debemos hacer un update al contenedor de *templates* con `pveam update`. Debería devolver el mensaje `update successful`.
Una vez hecho esto, podemos listar los templates disponibles, que en mi caso voy a filtrar el output del comando `pveam available` con un pipe **grep**, así:

```bash
pveam available | grep centos
```

En mi caso, busco CentOS 7, porque debo hacer una migración desde un VPS que utiliza este sistema operativo. La salida del comando `pveam available | grep centos` arroja el siguiente output:

```bash
pveam available | grep centos

system          centos-7-default_20190926_amd64.tar.xz
system          centos-8-default_20201210_amd64.tar.xz
system          centos-8-stream-default_20220327_amd64.tar.xz
system          centos-9-stream-default_20240828_amd64.tar.xz
```

Procedo a descargar el template con el comando:

```bash
pveam download templates centos-7-default_20190926_amd64.tar.xz
```

Nótese que **templates** es el dataset en el que se guardará el template. El mensaje al finalizar la descarga, será similar a éste:

```bash
download of 'http://download.proxmox.com/images/system/centos-7-default_20190926_amd64.tar.xz' to '/wd-1tb-3/templates/template/cache/centos-7-default_20190926_amd64.tar.xz' finished
```