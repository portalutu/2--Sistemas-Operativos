# Actividad práctica: administración básica de un sistema Linux

> **Entorno recomendado:** Ubuntu Server o una distribución basada en Debian instalada en una máquina virtual.  
> **Modalidad:** trabajo individual o en parejas.  
> **Duración estimada:** entre 6 y 9 horas.  
> **Requisito:** debes utilizar una cuenta con permisos para ejecutar `sudo`.

---

## 1. Situación de trabajo

Debes preparar un servidor Linux para un pequeño equipo de soporte técnico. El servidor tendrá una estructura ordenada de carpetas, cuentas separadas para cada integrante, un grupo de trabajo, permisos de acceso, configuración de red, registros automáticos y tareas programadas.

Completarás la configuración en etapas. No avances a la etapa siguiente sin comprobar que la anterior funciona correctamente.

> [!CAUTION]
> Realiza esta actividad en una máquina virtual de práctica. Antes de modificar un archivo de configuración, crea siempre una copia de seguridad.

---

## 2. Resultado esperado

Al finalizar, podrás:

- navegar por el sistema de archivos desde la terminal;
- crear, copiar, mover, renombrar y eliminar archivos y carpetas;
- consultar información básica del sistema;
- revisar y configurar los parámetros de red;
- crear cuentas de usuario y grupos;
- asignar propietarios y permisos a directorios compartidos;
- utilizar variables en la terminal;
- crear y ejecutar scripts Bash sencillos;
- programar tareas automáticas mediante `crontab`;
- verificar cada cambio realizado.

---

## 3. Convenciones utilizadas

Durante la actividad encontrarás los siguientes símbolos:

| Símbolo | Significado |
|---|---|
| `$` | Comando que puedes ejecutar con tu cuenta habitual |
| `sudo` | Comando que requiere permisos administrativos |
| `<valor>` | Dato que debes sustituir por el valor real de tu sistema |
| `#` | Comentario dentro de un archivo de configuración o script |

No escribas los signos `<` y `>` cuando sustituyas un valor. Por ejemplo, si tu interfaz se llama `enp0s3`, escribe `enp0s3` y no `<enp0s3>`.

---

# Parte I - Terminal, carpetas y archivos

## 4. Identifica tu sesión y tu ubicación

Abre la terminal y ejecuta:

```bash
whoami
hostname
pwd
```

Interpreta los resultados:

- `whoami` muestra la cuenta con la que has iniciado sesión;
- `hostname` muestra el nombre del equipo;
- `pwd` muestra tu ubicación actual dentro del sistema de archivos.

Consulta el contenido de la carpeta actual:

```bash
ls
ls -l
ls -la
```

Comprueba las diferencias entre las tres salidas. La opción `-l` agrega detalles y la opción `-a` incluye los elementos ocultos.

### Comprobación

Ejecuta:

```bash
echo "Usuario: $USER"
echo "Equipo: $(hostname)"
echo "Ubicación: $(pwd)"
```

Debes obtener tu nombre de usuario, el nombre del equipo y la ruta de tu carpeta actual.

---

## 5. Crea la estructura de trabajo

Dirígete a tu carpeta personal y crea el directorio principal:

```bash
cd "$HOME"
mkdir practica_linux
cd practica_linux
```

Crea varias carpetas en un solo comando:

```bash
mkdir documentos copias scripts logs temporal
```

Crea dos subcarpetas dentro de `documentos`:

```bash
mkdir -p documentos/informes documentos/inventario
```

Visualiza la estructura:

```bash
find . -maxdepth 2 -type d
```

La estructura debe ser equivalente a la siguiente:

```text
practica_linux/
├── copias/
├── documentos/
│   ├── informes/
│   └── inventario/
├── logs/
├── scripts/
└── temporal/
```

### Comprobación

```bash
test -d "$HOME/practica_linux/documentos/informes" && echo "Estructura creada correctamente"
```

---

## 6. Crea y consulta archivos

Crea tres archivos vacíos:

```bash
touch documentos/inventario/equipos.txt
touch documentos/informes/incidencias.txt
touch temporal/notas.txt
```

Agrega contenido mediante redirección:

```bash
echo "Inventario del equipo de soporte" > documentos/inventario/equipos.txt
echo "PC-01 - Ubuntu - Operativo" >> documentos/inventario/equipos.txt
echo "PC-02 - Ubuntu - En mantenimiento" >> documentos/inventario/equipos.txt
```

Observa la diferencia:

- `>` reemplaza el contenido del archivo;
- `>>` agrega contenido al final sin borrar lo anterior.

Consulta el archivo:

```bash
cat documentos/inventario/equipos.txt
less documentos/inventario/equipos.txt
```

Para salir de `less`, presiona `q`.

Edita las notas con un editor de texto:

```bash
nano temporal/notas.txt
```

Escribe al menos dos líneas. En Nano, guarda con `Ctrl+O`, confirma con `Enter` y sal con `Ctrl+X`.

### Comprobación

```bash
wc -l documentos/inventario/equipos.txt
file documentos/inventario/equipos.txt
```

El inventario debe contener tres líneas y ser reconocido como un archivo de texto.

---

## 7. Copia, mueve, renombra y elimina

Copia el inventario:

```bash
cp documentos/inventario/equipos.txt copias/equipos_respaldo.txt
```

Renombra el archivo de notas y muévelo:

```bash
mv temporal/notas.txt documentos/informes/notas_soporte.txt
```

Crea un archivo temporal y elimínalo:

```bash
touch temporal/prueba.tmp
rm temporal/prueba.tmp
```

Elimina la carpeta `temporal` solamente después de comprobar que está vacía:

```bash
ls -la temporal
rmdir temporal
```

> [!IMPORTANT]
> `rm` elimina archivos sin enviarlos a una papelera. Revisa siempre la ruta antes de confirmar un borrado. Evita utilizar `rm -r` o `rm -rf` hasta dominar claramente su alcance.

### Comprobación

```bash
ls -l copias/equipos_respaldo.txt
ls -l documentos/informes/notas_soporte.txt
test ! -d temporal && echo "La carpeta temporal fue eliminada"
```

---

# Parte II - Consulta y configuración del sistema

## 8. Consulta información del sistema operativo

Ejecuta los siguientes comandos:

```bash
hostnamectl
cat /etc/os-release
uname -r
uptime
free -h
df -h
lsblk
```

Registra un resumen en un archivo:

```bash
{
  echo "Fecha: $(date)"
  echo "Equipo: $(hostname)"
  echo "Kernel: $(uname -r)"
  echo "Tiempo encendido: $(uptime -p)"
} > documentos/informes/sistema.txt
```

Comprueba el contenido:

```bash
cat documentos/informes/sistema.txt
```

---

## 9. Cambia el nombre del equipo

Consulta primero el nombre actual:

```bash
hostnamectl hostname
```

Asigna un nombre que identifique al servidor. Utiliza solamente letras minúsculas, números y guiones:

```bash
sudo hostnamectl set-hostname srv-soporte
```

Comprueba el cambio:

```bash
hostnamectl hostname
cat /etc/hostname
```

Si el archivo `/etc/hosts` contiene el nombre anterior, crea una copia y actualízalo:

```bash
sudo cp /etc/hosts /etc/hosts.bak
sudo nano /etc/hosts
```

Mantén la entrada de `localhost` y reemplaza únicamente el nombre anterior cuando corresponda.

---

## 10. Revisa la configuración de red

Identifica las interfaces y las direcciones actuales:

```bash
ip -br link
ip -4 -br address
ip route
```

Consulta la resolución de nombres:

```bash
resolvectl status
```

Prueba la conectividad por dirección IP y luego por nombre:

```bash
ping -c 4 1.1.1.1
ping -c 4 example.com
```

Interpreta las pruebas:

- si responde `1.1.1.1`, existe conectividad IP;
- si también responde `example.com`, la resolución DNS funciona;
- si responde la primera prueba pero falla la segunda, revisa la configuración DNS.

Guarda la información recibida por DHCP antes de realizar cambios:

```bash
ip -4 -br address > documentos/informes/red_antes.txt
ip route >> documentos/informes/red_antes.txt
resolvectl status >> documentos/informes/red_antes.txt
```

---

## 11. Configura una dirección IP fija con Netplan

> [!WARNING]
> Utiliza direcciones, prefijos, puerta de enlace y servidores DNS válidos para tu red. Una configuración incorrecta puede dejar el equipo sin conexión. Si trabajas mediante SSH, utiliza `netplan try` para permitir la recuperación automática.

Lista los archivos existentes:

```bash
ls -l /etc/netplan/
```

Identifica el archivo terminado en `.yaml` y crea una copia. Sustituye el nombre del ejemplo por el nombre real:

```bash
sudo cp /etc/netplan/50-cloud-init.yaml /etc/netplan/50-cloud-init.yaml.bak
```

Abre el archivo original:

```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

Adapta el siguiente modelo. Debes sustituir `enp0s3` y los valores de red:

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 192.168.1.50/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses:
          - 1.1.1.1
          - 8.8.8.8
```

En YAML debes utilizar espacios, nunca tabulaciones.

Valida la sintaxis:

```bash
sudo netplan generate
```

Prueba temporalmente la configuración:

```bash
sudo netplan try
```

Confirma la configuración solamente si mantienes la conectividad. Luego aplícala:

```bash
sudo netplan apply
```

### Comprobación

```bash
ip -4 -br address
ip route
ping -c 4 1.1.1.1
ping -c 4 example.com
```

Guarda el estado final:

```bash
ip -4 -br address > documentos/informes/red_despues.txt
ip route >> documentos/informes/red_despues.txt
```

---

# Parte III - Usuarios, grupos y permisos

## 12. Crea las cuentas y el grupo de trabajo

Crea un grupo llamado `soporte`:

```bash
sudo groupadd soporte
```

Crea dos cuentas:

```bash
sudo adduser soporte1
sudo adduser soporte2
```

Asigna una contraseña segura cuando el sistema la solicite. Puedes dejar vacíos los datos opcionales presionando `Enter`.

Agrega ambas cuentas al grupo `soporte`:

```bash
sudo usermod -aG soporte soporte1
sudo usermod -aG soporte soporte2
```

> [!IMPORTANT]
> No omitas `-a` al utilizar `usermod -aG`. La opción `-a` agrega el grupo sin eliminar otras pertenencias existentes.

### Comprobación

```bash
getent group soporte
id soporte1
id soporte2
```

Las dos cuentas deben aparecer como integrantes del grupo `soporte`.

---

## 13. Crea las carpetas compartidas

Crea una estructura común fuera de las carpetas personales:

```bash
sudo mkdir -p /srv/soporte/compartido
sudo mkdir -p /srv/soporte/publico
```

Asigna el grupo `soporte`:

```bash
sudo chown root:soporte /srv/soporte/compartido
sudo chown root:soporte /srv/soporte/publico
```

Consulta los propietarios y permisos actuales:

```bash
ls -ld /srv/soporte /srv/soporte/*
```

---

## 14. Asigna permisos de acceso

Configura la carpeta `compartido` para que solamente `root` y el grupo `soporte` puedan acceder:

```bash
sudo chmod 2770 /srv/soporte/compartido
```

Configura `publico` para que el grupo pueda escribir y las demás cuentas solamente puedan leer y entrar:

```bash
sudo chmod 2775 /srv/soporte/publico
```

El primer dígito `2` activa **SGID**. Los nuevos archivos y carpetas heredarán el grupo `soporte`.

| Valor | Permiso | Significado |
|---:|:---:|---|
| `4` | `r` | lectura |
| `2` | `w` | escritura |
| `1` | `x` | ejecución en archivos o acceso en directorios |
| `7` | `rwx` | lectura, escritura y ejecución/acceso |
| `5` | `r-x` | lectura y ejecución/acceso |
| `0` | `---` | sin permisos |

Comprueba la configuración:

```bash
ls -ld /srv/soporte/compartido /srv/soporte/publico
```

Debes observar permisos equivalentes a:

```text
drwxrws--- root soporte ... /srv/soporte/compartido
drwxrwsr-x root soporte ... /srv/soporte/publico
```

Prueba la creación de archivos con las nuevas cuentas:

```bash
sudo -u soporte1 touch /srv/soporte/compartido/archivo_soporte1.txt
sudo -u soporte2 touch /srv/soporte/compartido/archivo_soporte2.txt
ls -l /srv/soporte/compartido
```

Los dos archivos deben pertenecer al grupo `soporte`.

### Desafío de permisos

Ejecuta:

```bash
sudo -u nobody touch /srv/soporte/compartido/prueba_no_autorizada.txt
```

La operación debe devolver `Permiso denegado`. Si el archivo se crea, revisa los propietarios y los permisos de la carpeta.

---

# Parte IV - Variables y primeros scripts Bash

## 15. Utiliza variables en la terminal

Crea tres variables:

```bash
CURSO="Sistemas Operativos"
SERVIDOR="$(hostname)"
FECHA="$(date +%F)"
```

Consulta sus valores:

```bash
echo "$CURSO"
echo "$SERVIDOR"
echo "$FECHA"
```

Combínalas en una frase:

```bash
echo "Práctica de $CURSO realizada en $SERVIDOR el $FECHA"
```

Reconoce algunas variables del entorno:

```bash
echo "$USER"
echo "$HOME"
echo "$SHELL"
echo "$PATH"
```

Observa estas reglas:

- no agregues espacios alrededor de `=` al asignar una variable;
- utiliza `$NOMBRE` o `${NOMBRE}` para recuperar su valor;
- utiliza comillas dobles cuando el contenido tenga espacios;
- utiliza `$(comando)` para guardar la salida de un comando.

### Entrada por teclado

Solicita y reutiliza un dato:

```bash
read -r -p "Escribe tu nombre: " NOMBRE
echo "Hola, $NOMBRE. Estás trabajando en $(hostname)."
```

---

## 16. Crea tu primer script

Dirígete a la carpeta de scripts:

```bash
cd "$HOME/practica_linux/scripts"
```

Crea el archivo:

```bash
nano saludo.sh
```

Escribe el siguiente contenido:

```bash
#!/usr/bin/env bash

NOMBRE="${USER}"
EQUIPO="$(hostname)"
FECHA="$(date '+%d/%m/%Y %H:%M')"

echo "Hola, ${NOMBRE}."
echo "Estás trabajando en ${EQUIPO}."
echo "Fecha y hora: ${FECHA}."
```

Guarda el archivo, agrega permiso de ejecución y ejecútalo:

```bash
chmod u+x saludo.sh
./saludo.sh
```

También puedes ejecutarlo indicando el intérprete:

```bash
bash saludo.sh
```

### Comprobación

```bash
ls -l saludo.sh
```

El permiso del propietario debe incluir `x`.

---

## 17. Crea un script de estado del servidor

Crea el archivo:

```bash
nano estado_servidor.sh
```

Agrega el siguiente contenido:

```bash
#!/usr/bin/env bash

DIRECTORIO_LOG="${HOME}/practica_linux/logs"
ARCHIVO_LOG="${DIRECTORIO_LOG}/estado_servidor.log"
FECHA="$(date '+%F %T')"

mkdir -p "${DIRECTORIO_LOG}"

{
  echo "========================================"
  echo "Fecha: ${FECHA}"
  echo "Equipo: $(hostname)"
  echo "Usuario: ${USER}"
  echo "Kernel: $(uname -r)"
  echo "Tiempo encendido: $(uptime -p)"
  echo "Uso del sistema de archivos raíz:"
  df -h /
  echo "Memoria disponible:"
  free -h
} >> "${ARCHIVO_LOG}"

echo "Estado guardado en ${ARCHIVO_LOG}"
```

Agrega el permiso de ejecución y prueba el script:

```bash
chmod u+x estado_servidor.sh
./estado_servidor.sh
cat "$HOME/practica_linux/logs/estado_servidor.log"
```

Ejecuta el script una segunda vez y comprueba que el nuevo registro se agrega sin borrar el anterior:

```bash
./estado_servidor.sh
grep -c "Fecha:" "$HOME/practica_linux/logs/estado_servidor.log"
```

El resultado debe ser `2` si el archivo no contenía registros anteriores.

---

## 18. Crea un script con un argumento

Crea `crear_informe.sh`:

```bash
nano crear_informe.sh
```

Agrega:

```bash
#!/usr/bin/env bash

NOMBRE_INFORME="${1:-}"
DESTINO="${HOME}/practica_linux/documentos/informes"

if [ -z "${NOMBRE_INFORME}" ]; then
  echo "Uso: $0 nombre_del_informe"
  exit 1
fi

ARCHIVO="${DESTINO}/${NOMBRE_INFORME}.txt"

{
  echo "Informe: ${NOMBRE_INFORME}"
  echo "Fecha: $(date)"
  echo "Equipo: $(hostname)"
  echo "Responsable: ${USER}"
} > "${ARCHIVO}"

echo "Informe creado: ${ARCHIVO}"
```

Agrega permiso de ejecución y realiza dos pruebas:

```bash
chmod u+x crear_informe.sh
./crear_informe.sh
./crear_informe.sh revision_semanal
```

La primera ejecución debe mostrar el uso correcto. La segunda debe crear `revision_semanal.txt`.

Compruébalo:

```bash
cat "$HOME/practica_linux/documentos/informes/revision_semanal.txt"
```

---

# Parte V - Tareas programadas con Cron

## 19. Comprende la estructura de Crontab

Una entrada de `crontab` utiliza cinco campos de tiempo seguidos por el comando:

```text
┌──────── minuto (0-59)
│ ┌────── hora (0-23)
│ │ ┌──── día del mes (1-31)
│ │ │ ┌── mes (1-12)
│ │ │ │ ┌ día de la semana (0-7; 0 y 7 representan domingo)
│ │ │ │ │
* * * * * comando
```

Ejemplos:

| Expresión | Ejecución |
|---|---|
| `*/5 * * * *` | cada 5 minutos |
| `0 * * * *` | al comienzo de cada hora |
| `30 7 * * 1-5` | a las 07:30, de lunes a viernes |
| `0 2 1 * *` | a las 02:00 del primer día de cada mes |
| `@reboot` | cada vez que se inicia el sistema |

Comprueba que el servicio está disponible:

```bash
systemctl status cron --no-pager
```

Si no está activo, inicia y habilita el servicio:

```bash
sudo systemctl enable --now cron
```

---

## 20. Programa una tarea de prueba

Abre tu archivo personal de tareas:

```bash
crontab -e
```

Si se solicita un editor, selecciona Nano. Agrega al final:

```cron
*/2 * * * * /usr/bin/date >> "$HOME/practica_linux/logs/cron_prueba.log" 2>&1
```

Guarda el archivo y consulta la tarea instalada:

```bash
crontab -l
```

Espera al menos dos minutos y comprueba el registro:

```bash
cat "$HOME/practica_linux/logs/cron_prueba.log"
```

Debes observar una nueva fecha cada dos minutos.

Consulta la actividad reciente del servicio:

```bash
journalctl -u cron --since "10 minutes ago" --no-pager
```

> [!NOTE]
> Cron trabaja con un entorno reducido. Utiliza rutas absolutas para los comandos y scripts, y redirige tanto la salida normal como los errores a un archivo de registro.

---

## 21. Programa el script de estado

Obtén la ruta absoluta del script:

```bash
realpath "$HOME/practica_linux/scripts/estado_servidor.sh"
```

Abre nuevamente tu `crontab`:

```bash
crontab -e
```

Agrega una tarea que ejecute el script cada cinco minutos. Sustituye `<tu_usuario>` por tu cuenta real:

```cron
*/5 * * * * /home/<tu_usuario>/practica_linux/scripts/estado_servidor.sh >> /home/<tu_usuario>/practica_linux/logs/cron_estado.log 2>&1
```

Verifica la sintaxis instalada:

```bash
crontab -l
```

Después de cinco minutos, consulta ambos registros:

```bash
tail -n 20 "$HOME/practica_linux/logs/estado_servidor.log"
cat "$HOME/practica_linux/logs/cron_estado.log"
```

El primer archivo debe contener un nuevo bloque de información. El segundo permanecerá vacío si el script no produce errores, salvo por el mensaje final del propio script.

Cuando termines la prueba, elimina solamente la tarea que se ejecuta cada dos minutos y conserva la tarea del script de estado:

```bash
crontab -e
```

Confirma el resultado:

```bash
crontab -l
```

---

# Parte VI - Desafío integrador

## 22. Automatiza una copia de seguridad

Crea un script llamado `respaldo_soporte.sh` dentro de `scripts`. Debe cumplir estas condiciones:

1. Debe comenzar con `#!/usr/bin/env bash`.
2. Debe utilizar variables para definir:
   - la carpeta de origen `/srv/soporte/compartido`;
   - la carpeta de destino `$HOME/practica_linux/copias`;
   - la fecha y hora del respaldo;
   - el nombre del archivo comprimido.
3. Debe crear la carpeta de destino con `mkdir -p`.
4. Debe comprobar con `if [ -d ... ]` que la carpeta de origen existe.
5. Debe crear un archivo `.tar.gz` mediante `tar`.
6. Debe registrar el resultado en `$HOME/practica_linux/logs/respaldos.log`.
7. Debe finalizar con un mensaje que indique si la operación fue correcta o si ocurrió un error.

Puedes utilizar esta base y completar los espacios indicados:

```bash
#!/usr/bin/env bash

ORIGEN="/srv/soporte/compartido"
DESTINO="${HOME}/practica_linux/copias"
REGISTRO="${HOME}/practica_linux/logs/respaldos.log"
MARCA_TIEMPO="$(date '+%Y%m%d_%H%M%S')"
ARCHIVO="${DESTINO}/soporte_${MARCA_TIEMPO}.tar.gz"

mkdir -p "${DESTINO}"

if [ -d "${ORIGEN}" ]; then
  # Completa aquí el comando tar.
  # Comprueba el resultado mediante $? o una condición if.
  # Registra la fecha, el nombre del archivo y el resultado.
else
  echo "$(date '+%F %T') - No existe ${ORIGEN}" >> "${REGISTRO}"
  echo "No se pudo realizar el respaldo."
  exit 1
fi
```

Para crear el archivo comprimido, investiga y prueba la estructura:

```bash
tar -czf archivo_destino.tar.gz carpeta_origen
```

Cuando el script funcione:

```bash
chmod u+x "$HOME/practica_linux/scripts/respaldo_soporte.sh"
"$HOME/practica_linux/scripts/respaldo_soporte.sh"
```

Verifica el contenido sin extraerlo:

```bash
tar -tzf "$HOME/practica_linux/copias/<nombre_del_respaldo>.tar.gz"
```

Finalmente, programa el respaldo para todos los días a las 20:30:

```cron
30 20 * * * /home/<tu_usuario>/practica_linux/scripts/respaldo_soporte.sh >> /home/<tu_usuario>/practica_linux/logs/cron_respaldo.log 2>&1
```

---

## 23. Comprobación final

Marca cada punto después de verificarlo en la terminal:

- [ ] La estructura `$HOME/practica_linux` contiene `documentos`, `copias`, `scripts` y `logs`.
- [ ] El archivo `equipos.txt` conserva las tres líneas solicitadas.
- [ ] El nombre del servidor es `srv-soporte`.
- [ ] La dirección IP, la ruta predeterminada y DNS funcionan correctamente.
- [ ] Las cuentas `soporte1` y `soporte2` existen.
- [ ] Ambas cuentas pertenecen al grupo `soporte`.
- [ ] `/srv/soporte/compartido` pertenece a `root:soporte` y tiene permisos `2770`.
- [ ] Una cuenta del grupo puede crear archivos en la carpeta compartida.
- [ ] Una cuenta ajena al grupo no puede crear archivos en la carpeta compartida.
- [ ] `saludo.sh`, `estado_servidor.sh` y `crear_informe.sh` tienen permiso de ejecución.
- [ ] `estado_servidor.sh` agrega información sin borrar los registros anteriores.
- [ ] `crontab -l` muestra la tarea de estado y la tarea diaria de respaldo.
- [ ] El respaldo `.tar.gz` puede listarse con `tar -tzf`.
- [ ] Los errores y resultados quedan registrados dentro de la carpeta `logs`.

Ejecuta este resumen para revisar los elementos principales:

```bash
echo "=== IDENTIDAD DEL SISTEMA ==="
hostnamectl hostname

echo "=== RED ==="
ip -4 -br address
ip route

echo "=== USUARIOS Y GRUPO ==="
getent group soporte
id soporte1
id soporte2

echo "=== PERMISOS ==="
ls -ld /srv/soporte/compartido /srv/soporte/publico

echo "=== SCRIPTS ==="
ls -l "$HOME/practica_linux/scripts"

echo "=== TAREAS PROGRAMADAS ==="
crontab -l

echo "=== RESPALDOS ==="
ls -lh "$HOME/practica_linux/copias"
```

---

## 24. Comandos utilizados

| Categoría | Comandos principales |
|---|---|
| Identificación y ayuda | `whoami`, `hostname`, `pwd`, `man`, `--help` |
| Navegación | `cd`, `ls`, `find` |
| Carpetas y archivos | `mkdir`, `touch`, `cp`, `mv`, `rm`, `rmdir` |
| Contenido de archivos | `cat`, `less`, `nano`, `echo`, `>`, `>>` |
| Estado del sistema | `hostnamectl`, `uname`, `uptime`, `free`, `df`, `lsblk` |
| Red | `ip`, `ping`, `resolvectl`, `netplan` |
| Usuarios y grupos | `adduser`, `groupadd`, `usermod`, `id`, `getent` |
| Propiedad y permisos | `chown`, `chmod`, `sudo -u` |
| Scripts | `bash`, `chmod`, `read`, `test`, `if`, variables |
| Automatización | `crontab`, `systemctl`, `journalctl` |
| Copias comprimidas | `tar` |

---

## 25. Limpieza opcional del entorno

Realiza esta limpieza solamente si ya no necesitas conservar la práctica.

Primero elimina las tareas programadas una por una mediante:

```bash
crontab -e
```

Luego elimina las cuentas, sus carpetas personales y el grupo:

```bash
sudo deluser --remove-home soporte1
sudo deluser --remove-home soporte2
sudo groupdel soporte
```

Por último, elimina la estructura compartida:

```bash
sudo rm -r /srv/soporte
```

La carpeta personal `$HOME/practica_linux` contiene tus scripts, registros y comprobaciones. Consérvala mientras necesites revisar el trabajo realizado.
