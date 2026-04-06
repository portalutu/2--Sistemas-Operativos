# Sistemas Operativos

## Historia, conceptos fundamentales y arquitectura

Este documento sirve como material de apoyo para clases introductorias sobre sistemas operativos. El objetivo es comprender cómo surgieron, cómo evolucionaron y cuál es su papel dentro de los sistemas informáticos modernos.

---

# 1. Evolución histórica de los sistemas operativos

Los sistemas operativos no siempre existieron como los conocemos hoy. En las primeras computadoras, los usuarios interactuaban directamente con el hardware.

## Primera generación (1940–1955)

Las primeras computadoras eran máquinas enormes utilizadas principalmente para cálculos científicos y militares.

Características:

* No existían sistemas operativos
* Los programas se cargaban manualmente
* Se utilizaban tarjetas perforadas
* Solo se ejecutaba un programa a la vez

Ejemplo de máquinas:

* ENIAC
* UNIVAC

Los operadores debían configurar físicamente la máquina para cada programa.

---

## Segunda generación (1955–1965)

Aparecen los primeros sistemas de procesamiento por lotes (**batch processing**).

Características:

* Los programas se agrupaban en lotes
* El operador cargaba varios trabajos
* El sistema los ejecutaba automáticamente

Esto permitió mejorar el uso del tiempo de la computadora.

Ejemplo de sistemas:

* IBM OS/360

---

## Tercera generación (1965–1980)

Surgen los sistemas operativos multitarea y los sistemas de tiempo compartido.

Características:

* múltiples usuarios
* múltiples programas ejecutándose
* mejor administración de recursos

Conceptos introducidos:

* multitarea
* memoria virtual
* sistemas interactivos

Ejemplos:

* Multics
* Unix

---

## Cuarta generación (1980–actualidad)

Los sistemas operativos se vuelven más complejos y orientados al usuario.

Características:

* interfaces gráficas
* redes
* seguridad
* virtualización

Ejemplos:

* Windows
* Linux
* macOS

---

# 2. Surgimiento de Unix

Unix fue creado en 1969 en los laboratorios **Bell Labs** por **Ken Thompson** y **Dennis Ritchie**.

Unix introdujo ideas revolucionarias que aún hoy influyen en los sistemas operativos.

Principios de diseño de Unix:

* simplicidad
* modularidad
* reutilización

Una característica fundamental de Unix es la filosofía de crear programas pequeños que realizan una sola tarea, pero que pueden combinarse entre sí.

Ejemplo:

```
cat archivo.txt | grep palabra
```

Aquí se combinan dos programas:

* `cat` muestra el contenido del archivo
* `grep` busca texto dentro del archivo

Este concepto se llama **pipes**.

Unix también introdujo:

* sistema de archivos jerárquico
* herramientas de línea de comandos
* portabilidad

---

# 3. Creación de Linux

Linux fue creado en 1991 por **Linus Torvalds**, un estudiante de informática de la Universidad de Helsinki.

Torvalds desarrolló inicialmente el kernel de Linux como un proyecto personal inspirado en **Minix**, un sistema educativo basado en Unix.

Publicó el código en Internet permitiendo que otros desarrolladores colaboraran.

Esto dio origen a una comunidad global de desarrollo.

Linux hoy es utilizado en:

* servidores
* supercomputadoras
* teléfonos Android
* dispositivos embebidos

Ejemplos de distribuciones Linux:

* Ubuntu
* Debian
* Fedora
* Arch Linux

---

# 4. Filosofía del software libre

El movimiento de software libre fue impulsado por **Richard Stallman** en la década de 1980.

Stallman fundó el proyecto **GNU** y la **Free Software Foundation**.

El software libre se basa en cuatro libertades fundamentales.

## Las cuatro libertades del software libre

1. Libertad de ejecutar el programa para cualquier propósito
2. Libertad de estudiar cómo funciona el programa
3. Libertad de modificar el programa
4. Libertad de distribuir copias

Esto es posible porque el código fuente está disponible.

Linux utiliza muchas herramientas del proyecto GNU, por lo que frecuentemente se denomina **GNU/Linux**.

---

# 5. Concepto de sistema operativo

Un sistema operativo es el software que administra los recursos de una computadora y permite la interacción entre el usuario y el hardware.

Funciones principales:

* administración del procesador
* administración de memoria
* administración de dispositivos
* gestión de archivos
* seguridad

Sin un sistema operativo sería extremadamente difícil utilizar una computadora.

Ejemplo de interacción:

Usuario → Sistema Operativo → Hardware

---

# 6. Arquitectura de un sistema operativo

Un sistema operativo está compuesto por diferentes componentes que trabajan juntos.

## Kernel

El kernel es el núcleo del sistema operativo.

Se encarga de:

* controlar el hardware
* administrar memoria
* gestionar procesos

---

## Gestión de procesos

La gestión de procesos es una de las funciones más importantes del sistema operativo. Permite que múltiples programas se ejecuten al mismo tiempo compartiendo el procesador de forma organizada.

Un **proceso** es un programa que se encuentra en ejecución.

Por ejemplo:

* abrir un navegador web
* ejecutar un editor de texto
* reproducir música

Cada uno de estos programas se ejecuta como un proceso independiente.

### Funciones principales de la gestión de procesos

El sistema operativo debe encargarse de:

* crear procesos
* planificar su ejecución
* suspender o reanudar procesos
* finalizar procesos

### Planificación del procesador (CPU Scheduling)

El procesador solo puede ejecutar una instrucción a la vez. Para permitir que múltiples programas funcionen simultáneamente, el sistema operativo utiliza algoritmos de planificación.

El sistema operativo asigna pequeños intervalos de tiempo a cada proceso.

Este mecanismo se llama **multiprogramación** o **time sharing**.

Ejemplo simplificado:

```
Proceso A → CPU (5 ms)
Proceso B → CPU (5 ms)
Proceso C → CPU (5 ms)
```

Este intercambio ocurre tan rápido que el usuario percibe que todos los programas funcionan al mismo tiempo.

### Estados de un proceso

Un proceso puede encontrarse en diferentes estados durante su ejecución.

```
Nuevo → Listo → Ejecutando → Esperando → Finalizado
```

Descripción de cada estado:

* **Nuevo**: el proceso acaba de crearse
* **Listo**: el proceso está preparado para ejecutarse
* **Ejecutando**: el proceso está usando el procesador
* **Esperando**: el proceso espera un recurso (por ejemplo disco o red)
* **Finalizado**: el proceso terminó su ejecución

### Identificación de procesos

Cada proceso posee un identificador único llamado **PID (Process ID)**.

Ejemplo en Linux:

```bash
ps aux
```

Este comando muestra todos los procesos activos en el sistema.

---

## Gestión de memoria

La gestión de memoria se encarga de administrar la **memoria RAM** disponible en el sistema para que múltiples programas puedan ejecutarse al mismo tiempo sin interferir entre sí.

La memoria es un recurso limitado, por lo que el sistema operativo debe distribuirla eficientemente.

### Funciones principales

El sistema operativo debe encargarse de:

* asignar memoria a los programas
* liberar memoria cuando un programa finaliza
* evitar que un programa acceda a la memoria de otro
* optimizar el uso de la memoria disponible

### Espacios de memoria

Cada proceso recibe su propio espacio de memoria.

Esto evita que un programa pueda modificar accidentalmente la memoria de otro programa.

Ejemplo conceptual:

```
Proceso A → Memoria A
Proceso B → Memoria B
Proceso C → Memoria C
```

### Memoria virtual

Una de las técnicas más importantes es la **memoria virtual**.

La memoria virtual permite que el sistema utilice parte del disco como si fuera memoria adicional.

Cuando la memoria RAM se llena, el sistema operativo mueve temporalmente datos al disco.

Este mecanismo se conoce como **swap**.

Ejemplo:

```
RAM llena
↓
Sistema mueve datos al disco (swap)
↓
RAM queda disponible para nuevos procesos
```

Aunque este proceso es más lento que usar RAM, permite que el sistema continúe funcionando cuando hay muchos programas abiertos.

### Paginación

Muchos sistemas operativos utilizan un mecanismo llamado **paginación**.

La memoria se divide en bloques pequeños llamados **páginas**.

Esto permite:

* asignar memoria de forma más eficiente
* reducir fragmentación
* facilitar el uso de memoria virtual

---

## Usuarios, root y sudo en Linux

En Linux no todos los usuarios tienen el mismo nivel de acceso.

Normalmente se trabaja con un **usuario común**, que puede utilizar programas, crear archivos y trabajar en su espacio personal.

Existe también un usuario especial llamado **root**.

El usuario `root` es el administrador del sistema y tiene control total sobre la computadora.

Puede, por ejemplo:

* instalar programas
* crear y eliminar usuarios
* modificar configuraciones del sistema
* acceder a archivos protegidos

Por esa razón, no se recomienda trabajar siempre como `root`, ya que un error puede afectar todo el sistema.

### Uso de sudo

Para ejecutar una tarea administrativa sin iniciar sesión como `root`, en muchos sistemas Linux se utiliza el comando `sudo`.

`sudo` significa aproximadamente "superuser do".

Permite ejecutar un comando con privilegios de administrador.

Ejemplo:

```bash
sudo apt update
```

En este caso, un usuario común ejecuta una tarea administrativa de manera controlada.

Otro ejemplo:

```bash
sudo systemctl restart ssh
```

Aquí se reinicia un servicio del sistema, acción que normalmente requiere permisos elevados.

### Ejemplos de administración de usuarios

Para crear un usuario nuevo, puede utilizarse:

```bash
sudo useradd -m juan
```

Este comando crea el usuario `juan` y genera su directorio personal.

En muchos sistemas también es común utilizar:

```bash
sudo adduser juan
```

Este comando suele guiar el proceso paso a paso y pedir información adicional.

Para cambiar la contraseña de un usuario:

```bash
sudo passwd juan
```

El sistema solicitará ingresar y confirmar la nueva contraseña.

Si un usuario quiere cambiar su propia contraseña, puede usar:

```bash
passwd
```

Para eliminar un usuario:

```bash
sudo userdel juan
```

Si además se desea eliminar su directorio personal:

```bash
sudo userdel -r juan
```

### Resumen práctico

* un usuario común trabaja con permisos limitados
* `root` tiene control total del sistema
* `sudo` permite ejecutar tareas administrativas de forma puntual

Este modelo ayuda a mejorar la seguridad y evita cambios accidentales en partes críticas del sistema.

---

## Sistema de archivos

El sistema de archivos define cómo se almacenan, organizan y recuperan los datos en los dispositivos de almacenamiento, como discos duros o unidades SSD.

Sin un sistema de archivos, los datos se guardarían como una secuencia desordenada de bits en el disco.

El sistema de archivos permite organizar la información de manera lógica y estructurada.

### Funciones principales

Un sistema de archivos se encarga de:

* organizar archivos y directorios
* almacenar datos de forma persistente
* administrar permisos de acceso
* gestionar el espacio disponible en el disco

### Archivos y directorios

Los datos se organizan en **archivos**.

Los archivos se agrupan dentro de **directorios** (carpetas).

Ejemplo de estructura en Linux:

```
/
/home
/home/usuario
/home/usuario/documentos
/home/usuario/descargas
/etc
/var
/usr
```

Cada directorio puede contener:

* archivos
* otros directorios

Esto genera una estructura jerárquica en forma de árbol.

### Metadatos

Cada archivo posee información adicional llamada **metadatos**.

Ejemplos de metadatos:

* nombre del archivo
* tamaño
* fecha de creación
* permisos de acceso
* propietario

En Linux se pueden ver utilizando:

```bash
ls -l
```

### Tipos de sistemas de archivos

Existen diferentes sistemas de archivos dependiendo del sistema operativo.

Ejemplos:

| Sistema | Uso                                  |
| ------- | ------------------------------------ |
| FAT32   | sistemas antiguos y dispositivos USB |
| NTFS    | Windows                              |
| ext4    | Linux                                |
| APFS    | macOS                                |

Cada sistema de archivos tiene diferentes características de rendimiento, seguridad y capacidad.

### Cómo guarda la información un sistema de archivos

Cuando un archivo se guarda en un disco, el sistema operativo no lo coloca "todo junto" de manera arbitraria.

El sistema de archivos divide el espacio en unidades y registra en qué parte del disco se encuentra cada archivo.

De esta forma, cuando el usuario abre un documento, una imagen o un programa, el sistema operativo puede localizarlo correctamente.

Ejemplo conceptual:

```
Archivo: tarea.docx
Bloque 1 → sector del disco A
Bloque 2 → sector del disco B
Bloque 3 → sector del disco C
```

Aunque para el usuario aparece como un único archivo, internamente puede estar distribuido en distintos bloques del disco.

### Formateo

Antes de utilizar una partición o una unidad de almacenamiento, normalmente es necesario **formatearla**.

Formatear significa preparar ese espacio para trabajar con un sistema de archivos específico.

Por ejemplo:

* un pendrive puede formatearse en FAT32 o exFAT
* una partición de Linux puede formatearse en ext4
* una partición de Windows puede formatearse en NTFS

Sin este proceso, el sistema operativo no tendría una estructura clara para guardar carpetas, archivos y metadatos.

### Particionamiento de discos

Un disco puede dividirse en varias partes lógicas llamadas **particiones**.

Cada partición funciona como si fuera una unidad independiente.

Esto permite:

* separar datos del sistema operativo
* instalar más de un sistema operativo en el mismo disco
* organizar mejor la información
* aplicar distintos sistemas de archivos según la necesidad

Ejemplo:

Un disco de 1 TB podría dividirse así:

* Partición 1: 200 GB para Windows
* Partición 2: 300 GB para Linux
* Partición 3: 500 GB para archivos personales

En ese caso, cada partición puede tener su propio sistema de archivos.

Por ejemplo:

* la partición de Windows en NTFS
* la partición de Linux en ext4
* la partición de intercambio o datos en otro formato según el uso

### Ejemplo de particionamiento en la práctica

En una instalación típica de Linux pueden aparecer particiones como estas:

```
/dev/sda1   EFI
/dev/sda2   /
/dev/sda3   /home
/dev/sda4   swap
```

Interpretación:

* `EFI` almacena archivos necesarios para el arranque
* `/` contiene el sistema principal
* `/home` guarda los archivos de los usuarios
* `swap` se utiliza como apoyo de memoria virtual

Esta separación tiene ventajas importantes.

Por ejemplo, si se reinstala el sistema operativo, muchas veces es posible conservar la partición `/home` y mantener los archivos personales.

### Comandos de ejemplo para particionar y formatear

En Linux existen herramientas de línea de comandos que permiten crear particiones y preparar sistemas de archivos.

Algunos ejemplos comunes son:

```bash
sudo fdisk /dev/sda
```

Este comando permite administrar las particiones de un disco.

Dentro de `fdisk` pueden utilizarse opciones como:

* `n` para crear una nueva partición
* `d` para eliminar una partición
* `p` para mostrar la tabla de particiones
* `w` para guardar los cambios

Luego de crear una partición, puede formatearse con comandos como estos:

```bash
sudo mkfs.ext4 /dev/sda2
sudo mkfs.ext4 /dev/sda3
sudo mkswap /dev/sda4
```

En este ejemplo:

* `/dev/sda2` se prepara con sistema de archivos `ext4`
* `/dev/sda3` también se prepara con `ext4`
* `/dev/sda4` se configura como área de intercambio `swap`

Para comprobar el resultado, puede utilizarse:

```bash
lsblk
sudo fdisk -l
```

Estos comandos muestran los discos, sus particiones y, en muchos casos, el tipo de sistema de archivos detectado.

### Relación entre partición y sistema de archivos

Es importante no confundir estos dos conceptos:

* la **partición** es una división lógica del disco
* el **sistema de archivos** es la forma en que se organiza la información dentro de esa partición

Ejemplo sencillo:

* disco físico: un SSD de 500 GB
* particiones: dos partes de 250 GB
* sistema de archivos: una en NTFS y otra en ext4

Es decir, primero puede particionarse el disco y luego formatearse cada partición con el sistema de archivos que corresponda.

### Ejemplos practicos:

Caso 1:

Una computadora de laboratorio tiene Windows instalado y se agrega Linux para prácticas.

En ese escenario:

* una partición puede quedar para Windows
* otra partición para Linux
* otra para compartir archivos entre ambos sistemas

Caso 2:

Un usuario utiliza un pendrive para llevar trabajos entre distintas computadoras.

Si necesita compatibilidad con muchos equipos, puede resultar conveniente usar FAT32 o exFAT.

Caso 3:

En un servidor, puede separarse el sistema operativo de los datos de los usuarios para facilitar la administración y las copias de seguridad.

### Resumen conceptual

Un disco almacena información físicamente.

Las particiones permiten dividir ese disco en áreas lógicas.

El sistema de archivos organiza cómo se guardan y recuperan los datos dentro de cada partición.

Gracias a esto, el sistema operativo puede crear carpetas, guardar archivos, controlar permisos y localizar la información cuando el usuario la necesita.

### Permisos de archivos

En sistemas Unix y Linux, cada archivo posee permisos que determinan quién puede acceder o modificarlo.

Ejemplo:

```
-rwxr-xr-- archivo.txt
```

Estos permisos indican:

* lectura
* escritura
* ejecución

Esto permite mejorar la seguridad del sistema.

---

## Controladores de dispositivos

Los controladores (**drivers**) permiten que el sistema operativo se comunique con el hardware.

Ejemplos:

* teclado
* disco
* tarjeta de red

---

# 7. Sistemas operativos de cliente y de servidor

Los sistemas operativos pueden clasificarse según su propósito.

---

## Sistemas operativos de cliente

Están diseñados para ser utilizados directamente por usuarios finales.

Características:

* interfaz gráfica
* facilidad de uso
* soporte multimedia

Ejemplos:

* Windows 11
* macOS
* Ubuntu Desktop

---

## Sistemas operativos de servidor

Están diseñados para gestionar servicios en red y múltiples usuarios.

Características:

* alta estabilidad
* seguridad
* administración remota
* soporte para múltiples servicios

Ejemplos:

* Linux Server
* Windows Server
* FreeBSD

Servicios comunes en servidores:

* servidores web
* servidores de bases de datos
* servidores de archivos
* servidores de correo

---

# Conclusión

Los sistemas operativos son uno de los componentes fundamentales de cualquier sistema informático. Comprender su evolución histórica, arquitectura y diferentes tipos permite entender mejor cómo funcionan las computadoras modernas.

Linux y la filosofía del software libre han tenido un impacto profundo en la informática actual, impulsando el desarrollo colaborativo y la innovación tecnológica.
