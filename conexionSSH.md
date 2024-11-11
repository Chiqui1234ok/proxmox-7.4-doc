[Volver al Índice](./README.md)

# Conexión SSH al host

Si precisamos conectarnos por SSH, simplemente ejecutamos el comando *ssh*, seguido del usuario root y la IP de nuestro servidor host.

```
ssh root@192.168.21.200
```

La contraseña será la misma que la utilizada para loguearnos en la interfaz web.

# Conexión SSH a un contenedor

Idealmente, accedemos con `pct enter 100` desde la interfaz web, porque si el contenedor no posee SSH hacia el mundo exterior, lo convierte en algo un poco más seguro. Sin embargo, yo por ejemplo necesitaba una conexión para poder editar archivos de configuración desde *Visual Studio Code*.

Por default, SSH Server no está instalado, así que procedemos a instalarlo con:
```bash
yum install openssh-server
```

Debemos editar la configuración con `vi /etc/ssh/sshd_config`, buscando la línea `PermitRootLogin no` y transformarlo a `PermitRootLogin yes` (y asegurarse que la línea no esté comentada). 
**No es recomendable, sino obligatorio, desinstalar *openssh-server* al terminar de realizar la configuración**:

```bash
yum remove openssh-server
```

O al menos bloquear el puerto 22, pero ésto es ligeramente más difícil y, posiblemente, no se requiera editar archivos de forma tan avanzada en el futuro (si hiciste bien tu trabajo o las demandas del negocio no cambiaron mucho 😜).

# Conexión SSH por VSCode

Para realizar esta conexión, y poder editar archivos con este programa, debemos hacer clic en este icono.

Insertamos los datos según nos pide VSCode, y si no llega a conectarse podemos probar:
- Ejecutar VSCode como administrador (ya que VSCode con el usuario `Usuario1` de Windows no tiene permiso para SSH)
- Que el archivo `.ssh/config` (en el usuario administrador) se vea algo así (con nombre de host, ip, usuario y puerto; en ese órden)
```bash
Host mailserver
  HostName 192.168.21.210
  User root
  Port 22
```