# Sistemas Operativos - 2º EMT

Repositorio de materiales para la asignatura **Sistemas Operativos** de 2º EMT.

El contenido reúne apuntes teóricos, guías prácticas, actividades de investigación y recursos interactivos para trabajar conceptos de GNU/Linux, virtualización, instalación de servidores, administración básica del sistema, red y uso de la terminal.

## Estructura del repositorio

```text
.
├── Actividades/
├── Teoricos/
├── Obsoletos/
├── README.md
└── .gitignore
```

## Materiales teóricos

Los archivos de la carpeta [`Teoricos`](Teoricos/) contienen apuntes de apoyo para clase:

- [`Sistemas Operativos.md`](Teoricos/Sistemas%20Operativos.md): introducción general a los sistemas operativos, evolución histórica, Unix, Linux, software libre, kernel, procesos, memoria, archivos, seguridad y redes.
- [`Linux.md`](Teoricos/Linux.md): introducción a Linux desde la administración de sistemas, arquitectura, usuarios, daemons, procesos, servicios, logs, red, permisos y comandos de diagnóstico.
- [`linux_commands.md`](Teoricos/linux_commands.md): referencia amplia de comandos de Linux organizada por tareas: sistema, archivos, permisos, procesos, red, paquetes, discos, compresión y ayuda.
- [`virtual.md`](Teoricos/virtual.md): virtualización, hipervisores, máquinas virtuales, contenedores, Docker, Kubernetes y orquestación.
- [`virtualbox.md`](Teoricos/virtualbox.md): guía teórica y práctica sobre VirtualBox, instalación, creación de máquinas virtuales y configuración.
- [`netplan.md`](Teoricos/netplan.md): explicación de Netplan en Ubuntu, configuración DHCP, IP estática, rutas, DNS y comandos de validación.
- [`storage.ods`](Teoricos/storage.ods): planilla de apoyo relacionada con contenidos de almacenamiento.

## Actividades y prácticas

La carpeta [`Actividades`](Actividades/) contiene consignas, prácticas guiadas y recursos para trabajo en clase:

- [`actividad_clase1.md`](Actividades/actividad_clase1.md): actividades de investigación sobre evolución de sistemas operativos, filosofía Unix, Linux, software libre, kernel y procesos.
- [`práctica1.md`](Actividades/pr%C3%A1ctica1.md): práctica inicial de terminal Linux con navegación, exploración del sistema, archivos, permisos, procesos y comandos básicos.
- [`tarea_investigacion_distro_linux.md`](Actividades/tarea_investigacion_distro_linux.md): tarea grupal para investigar, instalar y presentar una distribución GNU/Linux.
- [`actividad_instalacion_ubuntu_server_virtualbox.md`](Actividades/actividad_instalacion_ubuntu_server_virtualbox.md): práctica completa de instalación y configuración de Ubuntu Server en VirtualBox.
- [`guia_virtualbox_configuracion_cli.md`](Actividades/guia_virtualbox_configuracion_cli.md): guía sobre configuración de máquinas virtuales en VirtualBox y uso de `VBoxManage` por línea de comandos.
- [`so_2_emt_comandos_linux_interactiva_sin_subtitulo.html`](Actividades/so_2_emt_comandos_linux_interactiva_sin_subtitulo.html): recurso interactivo sobre comandos Linux, con terminal simulada y actividades.
- [`tutorial_ubuntu_server_virtualbox_interactivo.html`](Actividades/tutorial_ubuntu_server_virtualbox_interactivo.html): tutorial interactivo para instalar Ubuntu Server en VirtualBox, configurar red con Netplan, compartir carpetas por SMB e instalar Docker.

## Material obsoleto

La carpeta [`Obsoletos`](Obsoletos/) conserva materiales que fueron reemplazados o quedaron como referencia histórica:

- [`comandos_linux.md`](Obsoletos/comandos_linux.md): versión anterior de una guía de comandos principales de Linux.

## Sugerencia de recorrido

1. Comenzar con [`Sistemas Operativos.md`](Teoricos/Sistemas%20Operativos.md).
2. Continuar con [`Linux.md`](Teoricos/Linux.md) y [`linux_commands.md`](Teoricos/linux_commands.md).
3. Realizar [`práctica1.md`](Actividades/pr%C3%A1ctica1.md).
4. Trabajar virtualización con [`virtual.md`](Teoricos/virtual.md), [`virtualbox.md`](Teoricos/virtualbox.md) y [`guia_virtualbox_configuracion_cli.md`](Actividades/guia_virtualbox_configuracion_cli.md).
5. Instalar Ubuntu Server usando la actividad en Markdown o el tutorial interactivo HTML.
6. Profundizar configuración de red con [`netplan.md`](Teoricos/netplan.md).

## Notas

- Los archivos `.md` pueden leerse directamente en GitHub, GitLab, VS Code u otro visor Markdown.
- Los archivos `.html` son recursos interactivos y pueden abrirse directamente en un navegador.
- Este repositorio está pensado como material vivo de clase, por lo que puede crecer o reorganizarse durante el curso.
