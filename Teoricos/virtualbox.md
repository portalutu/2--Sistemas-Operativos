# VirtualBox

# 1. ¿Qué es VirtualBox?

**VirtualBox** es un software de virtualización que permite ejecutar sistemas operativos invitados dentro de una computadora anfitriona.

VirtualBox es un **hipervisor de tipo 2**.

Esto significa que:

* se instala sobre un sistema operativo ya existente
* depende del sistema operativo anfitrión para acceder al hardware
* es ideal para laboratorios, pruebas, capacitación y uso educativo

Por ejemplo:

* Windows puede ser el sistema anfitrión y Ubuntu Server el invitado
* Linux puede ser el sistema anfitrión y Windows el invitado

En general, los hipervisores de tipo 2 son más sencillos de usar en entornos de escritorio que en centros de datos o servidores de producción.

Sitio oficial:

* https://www.virtualbox.org/

Descargas oficiales:

* https://www.oracle.com/virtualization/technologies/vm/downloads/virtualbox-downloads.html

---

# 2. ¿Para qué se utiliza?

VirtualBox se utiliza para:

* instalar varios sistemas operativos en una misma computadora
* practicar administración de sistemas sin afectar el sistema principal
* crear laboratorios de redes
* probar software
* aprender Linux, Windows Server y otras plataformas
* tomar snapshots y volver rápidamente a un estado anterior

---

# 3. Requisitos generales

Para usar VirtualBox conviene contar con:

* procesador de 64 bits
* soporte de virtualización por hardware activado en BIOS o UEFI
* memoria RAM suficiente
* espacio libre en disco

Recomendación para prácticas:

* mínimo 8 GB de RAM en el equipo anfitrión
* al menos 30 GB libres en disco

Si la virtualización por hardware está desactivada, algunas máquinas virtuales pueden no iniciar correctamente o funcionar con bajo rendimiento.

---

# 4. Descarga de VirtualBox

## 4.1 Descarga desde el sitio oficial

Ingresar a:

* https://www.virtualbox.org/wiki/Downloads

o a la página oficial de Oracle:

* https://www.oracle.com/virtualization/technologies/vm/downloads/virtualbox-downloads.html

Allí se encuentra el paquete base para diferentes sistemas operativos.

También puede aparecer el **Extension Pack**, que agrega funciones extra como:

* soporte USB 2.0 y USB 3.0
* VirtualBox RDP
* algunas mejoras de integración

Para un curso básico, el paquete base suele ser suficiente.

---

# 5. Instalación en Windows

## 5.1 Pasos de instalación

1. Descargar el instalador para Windows desde el sitio oficial.
2. Ejecutar el archivo `.exe`.
3. Aceptar el contrato de licencia.
4. Elegir los componentes a instalar.
5. Continuar con la instalación.
6. Aceptar los controladores de red o dispositivos virtuales cuando Windows lo solicite.
7. Finalizar y abrir VirtualBox.

---

## 5.2 Componentes que suelen aparecer en el instalador

Durante la instalación pueden aparecer opciones como:

* `VirtualBox Application`
* `USB Support`
* `Networking`
* `Python Support`

Para un laboratorio educativo, normalmente conviene dejar la instalación por defecto.

---

## 5.3 Advertencia sobre red

Durante la instalación, Windows puede mostrar un aviso indicando que se reiniciarán o reconfigurarán interfaces de red temporalmente.

Esto sucede porque VirtualBox instala adaptadores virtuales para:

* red NAT
* Host-Only
* Bridged

Es normal que la conectividad de red se interrumpa por unos segundos.

---

# 6. Instalación en Linux

## 6.1 Opciones de instalación

En Linux, VirtualBox puede instalarse de dos formas principales:

* desde los repositorios de la distribución
* desde el paquete oficial descargado del sitio de Oracle

Para prácticas de laboratorio, muchas veces se utiliza el paquete oficial o el paquete provisto por la distribución.

---

## 6.2 Instalación en Ubuntu o Debian desde repositorios

Ejemplo:

```bash
sudo apt update
sudo apt install virtualbox
```

Según la versión de la distribución, el paquete disponible puede no ser el más reciente.

---

## 6.3 Instalación desde paquete oficial

1. Descargar el paquete `.deb` o `.rpm` correspondiente desde la web oficial.
2. Instalar el paquete con la herramienta de la distribución.

Ejemplo en Ubuntu o Debian:

```bash
sudo dpkg -i virtualbox-*.deb
sudo apt -f install
```

Ejemplo en Fedora, Rocky o similares:

```bash
sudo rpm -ivh VirtualBox-*.rpm
```

---

## 6.4 Dependencias y módulos del kernel

En Linux, VirtualBox necesita módulos del kernel para funcionar correctamente.

En algunos casos será necesario instalar:

* headers del kernel
* herramientas de compilación
* `dkms`

Si falta alguno de estos componentes, VirtualBox puede instalarse pero no iniciar las máquinas virtuales.

---

# 7. Primer inicio de VirtualBox

Al abrir VirtualBox se muestra el administrador principal de máquinas virtuales.

Desde esa ventana se pueden realizar tareas como:

* crear una nueva máquina virtual
* importar o exportar appliances
* modificar configuraciones
* iniciar, pausar o apagar máquinas virtuales
* tomar snapshots

---

# 8. Crear una máquina virtual

## 8.1 Pasos básicos

1. Presionar `Nueva`.
2. Indicar nombre de la máquina virtual.
3. Elegir el tipo de sistema operativo.
4. Seleccionar la versión.
5. Asignar memoria RAM.
6. Crear un disco virtual.
7. Finalizar el asistente.

Ejemplo:

* Nombre: `Ubuntu Server`
* Tipo: `Linux`
* Versión: `Ubuntu (64-bit)`

---

# 9. Configuración básica

Las opciones básicas son las necesarias para que una máquina virtual funcione correctamente en un laboratorio simple.

## 9.1 Nombre y carpeta

Permite definir:

* nombre visible de la máquina virtual
* ubicación donde se guardarán sus archivos

Es recomendable usar nombres claros:

* `Ubuntu-Server-Grupo1`
* `Debian-Lab-Redes`

---

## 9.2 Tipo y versión del sistema operativo

Esto ayuda a VirtualBox a proponer valores adecuados para:

* memoria
* almacenamiento
* chipset
* configuración general

---

## 9.3 Memoria RAM

Define cuánta RAM se asigna a la máquina virtual.

Recomendaciones aproximadas:

* Ubuntu Server básico: 1 GB a 2 GB
* Ubuntu Desktop: 4 GB o más
* Windows moderno: 4 GB o más

No conviene asignar demasiada RAM, porque el sistema anfitrión también necesita memoria para funcionar.

---

## 9.4 Disco duro virtual

Se puede crear un disco virtual en formatos como:

* `VDI`
* `VMDK`
* `VHD`

Para uso con VirtualBox, el formato más común es:

* `VDI`

Luego se puede elegir:

* tamaño fijo
* reservado dinámicamente

El modo dinámico suele ser más práctico en entornos de clase.

---

## 9.5 ISO de instalación

Para instalar un sistema operativo, se debe montar una imagen:

* `.iso`

Esto se configura desde la unidad óptica virtual o durante el asistente de creación.

---

# 10. Configuración media

Las opciones medias permiten mejorar la experiencia y adaptar mejor la máquina virtual a un escenario de laboratorio.

## 10.1 Procesadores

Se puede asignar más de un CPU virtual.

Esto mejora el rendimiento en algunos casos, pero no conviene exagerar.

Ejemplo:

* 1 vCPU para servidores pequeños
* 2 vCPU para prácticas más exigentes

---

## 10.2 Video

Permite ajustar:

* memoria de video
* controlador gráfico
* aceleración 3D

En servidores, esta configuración no suele ser crítica.

En escritorios virtuales, puede mejorar la interfaz gráfica.

---

## 10.3 Audio

Puede habilitarse o deshabilitarse según necesidad.

En máquinas virtuales de servidor normalmente no es necesario usar audio.

---

## 10.4 Carpetas compartidas

Permiten compartir archivos entre:

* sistema anfitrión
* sistema invitado

Son útiles para:

* copiar scripts
* mover ISOs
* compartir documentos de clase

---

## 10.5 Portapapeles compartido y arrastrar y soltar

Facilitan la interacción entre anfitrión e invitado.

Opciones posibles:

* deshabilitado
* anfitrión a invitado
* invitado a anfitrión
* bidireccional

Para que estas funciones trabajen mejor, normalmente se instalan las **Guest Additions**.

---

# 11. Configuración avanzada

Las opciones avanzadas permiten construir laboratorios más complejos o ajustar con mayor precisión el comportamiento de la máquina virtual.

## 11.1 Red

Es una de las configuraciones más importantes.

VirtualBox ofrece varios modos de red.

### NAT

La máquina virtual sale a Internet usando la conexión del anfitrión.

Ventajas:

* simple
* suele funcionar sin cambios adicionales

Uso típico:

* una VM con acceso a Internet para actualizar paquetes

---

### NAT Network

Similar a NAT, pero pensado para que varias máquinas virtuales compartan una red NAT entre sí.

Uso típico:

* laboratorios con varias VMs conectadas entre sí y con salida a Internet

---

### Bridged Adapter

La máquina virtual se conecta a la red física como si fuera otro equipo real.

Ventajas:

* recibe IP de la red local
* otros equipos pueden verla directamente

Uso típico:

* prácticas de servicios de red
* servidores visibles desde la LAN

---

### Host-Only Adapter

La VM se comunica con el anfitrión y con otras VMs de esa red privada, pero no necesariamente con Internet.

Uso típico:

* laboratorios aislados
* prácticas internas entre máquinas virtuales

---

### Internal Network

Solo se comunican las máquinas virtuales entre sí dentro de una red interna de VirtualBox.

Uso típico:

* simulación de topologías sin acceso al anfitrión ni a Internet

---

## 11.2 Almacenamiento

Desde esta sección se pueden:

* agregar discos adicionales
* cambiar controladores SATA o IDE
* montar o desmontar ISOs

Esto es útil en prácticas de:

* particionado
* RAID por software
* instalación de sistemas

---

## 11.3 Snapshots

Un **snapshot** guarda el estado de una máquina virtual en un momento determinado.

Permite:

* volver atrás si algo falla
* probar configuraciones riesgosas
* preparar laboratorios reutilizables

Es una herramienta muy valiosa para el trabajo práctico.

---

## 11.4 USB

VirtualBox puede permitir que la máquina virtual acceda a dispositivos USB del equipo anfitrión.

Esto puede usarse para:

* pendrives
* adaptadores de red
* dispositivos de almacenamiento

En algunos casos esta función depende del **Extension Pack**.

---

## 11.5 Orden de arranque

Se puede definir el orden de inicio de los dispositivos:

* disco
* red
* unidad óptica

Esto es útil cuando se desea:

* arrancar desde una ISO
* evitar que la VM vuelva a iniciar el instalador

---

## 11.6 Modo EFI

VirtualBox puede arrancar una VM usando BIOS virtual tradicional o EFI.

EFI puede ser necesario para algunos sistemas operativos modernos, aunque en laboratorios básicos muchas veces alcanza con la configuración por defecto.

---

# 12. Guest Additions

Las **Guest Additions** son componentes que se instalan dentro del sistema operativo invitado para mejorar la integración con el anfitrión.

Pueden aportar:

* mejor soporte de video
* integración del mouse
* portapapeles compartido
* carpetas compartidas
* mejor cambio de resolución

Se instalan desde el menú de VirtualBox cuando la máquina virtual ya está iniciada.

---

# 13. Recomendaciones para el laboratorio

* usar nombres claros para las máquinas virtuales
* asignar solo la RAM necesaria
* tomar snapshots antes de cambios importantes
* verificar que la ISO corresponda al sistema a instalar
* elegir correctamente el modo de red según la práctica
* apagar correctamente la VM antes de copiar o mover archivos

---

# 14. Problemas frecuentes

## 14.1 No aparece la opción de 64 bits

Posibles causas:

* virtualización por hardware desactivada en BIOS o UEFI
* Hyper-V u otro hipervisor interfiriendo en Windows

---

## 14.2 La VM anda lenta

Posibles causas:

* poca RAM
* pocos CPU asignados
* disco del anfitrión muy lento
* demasiadas VMs ejecutándose al mismo tiempo

---

## 14.3 No hay red

Conviene revisar:

* modo de red configurado
* conexión del anfitrión
* configuración IP de la VM
* reglas de firewall

---

## 14.4 No inicia el sistema desde la ISO

Conviene revisar:

* que la ISO esté correctamente montada
* orden de arranque
* compatibilidad entre ISO y arquitectura

---

# 15. Ejemplo de configuración recomendada para Ubuntu Server

Para una práctica básica con **Ubuntu Server**, una configuración razonable puede ser:

* nombre: `Ubuntu-Server-Lab`
* tipo: `Linux`
* versión: `Ubuntu (64-bit)`
* memoria RAM: `2048 MB`
* procesadores: `2`
* disco: `20 GB` dinámico en formato `VDI`
* red: `NAT` para prácticas simples o `Bridged` para servicios visibles en la red
* ISO: imagen oficial de Ubuntu Server

Esta configuración permite realizar la mayoría de las prácticas iniciales del curso sin exigir demasiado al equipo anfitrión.
