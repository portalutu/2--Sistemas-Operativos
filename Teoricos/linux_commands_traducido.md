# Comandos de Linux

## Sistema Operativo Linux

### Línea de comandos

**Solo con fines didácticos**

Diego Vera  
(SA/SMC/ISE)

---

# OBTENER INFORMACIÓN

## INFORMACIÓN DEL SISTEMA

```bash
# Mostrar información del sistema Linux
uname -a

# Mostrar información de la versión del kernel
uname -r

# Mostrar información del sistema operativo, como el nombre de la distribución y la versión
cat /etc/os-release

# Mostrar cuánto tiempo ha estado funcionando el sistema + carga
uptime

# Mostrar el nombre del host del sistema
hostname

# Mostrar todas las direcciones IP locales del host
hostname -I

# Mostrar historial de reinicios del sistema
last reboot

# Mostrar la fecha y hora actuales
date

# Mostrar el calendario del mes actual
cal

# Mostrar quién está en línea
w

# Mostrar con qué usuario has iniciado sesión
whoami
```

## REGISTRO Y AUDITORÍA

```bash
# Mostrar mensajes en el búfer circular del kernel
dmesg

# Mostrar registros almacenados en el journal de systemd
journalctl

# Mostrar registros de una unidad (servicio) específica
journalctl -u nombre_del_servicio
```

## INFORMACIÓN DE HARDWARE

```bash
# Mostrar mensajes en el búfer circular del kernel
dmesg

# Mostrar información de la CPU
cat /proc/cpuinfo

# Mostrar información de memoria
cat /proc/meminfo

# Mostrar memoria libre y usada (-h para formato legible, -m para MB, -g para GB)
free -h

# Mostrar dispositivos PCI
lspci -tv

# Mostrar dispositivos USB
lsusb -tv

# Mostrar información DMI/SMBIOS (hardware) desde la BIOS
dmidecode

# Mostrar información sobre el disco sda
hdparm -i /dev/sda

# Realizar una prueba de velocidad de lectura sobre el disco sda
hdparm -tT /dev/sda

# Probar bloques ilegibles en el disco sda
badblocks -s /dev/sda
```

## MONITOREO DE RENDIMIENTO Y ESTADÍSTICAS

```bash
# Mostrar y gestionar los procesos principales
top

# Visor interactivo de procesos (alternativa a top)
htop

# Mostrar estadísticas relacionadas con el procesador
mpstat 1

# Mostrar estadísticas de memoria virtual
vmstat 1

# Mostrar estadísticas de E/S
iostat 1

# Mostrar los últimos 100 mensajes de syslog
# (Usar /var/log/syslog en sistemas basados en Debian)
tail -100 /var/log/messages

# Capturar y mostrar todos los paquetes en la interfaz eth0
tcpdump -i eth0

# Monitorear todo el tráfico del puerto 80 (HTTP)
tcpdump -i eth0 'port 80'

# Listar todos los archivos abiertos en el sistema
lsof

# Listar archivos abiertos por un usuario
lsof -u usuario

# Mostrar memoria libre y usada
free -h

# Ejecutar "df -h" mostrando actualizaciones periódicas
watch df -h
```

## INFORMACIÓN Y GESTIÓN DE USUARIOS

```bash
# Mostrar los IDs de usuario y grupo del usuario actual
id

# Mostrar los últimos usuarios que iniciaron sesión en el sistema
last

# Mostrar quién está conectado al sistema
who

# Mostrar quién está conectado y qué está haciendo
w

# Crear un grupo llamado "test"
groupadd test

# Crear una cuenta llamada john, con comentario "John Smith" y crear su directorio personal
useradd -c "John Smith" -m john

# Eliminar la cuenta john
userdel john

# Agregar la cuenta john al grupo sales
usermod -aG sales john
```

---

# ALMACENAMIENTO

## USO DE DISCO

```bash
# Mostrar espacio libre y usado en sistemas de archivos montados
df -h

# Mostrar inodos libres y usados en sistemas de archivos montados
df -i

# Mostrar tamaños y tipos de particiones de disco
fdisk -l

# Mostrar uso de disco de todos los archivos y directorios en formato legible
DU -ah

# Mostrar el uso total de disco del directorio actual
du -sh
```

## COMANDOS DE ARCHIVOS Y DIRECTORIOS

```bash
# Listar todos los archivos en formato largo (detallado)
ls -al

# Mostrar el directorio de trabajo actual
pwd

# Crear un directorio
mkdir directorio

# Eliminar archivo
rm archivo

# Eliminar el directorio y su contenido recursivamente
rm -r directorio

# Forzar la eliminación de un archivo sin pedir confirmación
rm -f archivo

# Forzar la eliminación recursiva de un directorio
rm -rf directorio

# Copiar archivo1 a archivo2
cp archivo1 archivo2

# Copiar source_directory recursivamente a destination
cp -r directorio_origen destino

# Renombrar o mover archivo1 a archivo2
mv archivo1 archivo2

# Crear un enlace simbólico a linkname
ln -s /ruta/al/archivo nombre_enlace

# Crear un archivo vacío o actualizar las fechas de acceso y modificación
touch archivo

# Ver el contenido de un archivo
cat archivo

# Navegar dentro de un archivo de texto
less archivo

# Mostrar las primeras 10 líneas de un archivo
head archivo

# Mostrar las últimas 10 líneas de un archivo
tail archivo

# Mostrar las últimas 10 líneas y seguir el crecimiento del archivo
tail -f archivo
```

## NAVEGACIÓN DE DIRECTORIOS

```bash
# Subir un nivel en el árbol de directorios
cd ..

# Ir al directorio $HOME
cd ~

# Cambiar al directorio /etc
cd /etc

# Cambiar al directorio raíz
cd /
```

## ARCHIVOS COMPRIMIDOS (TAR)

```bash
# Crear archive.tar que contenga directorio
tar cf archive.tar directorio

# Extraer el contenido de archive.tar
tar xf archive.tar

# Crear un archivo tar comprimido con gzip llamado archive.tar.gz
tar czf archive.tar.gz directorio

# Extraer un archivo tar comprimido con gzip
tar xzf archive.tar.gz

# Crear un archivo tar con compresión bzip2
tar cjf archive.tar.bz2 directorio

# Extraer un archivo tar comprimido con bzip2
tar xjf archive.tar.bz2
```

## BÚSQUEDA

```bash
# Buscar un patrón dentro de un archivo
grep patron archivo

# Buscar recursivamente un patrón dentro de un directorio
grep -r patron directorio

# Encontrar archivos y directorios por nombre
locate nombre

# Encontrar archivos en /home/john que comienzan con "prefix"
find /home/john -name 'prefix*'

# Encontrar archivos mayores a 100MB en /home
find /home -size +100M
```

## PERMISOS DE ARCHIVOS

### Ejemplos de `chmod`

| Permisos | Ejemplo |
|---|---|
| `rwx rwx rwx` | `chmod 777 filename` |
| `rwx rwx r-x` | `chmod 775 filename` |
| `rwx r-x r-x` | `chmod 755 filename` |
| `rw- rw- r--` | `chmod 664 filename` |
| `rw- r-- r--` | `chmod 644 filename` |

**Nota:** `777` = acceso total sin restricciones.

### Leyenda

- **U** = Usuario
- **G** = Grupo
- **W** = Mundo / Otros
- **r** = lectura
- **w** = escritura
- **x** = ejecución
- **-** = sin acceso

---

# ADMINISTRACIÓN DEL SISTEMA

## GESTIÓN DE PROCESOS

```bash
# Mostrar los procesos que estás ejecutando actualmente
ps

# Mostrar todos los procesos que se están ejecutando en el sistema
ps -ef

# Mostrar información de un proceso por nombre
ps -ef | grep nombre_del_proceso

# Mostrar y gestionar los procesos principales
top

# Visor interactivo de procesos (alternativa a top)
htop

# Finalizar el proceso con ID pid
kill pid

# Finalizar todos los procesos con el nombre processname
killall nombre_del_proceso

# Iniciar programa en segundo plano
programa &

# Mostrar trabajos detenidos o en segundo plano
bg

# Llevar al primer plano el trabajo más reciente en segundo plano
fg

# Llevar el trabajo n al primer plano
fg n
```

## REDES

```bash
# Mostrar todas las interfaces de red y direcciones IP
ip a

# Mostrar dirección y detalles de eth0
ip addr show dev eth0

# Consultar o controlar el driver de red y ajustes de hardware
ethtool eth0

# Enviar solicitud ICMP echo a un host
ping host

# Mostrar información whois de un dominio
whois dominio

# Mostrar información DNS de un dominio
dig dominio

# Búsqueda inversa de IP
dig -x DIRECCION_IP

# Mostrar la IP DNS de un dominio
host dominio

# Mostrar la dirección de red del nombre del host
hostname -i

# Mostrar todas las direcciones IP locales del host
hostname -I

# Descargar archivo desde una URL
wget http://dominio.com/archivo

# Mostrar puertos TCP y UDP en escucha y sus programas
netstat -nutlp
```

## COMANDO `ip`

### Consultas IP

| Subcomando | Descripción | Ejemplo |
|---|---|---|
| `addr` | Mostrar direcciones IP e información de propiedades | `ip addr` |
| `link` | Gestionar y mostrar el estado de interfaces de red | `ip link` |
| `route` | Mostrar y modificar la tabla de ruteo | `ip route` |
| `maddr` | Gestionar y mostrar direcciones IP multicast | `ip maddr` |
| `neigh` | Mostrar objetos vecinos; tabla ARP en IPv4 | `ip neigh` |
| `help` | Mostrar lista de comandos y argumentos | `ip help` |

### Direccionamiento Multicast

| Subcomando | Descripción | Ejemplo |
|---|---|---|
| `maddr add` | Agregar una dirección multicast estática de capa de enlace | `ip maddr add 33:33:00:00:00:01 dev em1` |
| `maddr del` | Eliminar una dirección multicast | `ip maddr del 33:33:00:00:00:01 dev em1` |

### Modificación de direcciones y enlaces

| Subcomando | Descripción | Ejemplo |
|---|---|---|
| `addr add` | Agregar una dirección | `ip addr add 192.168.1.1/24 dev em1` |
| `addr del` | Eliminar una dirección | `ip addr del 192.168.1.1/24 dev em1` |
| `link set` | Alterar el estado de la interfaz | `ip link set em1 up` |
| `link set` | Bajar interfaz | `ip link set em1 down` |
| `link set` | Configurar MTU | `ip link set em1 mtu 9000` |
| `link set` | Habilitar modo promiscuo | `ip link set em1 promisc on` |

### Ajuste y visualización de rutas

| Subcomando | Descripción | Ejemplo |
|---|---|---|
| `route add` | Agregar una entrada a la tabla de ruteo | `ip route add default via 192.168.1.1 dev em1` |
| `route delete` | Eliminar una entrada de la tabla de ruteo | `ip route delete 192.168.1.0/24 via 192.168.1.1` |
| `route replace` | Reemplazar o agregar una ruta | `ip route replace 192.168.1.0/24 dev em1` |
| `route get` | Mostrar la ruta que tomará una dirección | `ip route get 192.168.1.5` |

### Gestión de la tabla ARP

| Subcomando | Descripción | Ejemplo |
|---|---|---|
| `neigh add` | Agregar una entrada a la tabla ARP | `ip neigh add 192.168.1.1 lladdr 1:2:3:4:5:6 dev em1` |
| `neigh del` | Invalidar una entrada | `ip neigh del 192.168.1.1 dev em1` |
| `neigh replace` | Reemplazar o agregar una entrada en la tabla ARP | `ip neigh replace 192.168.1.1 lladdr 1:2:3:4:5:6 dev em1` |

### Comandos útiles de red

| Comando | Descripción | Ejemplo |
|---|---|---|
| `arping` | Enviar una solicitud ARP a un host vecino | `arping -I eth0 192.168.1.1` |
| `ethtool` | Consultar o controlar el driver y ajustes de red | `ethtool -i eth0` |
| `ss` | Mostrar estadísticas de sockets | `ss -a` |

### `net-tools` vs `ip`

| `net-tools` | `ip` |
|---|---|
| `arp -a` | `ip neigh` |
| `arp -v` | `ip -s neigh` |
| `arp -s 192.168.1.1 1:2:3:4:5:6` | `ip neigh add 192.168.1.1 lladdr 1:2:3:4:5:6 dev eth1` |
| `arp -i eth1 -d 192.168.1.1` | `ip neigh del 192.168.1.1 dev eth1` |
| `ifconfig -a` | `ip addr` |
| `ifconfig eth0 down` | `ip link set eth0 down` |
| `ifconfig eth0 up` | `ip link set eth0 up` |
| `ifconfig eth0 192.168.1.1` | `ip addr add 192.168.1.1/24 dev eth0` |
| `ifconfig eth0 netmask 255.255.255.0` | `ip addr add 192.168.1.1/24 dev eth0` |
| `ifconfig eth0 mtu 9000` | `ip link set eth0 mtu 9000` |
| `ifconfig eth0:0 192.168.1.2` | `ip addr add 192.168.1.2/24 dev eth0` |
| `netstat` | `ss` |
| `netstat -neopa` | `ss -neopa` |
| `netstat -g` | `ip maddr` |
| `route` | `ip route` |
| `route add -net 192.168.1.0 netmask 255.255.255.0 dev eth0` | `ip route add 192.168.1.0/24 dev eth0` |
| `route add default gw 192.168.1.1` | `ip route add default via 192.168.1.1` |

---

# HERRAMIENTAS DE LÍNEA DE COMANDOS

## Nano

### Manejo de archivos

- `Ctrl+S`: guardar archivo actual
- `Ctrl+O`: ofrecer guardar como
- `Ctrl+R`: insertar un archivo en el actual
- `Ctrl+X`: cerrar búfer y salir de nano

### Edición

- `Ctrl+K`: cortar la línea actual al búfer de corte
- `Alt+6`: copiar la línea actual al búfer de corte
- `Ctrl+U`: pegar el contenido del búfer de corte
- `Alt+T`: cortar hasta el final del búfer
- `Ctrl+]`: completar la palabra actual
- `Alt+3`: comentar o descomentar línea/región
- `Alt+U`: deshacer última acción
- `Alt+E`: rehacer última acción deshecha

### Buscar y reemplazar

- `Ctrl+Q`: búsqueda hacia atrás
- `Ctrl+W`: búsqueda hacia adelante
- `Alt+Q`: buscar siguiente coincidencia hacia atrás
- `Alt+W`: buscar siguiente coincidencia hacia adelante
- `Alt+R`: iniciar reemplazo

### Eliminación

- `Ctrl+H`: eliminar carácter antes del cursor
- `Ctrl+D`: eliminar carácter bajo el cursor
- `Alt+Bsp`: eliminar palabra a la izquierda
- `Ctrl+Del`: eliminar palabra a la derecha
- `Alt+Del`: eliminar línea actual

### Operaciones

- `Ctrl+T`: ejecutar algún comando
- `Ctrl+J`: justificar párrafo o región
- `Alt+J`: justificar todo el búfer
- `Alt+B`: ejecutar una comprobación de sintaxis
- `Alt+F`: ejecutar formateador/corrector/organizador
- `Alt+:`: iniciar/detener grabación de macro
- `Alt+;`: reproducir macro

### Movimiento

- `Ctrl+B`: un carácter atrás
- `Ctrl+F`: un carácter adelante
- `Ctrl+←`: una palabra atrás
- `Ctrl+→`: una palabra adelante
- `Ctrl+A`: ir al inicio de la línea
- `Ctrl+E`: ir al final de la línea
- `Ctrl+P`: una línea arriba
- `Ctrl+N`: una línea abajo
- `Ctrl+↑`: bloque anterior
- `Ctrl+↓`: bloque siguiente
- `Ctrl+Y`: una página arriba
- `Ctrl+V`: una página abajo
- `Alt+\`: ir al inicio del búfer
- `Alt+/`: ir al final del búfer

### Movimiento especial

- `Alt+G`: ir a la línea especificada
- `Alt+]`: ir al corchete complementario
- `Alt+↑`: desplazar vista hacia arriba
- `Alt+↓`: desplazar vista hacia abajo
- `Alt+<`: cambiar al búfer anterior
- `Alt+>`: cambiar al siguiente búfer

### Información

- `Ctrl+C`: informar posición del cursor
- `Alt+D`: informar conteo de líneas/palabras/caracteres
- `Ctrl+G`: mostrar ayuda

### Varios

- `Alt+A`: activar/desactivar selección
- `Tab`: indentar región marcada
- `Shift+Tab`: desindentar región marcada
- `Alt+V`: introducir siguiente pulsación literalmente
- `Alt+N`: activar/desactivar números de línea
- `Alt+P`: activar/desactivar espacios visibles
- `Alt+X`: ocultar o mostrar líneas de ayuda
- `Ctrl+L`: refrescar pantalla

## VIM

### Movimiento del cursor

- `h`: izquierda un carácter
- `l`: derecha un carácter
- `j`: bajar una línea
- `k`: subir una línea
- `w`: derecha una palabra
- `b`: retroceder una palabra
- `$`: ir al final de la línea
- `0`: ir al inicio de la línea
- `)`: derecha una oración
- `(`: izquierda una oración
- `}`: derecha un párrafo
- `{`: izquierda un párrafo
- `Ctrl-F`: avanzar una página
- `Ctrl-B`: retroceder una página
- `G`: ir a (sin argumentos, va al final del archivo)

### Eliminación

- `d`: eliminar
- `d$`: eliminar hasta el final de la línea
- `d0`: eliminar hasta el inicio de la línea
- `d}`: eliminar hasta el final del párrafo
- `dd`: eliminar toda la línea
- `x`: eliminar el carácter bajo el cursor

### Otros comandos básicos

- `r`: reemplazar un carácter
- `ZZ`: guardar y salir
- `y`: copiar al búfer temporal
- `y)`: copiar hasta el final de la oración
- `Y`: copiar la línea actual
- `p`: pegar debajo de la línea actual
- `P`: pegar encima de la línea actual
- `u`: deshacer último comando de edición
- `/texto`: buscar `texto`

Cualquier comando puede tomar un argumento numérico antes del objeto. Ejemplo:

- `5dd`: elimina 5 líneas comenzando desde la lí