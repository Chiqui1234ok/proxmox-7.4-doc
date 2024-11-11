[Volver al Índice](../README.md)

# Configuración de SWAP en la instalación
En la instalación, podemos establecer el disco dónde se instalará Proxmox, desde un menú desplegable. Al lado de ese menú, está el botón "Options", dónde debemos establecer un espacio SWAP de al menos 2GB, por si acaso (salvo que tengamos mucha memoria). En mi caso, al no tener tanta RAM y no saber bien para qué se utilizará este servidor de laboratorio, ni qué aplicaciones y fin tendrá (a lo mejor termina rackeado en algún lado 🤣), decidí asignar 3GB de memoria SWAP durante la instalación de Proxmox.

- SWAP: 3GB

> [!NOTE]  
> Más tarde configuraremos el valor *swappiness*, para indicarle a Proxmox que no utilice el swap, salvo que no le quede memoria RAM disponible. Así, se maximiza la performance de la memoria volátil y preservamos el SSD lo máximo posible.

# Modificar el uso de la memoria SWAP

Para modificar la agresividad en el uso del espacio SWAP, editamos `/etc/sysctl.conf` como administrador:

```bash
vim /etc/sysctl.conf
```

Y escribir:

```typescript
vm.swappiness = 0
```