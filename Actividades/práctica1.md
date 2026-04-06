# Práctica 1: Introducción a la terminal de Linux

## Objetivo

En esta práctica los estudiantes utilizarán la terminal de Linux para:

- moverse entre carpetas
- listar archivos y directorios
- visualizar archivos de configuración del sistema
- observar procesos en ejecución
- reconocer comandos básicos de uso frecuente

## Requisitos

- una computadora con Linux
- acceso a una terminal
- permisos de usuario normales

## Parte 1: Abrir la terminal

Abrir una terminal desde el entorno gráfico del sistema.

En Ubuntu, por ejemplo, se puede abrir con:

```bash
Ctrl + Alt + T
```

## Parte 2: Identificar la ubicación actual

Usar el siguiente comando:

```bash
pwd
```

Este comando muestra la carpeta actual en la que se encuentra el usuario.

## Parte 3: Ver archivos y carpetas

### Listado básico

```bash
ls
```

### Listado detallado

```bash
ls -l
```

### Ver también archivos ocultos

```bash
ls -la
```

## Parte 4: Moverse entre carpetas

### Entrar en una carpeta

```bash
cd Documentos
```

### Volver a la carpeta anterior

```bash
cd ..
```

### Ir al directorio personal

```bash
cd ~
```

### Ir a la raíz del sistema

```bash
cd /
```

## Parte 5: Explorar directorios importantes del sistema

Observar el contenido de los siguientes directorios:

```bash
ls /
ls /etc
ls /home
ls /var
ls /usr
```

### Explicación breve

- `/`: raíz del sistema de archivos
- `/etc`: archivos de configuración del sistema
- `/home`: carpetas personales de los usuarios
- `/var`: datos variables, registros y colas
- `/usr`: programas y recursos del sistema

## Parte 6: Ver archivos de configuración importantes

Visualizar el contenido de algunos archivos con `cat` o `less`.

### Ver usuarios del sistema

```bash
cat /etc/passwd
```

### Ver grupos del sistema

```bash
cat /etc/group
```

### Ver nombres de host configurados

```bash
cat /etc/hostname
```

### Ver información de resolución local

```bash
cat /etc/hosts
```

### Ver el archivo de sistema operativo

```bash
cat /etc/os-release
```

### Ver archivos largos de forma cómoda

```bash
less /etc/services
```

Para salir de `less`, presionar:

```text
q
```

## Parte 7: Crear y manipular archivos simples

### Crear una carpeta de prueba

```bash
mkdir practica_terminal
```

### Entrar en la carpeta creada

```bash
cd practica_terminal
```

### Crear un archivo vacío

```bash
touch ejemplo.txt
```

### Verificar que fue creado

```bash
ls -l
```

### Mostrar el contenido del archivo

```bash
cat ejemplo.txt
```

## Parte 8: Copiar, mover y eliminar archivos

### Copiar un archivo

```bash
cp ejemplo.txt copia.txt
```

### Renombrar un archivo

```bash
mv copia.txt copia_renombrada.txt
```

### Eliminar un archivo

```bash
rm copia_renombrada.txt
```

### Salir de la carpeta y eliminarla

```bash
cd ..
rm -r practica_terminal
```

## Parte 9: Ver procesos en ejecución

### Mostrar procesos del usuario actual

```bash
ps
```

### Mostrar todos los procesos con detalle

```bash
ps aux
```

### Ver procesos en tiempo real

```bash
top
```

Para salir de `top`, presionar:

```text
q
```

## Parte 10: Buscar ayuda sobre comandos

### Manual de un comando

```bash
man ls
```

### Ayuda breve de un comando

```bash
ls --help
```

## Actividades propuestas

1. Ejecutar `pwd` y anotar la ruta mostrada.
2. Ingresar a `/etc` y listar su contenido.
3. Visualizar el archivo `/etc/hostname`.
4. Visualizar el archivo `/etc/passwd` e identificar nombres de usuarios.
5. Crear una carpeta de prueba dentro del directorio personal.
6. Crear un archivo vacío dentro de esa carpeta.
7. Copiar y renombrar ese archivo.
8. Mostrar los procesos con `ps aux`.
9. Ejecutar `top` durante unos segundos y salir.
10. Consultar el manual de `cd`, `ls` o `ps`.

## Preguntas para responder

1. ¿Qué función cumple el directorio `/etc`?
2. ¿Cuál es la diferencia entre `ls`, `ls -l` y `ls -la`?
3. ¿Qué muestra el comando `pwd`?
4. ¿Para qué sirve `ps aux`?
5. ¿Qué diferencia hay entre `cat` y `less`?
6. ¿Qué precaución debe tomarse al usar `rm`?

## Cierre

Esta práctica introduce el uso básico de la terminal de Linux, una herramienta fundamental para la administración y comprensión del sistema operativo. Dominar estos comandos permite explorar el sistema, interpretar su organización y trabajar con mayor precisión en entornos Unix y GNU/Linux.
