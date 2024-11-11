[Volver al Índice](../README.md)

# Interfaz de red "bridge"

En la instalación, algo hice mal y tuve que re-armar mi archivo `/etc/network/interfaces`, por lo cuál en la interfaz de Proxmox no tengo ninguna interfaz de red, además de la física instalada en mi motherboard. Este paso puede que sea necesario, como no. Para saber si **no** necesitas este paso, ve a tu nodo (en mi caso, *pve*) y luego a *System* > ***Network***. Debería haber más de una interfaz de red. En mi caso tengo sólo una, así que procedo a crear una interfaz *bridge* para mis Linux Containers.

![Network del sistema](../README.src/system-network.jpg)

Bien, al darle al botón **Create**, procedemos a:
- Darle un nombre (que comience con *vmbr*, seguido de un número del 0 al 9999)
- Brindarle una IP IPv4, ejemplo 192.168.21.210/24

~~¡Listo!~~

**Recordá** que deberás reiniciar el servicio de *Networking* para aplicar cambios, con el comando `systemd restart networking`, o reiniciar el host. Caso contrario, la interfaz va a aparecer en la creación del contenedor y todo parecerá que está bien, pero el contenedor no podrá iniciar y nos costará unos minutos de Google, o un par de horas de ChatGPT 😣