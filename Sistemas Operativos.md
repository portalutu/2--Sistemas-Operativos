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
