[Volver al Índice](./README.md)

# Introducción

Si tenemos VM o Contenedores LXC que utilizan mucho espacio en disco, un snapshot puede matar rápidamente el espacio disponible.

Por ésto, muchas veces será necesario revisar el listado de snapshots, y eliminar los innecesarios (nótese que eliminar ésto no eliminará la VM, solo el snapshot).

# Ver VMs que tienen snapshots

Con el siguiente comando, veremos el listado de snapshots: `qm list`. El output sería algo así:

| VMID | NAME     | STATUS  | MEM (MB) | BOOTDISK (GB) | PID |
|------|----------|---------|----------|----------------|-----|
| 102  | trueNAS  | stopped | 8192     | 20.00          | 0   |
| 103  | VM 103   | stopped | 4096     | 489.00         | 0   |

# Ver los snapshots

Sabemos qué VMs / contenedores tienen snapshots, ahora debemos listarlos:

```bash
qm listsnap 102
```

El output fue:
`-> current                                             You are here!`

Esto nos quiere decir que **no hay snapshots** en este contenedor, así que seguimos por la *VM ID 103*.

El output es similar a ésto:

| Nombre          | Fecha y Hora          | Descripción                                         |
|-----------------|-----------------------|-----------------------------------------------------|
| pre-gparted     | 2024-10-14 13:57:24   | Snapshot de la VM antes de hacer resize de la partición |

Quiero eliminar **pre-gparted**, para eso: `qm delsnap 103 pre-gparted`.

Nótese que hay que indicar el VM ID, y el nombre del snapshot. ¡Listo!