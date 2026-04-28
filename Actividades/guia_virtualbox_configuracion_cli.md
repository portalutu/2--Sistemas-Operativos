# Guía de estudio: VirtualBox, configuración de máquinas virtuales y uso por CLI

**Curso:** Sistemas Operativos / Redes / Soporte IT / Administración de Sistemas Operativos  
**Nivel:** Bachillerato Técnico-Tecnológico - DGETP-UTU  
**Modalidad:** Material de apoyo para laboratorio  
**Uso sugerido:** Consulta previa y durante prácticas de virtualización  

---

## 1. Objetivos de aprendizaje

Al finalizar el trabajo con este documento, se espera que el estudiante pueda:

1. Explicar qué es VirtualBox y para qué se utiliza.
2. Diferenciar los conceptos de equipo anfitrión, máquina virtual y sistema invitado.
3. Identificar las principales opciones de configuración de una VM.
4. Comprender cómo afectan la CPU, RAM, disco, red y video al funcionamiento de una máquina virtual.
5. Seleccionar configuraciones adecuadas según el tipo de sistema operativo a instalar.
6. Reconocer los modos de red disponibles en VirtualBox y sus usos más comunes.
7. Utilizar comandos básicos de `VBoxManage` para listar, crear, configurar, iniciar y detener máquinas virtuales.
8. Aplicar buenas prácticas para administrar laboratorios virtualizados.

---

## 2. Requisitos previos

Para aprovechar esta guía, se recomienda contar con:

- VirtualBox instalado en el equipo anfitrión.
- Conocimientos básicos de sistemas operativos.
- Conocimientos básicos de direcciones IP y redes.
- Manejo elemental de terminal o consola.
- Una imagen ISO de algún sistema operativo para prácticas, por ejemplo Ubuntu Server.
- Espacio libre en disco para crear máquinas virtuales.

---

## 3. Conceptos clave

### 3.1. ¿Qué es VirtualBox?

VirtualBox es una aplicación de virtualización que permite crear y ejecutar máquinas virtuales dentro de un equipo físico. Una máquina virtual se comporta como una computadora independiente, aunque en realidad utiliza recursos del equipo anfitrión.

VirtualBox es un **hipervisor de tipo 2**, también llamado **hipervisor alojado**. Esto significa que se instala como una aplicación sobre un sistema operativo anfitrión, por ejemplo Windows, Linux o macOS. A diferencia de un hipervisor de tipo 1, VirtualBox no se instala directamente sobre el hardware físico, sino que depende del sistema operativo del equipo donde se ejecuta.

Con VirtualBox se puede instalar un sistema operativo completo dentro de una ventana o en modo sin interfaz gráfica. Por ejemplo, se puede ejecutar Ubuntu Server dentro de una computadora con Windows, Linux o macOS.

> **Recordatorio: tipos de hipervisores**
>
> | Tipo de hipervisor | También llamado | Cómo funciona | Uso común | Ejemplos |
> |---|---|---|---|---|
> | **Tipo 1** | Bare-metal o nativo | Se instala directamente sobre el hardware físico, sin depender de un sistema operativo anfitrión tradicional. | Centros de datos, servidores de producción, virtualización empresarial. | VMware ESXi, Microsoft Hyper-V Server, Proxmox VE, Xen. |
> | **Tipo 2** | Alojado o hosted | Se instala como una aplicación dentro de un sistema operativo ya existente. | Laboratorios, educación, pruebas, desarrollo, uso personal. | VirtualBox, VMware Workstation, VMware Fusion, Parallels Desktop. |
>
> En términos simples: un hipervisor de **tipo 1** se usa cuando se quiere dedicar un equipo físico a ejecutar máquinas virtuales de forma profesional o productiva. Un hipervisor de **tipo 2** se usa cuando se quiere virtualizar desde una computadora de uso general, sin reemplazar el sistema operativo principal.

VirtualBox se utiliza para:

- Aprender sistemas operativos sin modificar el equipo físico.
- Probar configuraciones de red.
- Practicar instalación de servidores.
- Simular laboratorios de soporte IT.
- Probar software en entornos aislados.
- Crear entornos de desarrollo.
- Ejecutar sistemas operativos antiguos o alternativos.

### 3.2. ¿Qué es virtualización?

La virtualización es una técnica que permite simular recursos de hardware mediante software. En lugar de instalar un sistema operativo directamente sobre una computadora física, se instala sobre una máquina virtual.

La virtualización permite que un mismo equipo físico ejecute varios sistemas operativos al mismo tiempo, cada uno dentro de su propia VM.

### 3.3. Equipo anfitrión, sistema invitado y máquina virtual

| Concepto | Explicación | Ejemplo |
|---|---|---|
| **Host / anfitrión** | Equipo físico donde se instala VirtualBox. | Una notebook con Windows 11. |
| **Guest / invitado** | Sistema operativo instalado dentro de la VM. | Ubuntu Server instalado en VirtualBox. |
| **VM / máquina virtual** | Computadora simulada por software. | `Ubuntu-Server-Lab`. |
| **Hipervisor** | Software que permite ejecutar máquinas virtuales. | VirtualBox. |
| **ISO** | Archivo que contiene un medio de instalación. | `ubuntu-24.04-live-server-amd64.iso`. |

### 3.4. ¿Por qué usar VirtualBox en clase?

VirtualBox es adecuado para prácticas educativas porque permite experimentar sin poner en riesgo el sistema operativo real del equipo. Si una VM se rompe, se puede borrar, restaurar o reinstalar.

En el aula permite trabajar con escenarios similares a los reales:

- Instalación de sistemas operativos.
- Configuración de red.
- Administración de servidores.
- Pruebas con direcciones IP.
- Instalación de servicios.
- Uso de snapshots.
- Trabajo con múltiples equipos virtuales conectados entre sí.

---

## 4. Estructura general de VirtualBox

VirtualBox tiene dos formas principales de uso:

1. **Interfaz gráfica:** permite crear y administrar máquinas virtuales mediante ventanas, botones y menús.
2. **Interfaz de línea de comandos:** permite administrar VirtualBox desde terminal usando `VBoxManage`.

Ambas formas trabajan sobre el mismo sistema de máquinas virtuales. Una VM creada desde la interfaz gráfica también puede administrarse por CLI, y una VM creada por CLI aparece luego en la interfaz gráfica.

---

## 5. Configuración general de una máquina virtual

Al crear o modificar una VM, VirtualBox permite configurar distintos componentes virtuales. Estas opciones representan el hardware y las funcionalidades que tendrá la máquina virtual.

### 5.1. Nombre de la máquina virtual

El nombre identifica la VM dentro de VirtualBox.

Ejemplo:

```text
Ubuntu-Server-Lab
Windows-Cliente-01
Debian-Router-Test
```

Recomendaciones:

- Usar nombres claros.
- Evitar tildes y caracteres especiales.
- Incluir el sistema operativo o propósito.
- Evitar nombres genéricos como `VM1` o `Prueba`.

### 5.2. Carpeta de la máquina

Es la ubicación donde VirtualBox guardará los archivos de la VM.

Normalmente incluye:

- Archivo de configuración de la VM.
- Disco virtual.
- Logs.
- Snapshots.
- Archivos temporales.

Recomendaciones:

- Usar una carpeta con suficiente espacio libre.
- No guardar VMs en carpetas sincronizadas con servicios cloud si son muy grandes.
- Evitar mover manualmente archivos de la VM sin usar las herramientas de VirtualBox.

### 5.3. Tipo y versión del sistema operativo

VirtualBox solicita seleccionar el **tipo** de sistema operativo y su **versión aproximada**. Esta opción no instala el sistema operativo automáticamente por sí sola; lo que hace es cargar una plantilla de configuración inicial adecuada para ese sistema invitado.

Esta selección ayuda a VirtualBox a sugerir configuraciones iniciales compatibles, como:

- Memoria RAM recomendada.
- Tipo de firmware.
- Chipset.
- Controlador gráfico.
- Tipo de disco.
- Controladores de almacenamiento.
- Opciones de virtualización.
- Identificador usado por `VBoxManage` mediante `--ostype`.

> **Importante:** la lista exacta de sistemas disponibles puede variar según la versión de VirtualBox, la arquitectura del equipo anfitrión y las extensiones instaladas. Para ver la lista exacta en un equipo concreto, se debe ejecutar:
>
> ```bash
> VBoxManage list ostypes
> ```
>
> Ese comando muestra todos los sistemas operativos invitados conocidos por la instalación local de VirtualBox, junto con su identificador técnico para uso por CLI.

### Sistemas operativos reconocidos por VirtualBox de forma predeterminada

La siguiente tabla resume las familias y versiones de sistemas operativos que VirtualBox suele reconocer **out-of-the-box**, es decir, sin necesidad de crear una plantilla manual desde cero.

| Familia / tipo | Versiones o variantes disponibles habitualmente | Uso típico en laboratorio |
|---|---|---|
| **Other / Unknown** | Other, Other/Unknown 32-bit, Other/Unknown 64-bit | Sistemas no listados, pruebas genéricas o sistemas antiguos. |
| **DOS** | DOS, MS-DOS, FreeDOS y variantes compatibles | Prácticas históricas, software legado, arranque básico. |
| **Microsoft Windows antiguo** | Windows 3.1, Windows 95, Windows 98, Windows ME, Windows NT 4.0, Windows 2000 | Compatibilidad con software antiguo y estudio histórico. |
| **Microsoft Windows XP / 2003** | Windows XP 32-bit, Windows XP 64-bit, Windows Server 2003 32-bit, Windows Server 2003 64-bit | Laboratorios de sistemas heredados. |
| **Microsoft Windows Vista / 2008** | Windows Vista 32-bit, Windows Vista 64-bit, Windows Server 2008 32-bit, Windows Server 2008 64-bit | Pruebas de compatibilidad y sistemas empresariales antiguos. |
| **Microsoft Windows 7 / 2008 R2** | Windows 7 32-bit, Windows 7 64-bit, Windows Server 2008 R2 64-bit | Laboratorios de escritorio y administración Windows clásica. |
| **Microsoft Windows 8 / 8.1 / 2012** | Windows 8 32-bit, Windows 8 64-bit, Windows 8.1 32-bit, Windows 8.1 64-bit, Windows Server 2012 64-bit, Windows Server 2012 R2 64-bit | Prácticas con sistemas de transición hacia UEFI y entornos modernos. |
| **Microsoft Windows 10 / 2016 / 2019** | Windows 10 32-bit, Windows 10 64-bit, Windows Server 2016 64-bit, Windows Server 2019 64-bit | Escritorio moderno, administración básica, pruebas de aplicaciones. |
| **Microsoft Windows 11 / Server moderno** | Windows 11 64-bit, Windows Server 2022 64-bit y, según versión de VirtualBox, opciones más recientes | Laboratorios modernos, pruebas de compatibilidad y administración. |
| **Linux genérico** | Linux 2.2, Linux 2.4, Linux 2.6, Linux 3.x, Linux 4.x, Linux 5.x, Linux 6.x, versiones 32-bit y 64-bit según corresponda | Distribuciones no listadas específicamente o kernels personalizados. |
| **Ubuntu** | Ubuntu 32-bit, Ubuntu 64-bit | Instalación de Ubuntu Desktop o Ubuntu Server. Es una de las opciones más usadas en clase. |
| **Debian** | Debian 32-bit, Debian 64-bit | Servidores, administración Linux, base conceptual de muchas distribuciones. |
| **Fedora** | Fedora 32-bit, Fedora 64-bit | Pruebas con tecnologías Linux recientes. |
| **Red Hat / RHEL** | Red Hat 32-bit, Red Hat 64-bit, Red Hat Enterprise Linux 5, 6, 7, 8, 9 y versiones recientes según VirtualBox | Laboratorios empresariales Linux. |
| **Oracle Linux** | Oracle Linux 32-bit, Oracle Linux 64-bit, Oracle Linux 5, 6, 7, 8, 9 y versiones recientes según VirtualBox | Laboratorios empresariales, servidores y ecosistema Oracle. |
| **SUSE / openSUSE** | SUSE, openSUSE, variantes 32-bit y 64-bit | Administración Linux en distribuciones RPM. |
| **Arch Linux** | Arch Linux 64-bit | Laboratorios avanzados, instalación manual y comprensión profunda de Linux. |
| **Gentoo** | Gentoo 32-bit, Gentoo 64-bit | Laboratorios avanzados, compilación y personalización del sistema. |
| **Mandriva / Mandrake** | Mandriva 32-bit, Mandriva 64-bit | Sistemas Linux históricos o heredados. |
| **Turbolinux** | Turbolinux 32-bit, Turbolinux 64-bit | Compatibilidad histórica. |
| **Xandros** | Xandros 32-bit, Xandros 64-bit | Compatibilidad histórica. |
| **BSD** | FreeBSD 32-bit, FreeBSD 64-bit, OpenBSD 32-bit, OpenBSD 64-bit, NetBSD 32-bit, NetBSD 64-bit, DragonFly BSD 64-bit | Laboratorios de sistemas tipo Unix no Linux, redes y seguridad. |
| **Solaris / OpenSolaris** | Oracle Solaris 10, Oracle Solaris 11, OpenSolaris, variantes 32-bit y 64-bit según disponibilidad | Sistemas Unix empresariales, administración avanzada. |
| **IBM OS/2** | OS/2 Warp 3, OS/2 Warp 4, OS/2 Warp 4.5, eComStation, ArcaOS, variantes compatibles | Estudio de sistemas históricos y software legado. |
| **macOS / Mac OS X** | Mac OS X y macOS en versiones Intel compatibles, según host y versión de VirtualBox | Pruebas específicas, con restricciones técnicas y legales. |
| **NetWare** | Novell NetWare y variantes compatibles | Redes históricas y servicios de archivo heredados. |
| **QNX** | QNX y variantes compatibles | Sistemas embebidos, tiempo real y usos especializados. |
| **Haiku / BeOS compatibles** | Haiku, BeOS o variantes reconocidas según versión | Sistemas alternativos, estudio de diseño de sistemas operativos. |
| **L4 / sistemas experimentales** | L4 y otros sistemas de investigación o experimentales | Investigación, pruebas de sistemas operativos y casos avanzados. |
| **JRockitVE / appliances especializados** | JRockit Virtual Edition y otros entornos especializados reconocidos por versiones anteriores | Casos empresariales o históricos muy específicos. |

### Aclaración importante sobre “soportado”

Que VirtualBox muestre una opción en la lista de sistemas operativos no significa que el sistema esté instalado, licenciado o garantizado para funcionar perfectamente en cualquier equipo. Significa que VirtualBox conoce ese tipo de sistema y puede aplicar una configuración inicial razonable.

Por ejemplo:

- Si se elige **Ubuntu 64-bit**, VirtualBox ajusta la VM pensando en una distribución Linux moderna de 64 bits.
- Si se elige **Windows 11 64-bit**, VirtualBox puede activar o sugerir opciones más adecuadas para un Windows moderno.
- Si se elige **Other/Unknown**, VirtualBox usará una configuración más genérica.

### Identificadores usados por CLI

Desde la línea de comandos, estas opciones se usan con el parámetro `--ostype`.

Ejemplos:

```bash
VBoxManage createvm --name "Ubuntu-Lab" --ostype Ubuntu_64 --register
VBoxManage createvm --name "Debian-Lab" --ostype Debian_64 --register
VBoxManage createvm --name "Windows11-Lab" --ostype Windows11_64 --register
VBoxManage createvm --name "FreeBSD-Lab" --ostype FreeBSD_64 --register
```

Para evitar errores, no conviene adivinar el identificador. Primero se debe consultar la lista local:

```bash
VBoxManage list ostypes
```

Luego se copia exactamente el valor que aparece en el campo `ID`.

---

## 6. Configuración de sistema

La sección **Sistema** define recursos esenciales de la VM: memoria RAM, orden de arranque, CPU, firmware y funciones de virtualización.

### 6.1. Placa base

La pestaña de placa base permite configurar elementos básicos de hardware virtual.

#### Memoria base

Define cuánta RAM del equipo anfitrión se asignará a la VM.

Ejemplo:

| Sistema invitado | RAM mínima sugerida | RAM recomendada |
|---|---:|---:|
| Ubuntu Server | 1024 MB | 2048 MB o más |
| Ubuntu Desktop | 4096 MB | 6144 MB o más |
| Windows 10/11 | 4096 MB | 8192 MB o más |
| Router/firewall Linux básico | 512 MB | 1024 MB |

Recomendaciones:

- No asignar toda la RAM del host.
- Dejar memoria suficiente para el sistema anfitrión.
- Para servidores sin entorno gráfico, 2 GB suele ser suficiente para prácticas iniciales.
- Para escritorios gráficos, se necesita más memoria.

#### Orden de arranque

Define desde qué dispositivo intentará arrancar la VM.

Opciones frecuentes:

- Disco duro.
- Unidad óptica virtual.
- Red.
- Disquete, en sistemas antiguos.

Durante una instalación desde ISO, conviene que la unidad óptica esté antes que el disco duro. Luego de instalar, el disco debe quedar primero.

#### Chipset

Define el tipo de chipset virtual que usará la VM. En la mayoría de los casos se puede dejar el valor predeterminado.

Opciones comunes:

- **PIIX3:** opción clásica y compatible.
- **ICH9:** opción más moderna, útil para algunos sistemas operativos recientes o configuraciones avanzadas.

Para prácticas comunes de Ubuntu Server, normalmente no es necesario modificar esta opción.

#### Dispositivo apuntador

Define el tipo de mouse o dispositivo de entrada virtual.

Opciones comunes:

- PS/2 Mouse.
- USB Tablet.
- USB Multi-Touch Tablet.

En sistemas con interfaz gráfica, USB Tablet suele mejorar la integración del mouse. En servidores sin interfaz gráfica, esta opción tiene poca importancia.

#### Habilitar I/O APIC

Permite mejor soporte para sistemas multiprocesador y sistemas operativos modernos. Debe quedar activado en la mayoría de las VMs actuales.

#### Habilitar EFI

Permite arrancar la VM usando firmware UEFI en lugar de BIOS tradicional.

Conviene usar EFI cuando:

- El sistema operativo lo requiere.
- Se desea simular una instalación moderna con UEFI.
- Se trabaja con particiones GPT y arranque UEFI.

En prácticas iniciales con Ubuntu Server, se puede usar BIOS tradicional salvo que el docente indique lo contrario.

### 6.2. Procesador

Esta sección define cuántos núcleos de CPU se asignarán a la VM.

#### Cantidad de procesadores

Ejemplo:

| Uso de la VM | CPU sugerida |
|---|---:|
| Ubuntu Server básico | 1 o 2 CPU |
| Ubuntu Desktop | 2 CPU o más |
| Windows moderno | 2 CPU o más |
| Laboratorio liviano | 1 CPU |
| Servicios más exigentes | 2 a 4 CPU |

Recomendaciones:

- No asignar todos los núcleos del host.
- Para clase, 1 o 2 CPU suelen ser suficientes.
- Más CPU no siempre significa mejor rendimiento si el host queda sin recursos.

#### Límite de ejecución

Permite limitar el porcentaje de CPU que puede usar la VM. Normalmente se deja en 100%.

Puede ser útil si se desea evitar que una VM consuma demasiados recursos del equipo anfitrión.

#### PAE/NX

PAE permite que algunos sistemas operativos de 32 bits accedan a más memoria. NX mejora seguridad al marcar zonas de memoria como no ejecutables.

En sistemas modernos de 64 bits, normalmente no requiere intervención manual.

### 6.3. Aceleración

VirtualBox utiliza funciones de virtualización asistida por hardware, como Intel VT-x o AMD-V.

Estas funciones permiten que las VMs funcionen con mejor rendimiento.

Si la virtualización está desactivada en BIOS/UEFI del equipo físico, VirtualBox puede fallar al iniciar sistemas de 64 bits o tener bajo rendimiento.

Opciones frecuentes:

- **VT-x / AMD-V:** virtualización asistida por hardware.
- **Nested Paging:** mejora la gestión de memoria virtual.
- **Paravirtualization Interface:** optimización según el sistema invitado.

Recomendación:

- Dejar las opciones predeterminadas salvo que se esté diagnosticando un problema.

---

## 7. Configuración de pantalla

La sección **Pantalla** controla la salida de video de la VM.

### 7.1. Memoria de video

Define cuánta memoria se asignará al video virtual.

Ejemplos:

| Tipo de VM | Memoria de video sugerida |
|---|---:|
| Servidor sin GUI | 16 MB |
| Linux Desktop | 64 MB o más |
| Windows Desktop | 128 MB o más |

En Ubuntu Server, la memoria de video no es crítica porque se trabaja principalmente por consola.

### 7.2. Cantidad de monitores

Permite simular más de una pantalla.

Es útil para sistemas de escritorio, pero no suele utilizarse en servidores.

### 7.3. Factor de escala

Permite aumentar o reducir el tamaño visual de la pantalla de la VM.

Puede ser útil en proyectores o pantallas de alta resolución.

### 7.4. Controlador gráfico

VirtualBox ofrece distintos controladores gráficos virtuales.

Opciones frecuentes:

- **VMSVGA:** recomendado para muchas distribuciones Linux modernas.
- **VBoxSVGA:** útil para algunos sistemas Windows.
- **VBoxVGA:** opción antigua, menos recomendable para sistemas modernos.

Para Ubuntu Server, normalmente no es necesario modificar el controlador gráfico.

### 7.5. Aceleración 3D

Permite usar aceleración gráfica 3D en sistemas invitados compatibles.

Conviene activarla en sistemas con escritorio gráfico si se necesita mejor rendimiento visual. En servidores, generalmente no es necesaria.

---

## 8. Configuración de almacenamiento

La sección **Almacenamiento** define los discos virtuales y unidades ópticas conectadas a la VM.

### 8.1. Controladores de almacenamiento

Un controlador de almacenamiento es el dispositivo virtual al que se conectan discos y unidades ópticas.

Opciones frecuentes:

| Controlador | Uso común |
|---|---|
| SATA | Recomendado para la mayoría de las VMs modernas. |
| IDE | Sistemas antiguos o compatibilidad con instalaciones viejas. |
| SCSI | Escenarios específicos o servidores. |
| NVMe | Mayor rendimiento en sistemas modernos compatibles. |
| Floppy | Sistemas muy antiguos. |

Para Ubuntu Server en laboratorio, SATA es una opción adecuada y simple.

### 8.2. Disco duro virtual

El disco virtual es un archivo del host que se comporta como disco dentro de la VM.

Formatos comunes:

| Formato | Característica |
|---|---|
| VDI | Formato nativo de VirtualBox. Recomendado para uso normal. |
| VMDK | Compatible con VMware. Útil para interoperabilidad. |
| VHD | Compatible con tecnologías de Microsoft. |

### 8.3. Disco dinámico o de tamaño fijo

VirtualBox permite crear discos de dos formas:

| Tipo | Explicación | Ventaja | Desventaja |
|---|---|---|---|
| Dinámico | Crece a medida que se usa. | Ahorra espacio inicial. | Puede fragmentarse o crecer hasta ocupar mucho. |
| Tamaño fijo | Reserva todo el espacio desde el inicio. | Puede tener rendimiento más estable. | Ocupa todo el espacio inmediatamente. |

Para clase, se recomienda disco dinámico porque ahorra espacio en los equipos del laboratorio.

### 8.4. Unidad óptica virtual

Permite montar una imagen ISO como si fuera un CD/DVD.

Se usa para:

- Instalar sistemas operativos.
- Montar herramientas de Guest Additions.
- Arrancar medios de rescate.

Después de instalar el sistema operativo, conviene retirar la ISO o cambiar el orden de arranque para evitar iniciar nuevamente el instalador.

---

## 9. Configuración de audio

La sección **Audio** permite habilitar o deshabilitar sonido en la VM.

En servidores, normalmente se puede desactivar para ahorrar recursos y evitar dispositivos innecesarios.

En sistemas de escritorio, puede ser útil mantenerlo activado.

Opciones principales:

- Controlador de audio del host.
- Controlador de audio invitado.
- Entrada de audio.
- Salida de audio.

Para Ubuntu Server, no es necesario usar audio.

---

## 10. Configuración de red

La red es una de las partes más importantes de VirtualBox, especialmente en prácticas de servidores y soporte IT.

VirtualBox permite conectar hasta varios adaptadores de red virtuales por VM, dependiendo de la configuración y versión.

### 10.1. Adaptador NAT

NAT permite que la VM tenga salida a Internet usando la conexión del host.

Características:

- La VM puede navegar o descargar paquetes.
- Otros equipos de la red no pueden acceder directamente a la VM.
- Es simple y suele funcionar sin configurar nada en la red física.

Uso recomendado:

- Instalaciones simples.
- Descarga de paquetes.
- Pruebas donde no se necesita acceder a la VM desde otros equipos.

Limitación:

- Si se desea entrar por SSH desde otro equipo de la red, NAT no es suficiente sin configurar redirección de puertos.

### 10.2. Adaptador puente / Bridged Adapter

El modo bridged conecta la VM directamente a la red física.

Características:

- La VM se comporta como otro equipo de la red.
- Puede recibir IP del router por DHCP.
- Puede tener IP fija dentro del rango de la red local.
- Otros equipos pueden acceder a la VM si la red lo permite.

Uso recomendado:

- Servidores de laboratorio.
- Prácticas con SSH.
- Configuración de IP fija.
- Pruebas donde la VM debe ser visible desde la red.

Advertencia:

- En algunas redes Wi-Fi o redes institucionales, bridged puede tener restricciones.
- Debe evitarse asignar IPs que ya estén en uso.

### 10.3. Red interna / Internal Network

Permite conectar VMs entre sí dentro de una red aislada. El host no participa directamente y las VMs no salen a Internet por esa red.

Uso recomendado:

- Laboratorios de redes aisladas.
- Pruebas de cliente-servidor.
- Simulación de LAN interna.
- Prácticas con DHCP, DNS o firewall.

Ejemplo:

- VM1: servidor DHCP.
- VM2: cliente Linux.
- VM3: cliente Windows.

Todas conectadas a la misma red interna.

### 10.4. Adaptador solo anfitrión / Host-only Adapter

Crea una red entre el host y la VM. Puede permitir comunicación entre varias VMs y el anfitrión, pero normalmente no da salida directa a Internet.

Uso recomendado:

- Administrar VMs desde el host.
- Laboratorios aislados con acceso desde la computadora física.
- Prácticas de SSH sin exponer la VM a toda la red física.

### 10.5. Red NAT / NAT Network

Es similar a NAT, pero permite que varias VMs compartan una misma red NAT y se comuniquen entre ellas.

Uso recomendado:

- Laboratorios con varias VMs que necesitan Internet.
- Pruebas cliente-servidor sin exponer las VMs a la red física.

### 10.6. No conectado

La VM tiene una tarjeta de red virtual, pero no está conectada a ninguna red.

Uso recomendado:

- Pruebas de desconexión.
- Simular fallas de red.
- Instalar sistemas sin red.

### 10.7. Controlador de red virtual

VirtualBox permite elegir el tipo de tarjeta de red virtual.

Opciones frecuentes:

| Controlador | Uso común |
|---|---|
| Intel PRO/1000 MT Desktop | Compatible con muchos sistemas operativos. |
| Intel PRO/1000 T Server | Útil en algunos sistemas de servidor. |
| Virtio-net | Mejor rendimiento en sistemas modernos con soporte Virtio. |
| PCnet-FAST III | Compatibilidad con sistemas antiguos. |

Para Ubuntu Server, Intel PRO/1000 o Virtio-net suelen funcionar correctamente.

### 10.8. Modo promiscuo

Permite que la interfaz virtual reciba tráfico que no está destinado directamente a ella.

Opciones típicas:

- Denegar.
- Permitir VMs.
- Permitir todo.

Uso avanzado:

- Análisis de tráfico.
- Laboratorios con Wireshark.
- Simulación de redes.

Para prácticas normales, se deja en **Denegar**.

### 10.9. Cable conectado

Simula si el cable de red está conectado o desconectado.

Si está desmarcado, la VM verá la interfaz como desconectada, aunque el adaptador esté configurado.

Es útil para simular fallas físicas de red.

---

## 11. Configuración de puertos serie

Los puertos serie son interfaces antiguas o especializadas utilizadas para comunicación entre dispositivos.

En laboratorios modernos casi no se usan, salvo para:

- Sistemas embebidos.
- Equipos de red.
- Consolas seriales.
- Depuración avanzada.

Para prácticas normales de Ubuntu Server, pueden quedar desactivados.

---

## 12. Configuración USB

VirtualBox permite conectar dispositivos USB del host a la VM.

Ejemplos:

- Pendrive.
- Adaptador Wi-Fi USB.
- Lectores de tarjetas.
- Dispositivos serie USB.

### 12.1. Controladores USB

Opciones comunes:

- USB 1.1.
- USB 2.0.
- USB 3.0.

Para usar USB 2.0 o 3.0 puede ser necesario instalar el Extension Pack de VirtualBox, según la versión y licencia utilizada.

### 12.2. Filtros USB

Un filtro USB permite que un dispositivo específico sea capturado automáticamente por la VM al conectarlo.

Precaución:

- Si la VM captura un teclado, mouse o dispositivo crítico, puede afectar el uso del host.
- No se recomienda activar filtros sin saber exactamente qué dispositivo se está asociando.

---

## 13. Carpetas compartidas

Las carpetas compartidas permiten acceder desde la VM a una carpeta del equipo anfitrión.

Uso recomendado:

- Pasar archivos entre host y VM.
- Compartir scripts.
- Guardar evidencias de prácticas.

Opciones frecuentes:

| Opción | Explicación |
|---|---|
| Solo lectura | La VM puede leer, pero no modificar. |
| Automontar | La carpeta se monta automáticamente al iniciar la VM. |
| Permanente | La configuración queda guardada. |
| Punto de montaje | Ruta donde se verá la carpeta dentro del invitado. |

Precaución:

- No compartir carpetas sensibles.
- Evitar dar escritura si no es necesario.
- Recordar que una VM comprometida podría modificar archivos compartidos si tiene permisos.

---

## 14. Portapapeles compartido y arrastrar y soltar

VirtualBox puede permitir intercambio de texto y archivos entre host y VM.

Opciones típicas:

- Deshabilitado.
- Host a invitado.
- Invitado a host.
- Bidireccional.

Uso recomendado:

- En prácticas de clase puede ser útil activar portapapeles bidireccional para copiar comandos.
- En entornos de seguridad o malware, debe estar desactivado.

En servidores sin entorno gráfico, estas funciones pueden ser limitadas o no tener utilidad práctica.

---

## 15. Snapshots o instantáneas

Una instantánea guarda el estado de una VM en un momento específico.

Permite volver atrás si algo sale mal.

### 15.1. ¿Qué guarda una snapshot?

Puede incluir:

- Estado del disco virtual.
- Configuración de la VM.
- Estado de memoria, si se toma con la VM encendida.

### 15.2. Uso didáctico

Antes de realizar cambios importantes, se puede crear una snapshot.

Ejemplos:

- Antes de configurar red.
- Antes de instalar un servicio.
- Antes de modificar archivos críticos.
- Antes de una práctica de seguridad.

### 15.3. Precauciones

- No abusar de las snapshots.
- Muchas snapshots consumen espacio en disco.
- No reemplazan a una copia de seguridad real.
- En servidores reales, deben usarse con criterio.

---

## 16. Clonación de máquinas virtuales

Clonar una VM permite crear una copia a partir de una máquina existente.

### 16.1. Clon completo

Crea una copia independiente de la VM y su disco.

Ventaja:

- La copia no depende de la VM original.

Desventaja:

- Ocupa más espacio.

### 16.2. Clon enlazado

Crea una VM nueva que depende del disco base de la original.

Ventaja:

- Ocupa menos espacio.

Desventaja:

- Depende de la VM base.
- Si se elimina o modifica la base incorrectamente, puede afectar clones.

Uso en clase:

- Crear varias VMs desde una instalación limpia.
- Preparar entornos repetibles para estudiantes.

---

## 17. Exportar e importar máquinas virtuales

VirtualBox permite exportar una VM como appliance, normalmente en formato OVA u OVF.

### 17.1. Exportar

Sirve para empaquetar una VM y moverla a otro equipo.

Uso recomendado:

- Entregar una VM preparada.
- Respaldar un laboratorio.
- Compartir entornos entre docentes o estudiantes.

### 17.2. Importar

Permite cargar una VM previamente exportada.

Precaución:

- Revisar RAM, CPU y red después de importar.
- La VM puede necesitar ajustes según el equipo donde se ejecute.

---

## 18. Guest Additions

Las Guest Additions son herramientas que mejoran la integración entre el sistema anfitrión y el invitado.

Pueden mejorar:

- Resolución de pantalla.
- Integración del mouse.
- Carpetas compartidas.
- Portapapeles compartido.
- Sincronización horaria.
- Controladores del sistema invitado.

En servidores sin interfaz gráfica, algunas funciones no se aprovechan, pero pueden seguir siendo útiles para integración básica.

---

## 19. Buenas prácticas al crear VMs

Antes de crear una VM, conviene definir:

- Sistema operativo a instalar.
- Recursos mínimos necesarios.
- Propósito de la VM.
- Tipo de red requerido.
- Tamaño de disco.
- Si se necesitarán snapshots.
- Si se compartirá con otros equipos.

Recomendaciones generales:

- Usar nombres claros.
- No asignar todos los recursos del host.
- Documentar usuario, hostname e IP.
- Crear snapshot antes de cambios importantes.
- Apagar correctamente la VM.
- No borrar archivos de VM manualmente.
- Mantener ISOs organizadas en una carpeta específica.
- Evitar configuraciones avanzadas sin comprender su efecto.

---

## 20. Introducción a VBoxManage

`VBoxManage` es la herramienta de línea de comandos de VirtualBox. Permite administrar máquinas virtuales desde la terminal.

Con `VBoxManage` se puede:

- Listar VMs.
- Crear máquinas virtuales.
- Modificar RAM, CPU y red.
- Crear discos virtuales.
- Conectar ISOs.
- Iniciar y apagar VMs.
- Crear snapshots.
- Exportar e importar VMs.
- Consultar información de configuración.

### 20.1. ¿Dónde se ejecuta VBoxManage?

Depende del sistema operativo anfitrión.

#### En Linux o macOS

Generalmente se puede ejecutar desde terminal:

```bash
VBoxManage --version
```

#### En Windows

Puede ser necesario ejecutar el comando desde la carpeta de instalación:

```powershell
"C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" --version
```

También se puede agregar VirtualBox al `PATH` del sistema para ejecutar `VBoxManage` desde cualquier ubicación.

---

## 21. Comandos básicos de VBoxManage

### 21.1. Ver versión instalada

```bash
VBoxManage --version
```

Sirve para confirmar que VirtualBox CLI está disponible.

### 21.2. Listar máquinas virtuales registradas

```bash
VBoxManage list vms
```

Muestra las VMs conocidas por VirtualBox.

Ejemplo de salida:

```text
"Ubuntu-Server-Lab" {uuid-de-la-vm}
"Windows-Test" {uuid-de-la-vm}
```

### 21.3. Listar máquinas virtuales encendidas

```bash
VBoxManage list runningvms
```

Muestra solo las VMs que están en ejecución.

### 21.4. Ver información detallada de una VM

```bash
VBoxManage showvminfo "Ubuntu-Server-Lab"
```

Permite revisar RAM, CPU, red, almacenamiento, snapshots y otros datos.

### 21.5. Listar tipos de sistemas operativos soportados

```bash
VBoxManage list ostypes
```

Esto ayuda a identificar el valor correcto para `--ostype` al crear una VM por CLI.

---

## 22. Crear una VM desde CLI

A continuación se muestra un ejemplo completo para crear una VM de Ubuntu Server desde la terminal.

### 22.1. Crear y registrar la VM

```bash
VBoxManage createvm --name "Ubuntu-Server-CLI" --ostype Ubuntu_64 --register
```

Explicación:

- `createvm`: crea una nueva máquina virtual.
- `--name`: define el nombre.
- `--ostype`: define el tipo de sistema operativo.
- `--register`: registra la VM en VirtualBox.

### 22.2. Configurar RAM, CPU y arranque

```bash
VBoxManage modifyvm "Ubuntu-Server-CLI" \
  --memory 2048 \
  --cpus 2 \
  --boot1 dvd \
  --boot2 disk \
  --graphicscontroller vmsvga
```

Explicación:

- `--memory 2048`: asigna 2048 MB de RAM.
- `--cpus 2`: asigna dos CPU virtuales.
- `--boot1 dvd`: intenta arrancar primero desde ISO.
- `--boot2 disk`: luego intenta arrancar desde disco.
- `--graphicscontroller vmsvga`: usa controlador gráfico compatible con Linux moderno.

---

## 23. Crear y conectar un disco virtual por CLI

### 23.1. Crear disco VDI

```bash
VBoxManage createmedium disk \
  --filename "$HOME/VirtualBox VMs/Ubuntu-Server-CLI/Ubuntu-Server-CLI.vdi" \
  --size 25000 \
  --format VDI
```

Explicación:

- `createmedium disk`: crea un disco virtual.
- `--filename`: indica dónde se guardará.
- `--size 25000`: tamaño en MB, aproximadamente 25 GB.
- `--format VDI`: formato nativo de VirtualBox.

### 23.2. Crear controlador SATA

```bash
VBoxManage storagectl "Ubuntu-Server-CLI" \
  --name "SATA Controller" \
  --add sata \
  --controller IntelAhci
```

### 23.3. Conectar el disco al controlador

```bash
VBoxManage storageattach "Ubuntu-Server-CLI" \
  --storagectl "SATA Controller" \
  --port 0 \
  --device 0 \
  --type hdd \
  --medium "$HOME/VirtualBox VMs/Ubuntu-Server-CLI/Ubuntu-Server-CLI.vdi"
```

---

## 24. Conectar una ISO por CLI

### 24.1. Crear controlador IDE para la ISO

```bash
VBoxManage storagectl "Ubuntu-Server-CLI" \
  --name "IDE Controller" \
  --add ide
```

### 24.2. Montar la ISO

```bash
VBoxManage storageattach "Ubuntu-Server-CLI" \
  --storagectl "IDE Controller" \
  --port 0 \
  --device 0 \
  --type dvddrive \
  --medium "/ruta/a/ubuntu-server.iso"
```

Reemplazar:

```text
/ruta/a/ubuntu-server.iso
```

por la ruta real del archivo ISO.

---

## 25. Configurar red por CLI

### 25.1. Configurar NAT

```bash
VBoxManage modifyvm "Ubuntu-Server-CLI" \
  --nic1 nat
```

Uso:

- Permite salida a Internet desde la VM.
- No expone la VM directamente en la red física.

### 25.2. Configurar bridged

Primero listar interfaces disponibles:

```bash
VBoxManage list bridgedifs
```

Luego configurar el adaptador puente:

```bash
VBoxManage modifyvm "Ubuntu-Server-CLI" \
  --nic1 bridged \
  --bridgeadapter1 "NOMBRE_DEL_ADAPTADOR"
```

Ejemplo:

```bash
VBoxManage modifyvm "Ubuntu-Server-CLI" \
  --nic1 bridged \
  --bridgeadapter1 "en0: Wi-Fi"
```

El nombre exacto del adaptador depende del sistema anfitrión.

### 25.3. Configurar red interna

```bash
VBoxManage modifyvm "Ubuntu-Server-CLI" \
  --nic1 intnet \
  --intnet1 "red-laboratorio"
```

Todas las VMs conectadas a `red-laboratorio` podrán comunicarse entre sí.

### 25.4. Configurar host-only

Primero listar interfaces host-only:

```bash
VBoxManage list hostonlyifs
```

Luego asignar una:

```bash
VBoxManage modifyvm "Ubuntu-Server-CLI" \
  --nic1 hostonly \
  --hostonlyadapter1 "vboxnet0"
```

---

## 26. Iniciar, apagar y controlar VMs por CLI

### 26.1. Iniciar VM con ventana gráfica

```bash
VBoxManage startvm "Ubuntu-Server-CLI" --type gui
```

### 26.2. Iniciar VM en modo headless

```bash
VBoxManage startvm "Ubuntu-Server-CLI" --type headless
```

Modo headless significa que la VM corre sin ventana visible. Es útil para servidores.

### 26.3. Apagar VM de forma ACPI

```bash
VBoxManage controlvm "Ubuntu-Server-CLI" acpipowerbutton
```

Esto simula presionar el botón de apagado del equipo. Es más seguro que apagar forzadamente.

### 26.4. Apagar VM forzadamente

```bash
VBoxManage controlvm "Ubuntu-Server-CLI" poweroff
```

Precaución:

- Equivale a cortar la energía.
- Puede causar pérdida de datos.
- Usar solo si la VM no responde.

### 26.5. Pausar VM

```bash
VBoxManage controlvm "Ubuntu-Server-CLI" pause
```

### 26.6. Reanudar VM

```bash
VBoxManage controlvm "Ubuntu-Server-CLI" resume
```

### 26.7. Guardar estado

```bash
VBoxManage controlvm "Ubuntu-Server-CLI" savestate
```

Guarda el estado actual de la VM para continuar luego.

---

## 27. Snapshots por CLI

### 27.1. Crear snapshot

```bash
VBoxManage snapshot "Ubuntu-Server-CLI" take "Antes de configurar red"
```

### 27.2. Listar snapshots

```bash
VBoxManage snapshot "Ubuntu-Server-CLI" list
```

### 27.3. Restaurar snapshot

```bash
VBoxManage snapshot "Ubuntu-Server-CLI" restore "Antes de configurar red"
```

### 27.4. Eliminar snapshot

```bash
VBoxManage snapshot "Ubuntu-Server-CLI" delete "Antes de configurar red"
```

Precaución:

- Restaurar una snapshot revierte cambios posteriores.
- Eliminar snapshots puede tardar si hay muchos cambios acumulados.

---

## 28. Clonar VMs por CLI

### 28.1. Clon completo

```bash
VBoxManage clonevm "Ubuntu-Server-CLI" \
  --name "Ubuntu-Server-CLI-Clon" \
  --register
```

### 28.2. Clon enlazado

```bash
VBoxManage clonevm "Ubuntu-Server-CLI" \
  --name "Ubuntu-Server-CLI-Link" \
  --options link \
  --register
```

Uso recomendado:

- Clon completo: cuando se necesita independencia.
- Clon enlazado: cuando se busca ahorrar espacio en laboratorio.

---

## 29. Exportar e importar VMs por CLI

### 29.1. Exportar a OVA

```bash
VBoxManage export "Ubuntu-Server-CLI" \
  --output "Ubuntu-Server-CLI.ova"
```

### 29.2. Importar una OVA

```bash
VBoxManage import "Ubuntu-Server-CLI.ova"
```

Uso:

- Respaldar una VM.
- Mover una VM entre equipos.
- Compartir un laboratorio preparado.

---

## 30. Eliminar una VM por CLI

### 30.1. Desregistrar sin borrar archivos

```bash
VBoxManage unregistervm "Ubuntu-Server-CLI"
```

La VM deja de aparecer en VirtualBox, pero los archivos pueden quedar en disco.

### 30.2. Desregistrar y borrar archivos

```bash
VBoxManage unregistervm "Ubuntu-Server-CLI" --delete
```

Precaución:

- Este comando borra los archivos asociados a la VM.
- Debe usarse con cuidado.

---

## 31. Ejemplo completo: crear una VM Ubuntu Server desde cero

Este ejemplo resume varios pasos anteriores.

> Ajustar rutas y nombres de adaptador según el equipo anfitrión.

```bash
# 1. Crear VM
VBoxManage createvm --name "Ubuntu-Server-CLI" --ostype Ubuntu_64 --register

# 2. Configurar recursos básicos
VBoxManage modifyvm "Ubuntu-Server-CLI" \
  --memory 2048 \
  --cpus 2 \
  --boot1 dvd \
  --boot2 disk \
  --graphicscontroller vmsvga \
  --nic1 bridged \
  --bridgeadapter1 "NOMBRE_DEL_ADAPTADOR"

# 3. Crear disco virtual
VBoxManage createmedium disk \
  --filename "$HOME/VirtualBox VMs/Ubuntu-Server-CLI/Ubuntu-Server-CLI.vdi" \
  --size 25000 \
  --format VDI

# 4. Crear controlador SATA
VBoxManage storagectl "Ubuntu-Server-CLI" \
  --name "SATA Controller" \
  --add sata \
  --controller IntelAhci

# 5. Conectar disco
VBoxManage storageattach "Ubuntu-Server-CLI" \
  --storagectl "SATA Controller" \
  --port 0 \
  --device 0 \
  --type hdd \
  --medium "$HOME/VirtualBox VMs/Ubuntu-Server-CLI/Ubuntu-Server-CLI.vdi"

# 6. Crear controlador IDE
VBoxManage storagectl "Ubuntu-Server-CLI" \
  --name "IDE Controller" \
  --add ide

# 7. Conectar ISO
VBoxManage storageattach "Ubuntu-Server-CLI" \
  --storagectl "IDE Controller" \
  --port 0 \
  --device 0 \
  --type dvddrive \
  --medium "/ruta/a/ubuntu-server.iso"

# 8. Iniciar VM
VBoxManage startvm "Ubuntu-Server-CLI" --type gui
```

---

## 32. Actividad práctica sugerida

### Consigna

Crear una máquina virtual llamada:

```text
Ubuntu-Server-CLI-Apellido
```

La VM debe tener:

- 2 GB de RAM.
- 2 CPU virtuales.
- Disco VDI dinámico de 25 GB.
- ISO de Ubuntu Server montada.
- Red en modo bridged o NAT, según indique el docente.
- Orden de arranque desde ISO primero y disco después.

### Parte A: trabajo desde interfaz gráfica

1. Crear la VM desde VirtualBox GUI.
2. Configurar RAM, CPU, disco y red.
3. Iniciar la VM.
4. Verificar que arranque desde la ISO.

### Parte B: trabajo desde CLI

Ejecutar y registrar la salida de:

```bash
VBoxManage list vms
VBoxManage showvminfo "Ubuntu-Server-CLI-Apellido"
VBoxManage list runningvms
```

### Parte C: administración básica

1. Crear una snapshot llamada `Instalacion limpia`.
2. Listar las snapshots.
3. Apagar la VM usando ACPI.
4. Iniciar la VM en modo headless.
5. Apagar la VM de forma segura.

Comandos sugeridos:

```bash
VBoxManage snapshot "Ubuntu-Server-CLI-Apellido" take "Instalacion limpia"
VBoxManage snapshot "Ubuntu-Server-CLI-Apellido" list
VBoxManage controlvm "Ubuntu-Server-CLI-Apellido" acpipowerbutton
VBoxManage startvm "Ubuntu-Server-CLI-Apellido" --type headless
```

---

## 33. Preguntas de comprensión

Responder con tus palabras:

1. ¿Qué diferencia hay entre el host y el guest?
2. ¿Por qué no conviene asignar toda la RAM del equipo físico a una VM?
3. ¿Qué diferencia hay entre NAT y Bridged Adapter?
4. ¿En qué situación usarías una red interna?
5. ¿Para qué sirve una snapshot?
6. ¿Qué riesgo tiene apagar una VM con `poweroff`?
7. ¿Por qué `VBoxManage` puede ser útil para un administrador de sistemas?
8. ¿Qué diferencia hay entre un disco dinámico y uno de tamaño fijo?
9. ¿Qué ventaja tiene iniciar una VM en modo headless?
10. ¿Por qué conviene documentar la configuración de una VM?

---

## 34. Criterios de evaluación

| Criterio | Excelente | Aceptable | En proceso |
|---|---|---|---|
| Comprensión conceptual | Explica claramente virtualización, host, guest y VM. | Explica los conceptos principales con algunas imprecisiones. | Presenta dificultades para diferenciar conceptos básicos. |
| Configuración de VM | Configura correctamente RAM, CPU, disco, red y arranque. | Configura la VM con errores menores. | Requiere asistencia constante para configurar la VM. |
| Uso de red | Diferencia y aplica NAT, bridged, host-only o red interna según el caso. | Reconoce algunos modos de red. | No logra justificar el modo de red usado. |
| Uso de CLI | Ejecuta comandos `VBoxManage` y comprende su función. | Ejecuta comandos básicos con explicación parcial. | Copia comandos sin interpretar su resultado. |
| Evidencias | Presenta capturas y salidas de comandos completas. | Presenta evidencias parciales. | No entrega evidencias suficientes. |
| Resolución de problemas | Diagnostica errores simples y propone soluciones. | Resuelve errores con apoyo. | No identifica causas probables de error. |
| Documentación | Registra configuración y conclusiones con claridad. | Documenta parcialmente. | La documentación es incompleta o confusa. |

---

## 35. Resolución de problemas frecuentes

### Problema 1: VBoxManage no se reconoce como comando

Posibles causas:

- VirtualBox no está instalado.
- `VBoxManage` no está en el PATH.
- En Windows se debe usar la ruta completa al ejecutable.

Solución en Windows:

```powershell
"C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" --version
```

### Problema 2: La VM no inicia sistemas de 64 bits

Posibles causas:

- Virtualización desactivada en BIOS/UEFI.
- Hyper-V u otro hipervisor interfiriendo en Windows.
- Equipo físico sin soporte adecuado.

Soluciones posibles:

- Activar Intel VT-x o AMD-V en BIOS/UEFI.
- Revisar funciones de virtualización del sistema anfitrión.
- Reiniciar el equipo después de cambios.

### Problema 3: La VM no tiene Internet

Verificar:

- Que el adaptador esté habilitado.
- Que esté marcada la opción cable conectado.
- Que el modo de red sea adecuado.
- Si usa bridged, revisar el adaptador físico seleccionado.
- Si usa NAT, probar reiniciar la VM.

### Problema 4: La VM arranca siempre desde la ISO

Soluciones:

- Retirar la ISO de la unidad óptica virtual.
- Cambiar el orden de arranque para que el disco quede primero.
- Verificar que la instalación haya finalizado correctamente.

### Problema 5: La VM funciona muy lenta

Posibles causas:

- RAM insuficiente.
- CPU insuficiente.
- Disco del host lento o lleno.
- Muchas VMs abiertas al mismo tiempo.
- Host con pocos recursos disponibles.

Soluciones:

- Cerrar programas innecesarios.
- Reducir cantidad de VMs encendidas.
- Asignar recursos de forma equilibrada.
- Usar sistemas sin entorno gráfico para prácticas de servidor.

---

## 36. Cierre

VirtualBox es una herramienta fundamental para aprender virtualización, sistemas operativos, redes y administración de servidores. Permite crear laboratorios seguros y repetibles, donde los estudiantes pueden practicar instalación, configuración, diagnóstico y resolución de problemas sin afectar el equipo físico.

Comprender sus opciones de configuración ayuda a tomar mejores decisiones técnicas. No se trata solo de crear una VM y presionar “iniciar”, sino de entender qué recursos se asignan, cómo se conecta a la red, cómo arranca, dónde guarda sus discos y cómo puede administrarse de forma gráfica o automatizada.

El uso de `VBoxManage` agrega una dimensión profesional: permite automatizar tareas, documentar configuraciones, crear entornos repetibles y administrar máquinas virtuales de forma más precisa. Estas habilidades son transferibles a otros entornos de virtualización y administración de infraestructura.

