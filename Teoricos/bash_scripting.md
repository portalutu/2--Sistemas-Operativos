# Introducción a Bash Scripting

Bash es un intérprete de comandos habitual en GNU/Linux. Un script de Bash reúne comandos en un archivo para ejecutarlos en un orden definido, reutilizar tareas y registrar resultados. Trabajaremos con scripts pequeños al inicio y avanzaremos hacia tareas de administración habituales.

> **Precaución:** prueba los scripts con datos de ejemplo antes de usarlos en directorios del sistema o con información importante. Revisa siempre las rutas y evita ejecutar comandos destructivos como `rm` si no comprendes exactamente qué archivos alcanzarán.

---

## 1. Preparación y estructura mínima

### Introducción

Todo script necesita indicar qué intérprete debe procesarlo. También conviene asignarle permiso de ejecución y ejecutarlo desde una ruta conocida.

### Explicación teórica

La primera línea, llamada *shebang*, indica el programa que ejecutará el archivo. `#!/usr/bin/env bash` busca Bash en el entorno del sistema y permite que el script funcione cuando Bash no está exactamente en `/bin/bash`.

Los comentarios comienzan con `#`. Sirven para documentar decisiones, entradas y efectos del script. Para ejecutar un archivo, se puede invocar Bash directamente o marcar el archivo como ejecutable con `chmod +x`.

### Ejemplo práctico: primer script

1. Crea un directorio de trabajo y entra en él:

   ```bash
   mkdir -p ~/scripts-bash
   cd ~/scripts-bash
   ```

2. Crea el archivo `saludo.sh` con este contenido:

   ```bash
   #!/usr/bin/env bash
   # Muestra un mensaje y el usuario que ejecuta el script.

   echo "Hola. Ejecutamos este script como: $(whoami)"
   ```

3. Otorga permiso de ejecución y ejecútalo:

   ```bash
   chmod +x saludo.sh
   ./saludo.sh
   ```

También es válido ejecutar `bash saludo.sh`. En ese caso no es necesario el permiso de ejecución, pero el *shebang* sigue siendo recomendable.

---

## 2. Variables, sustitución de comandos y argumentos

### Introducción

Las variables permiten guardar valores para usarlos varias veces. Los argumentos hacen que un mismo script pueda operar sobre diferentes rutas o nombres sin editar su contenido.

### Explicación teórica

Una variable se asigna sin espacios alrededor del signo igual: `nombre="valor"`. Para leerla se antepone `$`, normalmente dentro de llaves: `${nombre}`. La sintaxis `$(comando)` ejecuta un comando y reemplaza la expresión por su salida.

Los argumentos recibidos se identifican con `$1`, `$2` y así sucesivamente. La variable especial `$#` indica cuántos argumentos llegaron. Encierra las variables entre comillas dobles para que las rutas con espacios se traten como un solo valor.

### Ejemplo práctico: mostrar fecha y hora

Guarda el siguiente contenido en `fecha_hora.sh`:

```bash
#!/usr/bin/env bash

formato="${1:-%Y-%m-%d %H:%M:%S}"
momento=$(date "+${formato}")

echo "Fecha y hora: ${momento}"
```

El valor `${1:-...}` usa el primer argumento si fue proporcionado; de lo contrario, usa el formato por defecto. Ejecútalo de estas dos formas:

```bash
chmod +x fecha_hora.sh
./fecha_hora.sh
./fecha_hora.sh '%d/%m/%Y - %H:%M'
```

---

## 3. Información básica del sistema

### Introducción

Un script puede reunir datos que normalmente consultaríamos con varios comandos: equipo, sistema operativo, tiempo de actividad, memoria y espacio disponible.

### Explicación teórica

`uname` informa sobre el kernel; `/etc/os-release` describe la distribución; `uptime` muestra cuánto tiempo lleva encendido el sistema y su carga; `free -h` resume la memoria; `df -h` informa del uso de los sistemas de archivos montados. El operador de redirección `>` crea o reemplaza un archivo, mientras que `>>` agrega contenido al final.

### Ejemplo práctico: informe del sistema

Guarda este script como `informe_sistema.sh`:

```bash
#!/usr/bin/env bash

archivo="${1:-informe_sistema_$(date +%Y%m%d_%H%M%S).txt}"

{
  echo "INFORME DEL SISTEMA"
  echo "Generado: $(date '+%Y-%m-%d %H:%M:%S')"
  echo
  echo "--- Equipo y sistema operativo ---"
  hostnamectl 2>/dev/null || hostname
  cat /etc/os-release
  echo
  echo "--- Kernel y actividad ---"
  uname -r
  uptime
  echo
  echo "--- Memoria ---"
  free -h
  echo
  echo "--- Espacio en disco ---"
  df -h
} > "${archivo}"

echo "Informe creado en: ${archivo}"
```

Ejecuta `./informe_sistema.sh`. Para elegir el nombre del informe, proporciona una ruta como primer argumento:

```bash
./informe_sistema.sh /tmp/estado_servidor.txt
```

La agrupación entre llaves permite redirigir toda la salida al mismo archivo. El fragmento `2>/dev/null` oculta el mensaje de error si `hostnamectl` no está disponible.

---

## 4. Decisiones y validación de entradas

### Introducción

Antes de realizar una tarea, un script debe verificar que recibió los datos necesarios y que las rutas existen. Esto reduce errores por argumentos omitidos o mal escritos.

### Explicación teórica

La estructura `if ...; then ... fi` ejecuta comandos según el resultado de una condición. En Bash, el código de salida `0` representa éxito. Algunas pruebas frecuentes son `-d` para directorios, `-f` para archivos y `-r` para permiso de lectura.

La instrucción `exit 1` finaliza el script indicando que ocurrió un error. `exit 0` indica una finalización correcta; si se omite, Bash devuelve el estado del último comando ejecutado.

### Ejemplo práctico: contar archivos en un directorio

Guarda el script como `contar_archivos.sh`:

```bash
#!/usr/bin/env bash

if [ "$#" -ne 1 ]; then
  echo "Uso: $0 RUTA_DEL_DIRECTORIO" >&2
  exit 1
fi

directorio="$1"

if [ ! -d "${directorio}" ]; then
  echo "Error: no existe el directorio: ${directorio}" >&2
  exit 1
fi

cantidad=$(find "${directorio}" -maxdepth 1 -type f | wc -l)
echo "Archivos regulares en ${directorio}: ${cantidad}"
```

Prueba con `./contar_archivos.sh /etc`. El texto `>&2` envía mensajes de uso y errores a la salida de error estándar, separándolos de la salida normal del script.

---

## 5. Repetición y procesamiento de archivos

### Introducción

Los bucles permiten aplicar una acción a una colección de archivos. Antes de modificar archivos, conviene mostrar la acción que se realizará.

### Explicación teórica

`for` repite un bloque para cada elemento de una lista. `find` localiza elementos en una ruta usando condiciones como el tipo (`-type f`) o la extensión (`-name '*.log'`). Cuando se trabaja con nombres que pueden incluir espacios, usa `find ... -print0` junto con `read -d ''` para conservar cada nombre intacto.

### Ejemplo práctico: listar archivos de registro

Guarda el contenido en `listar_logs.sh`:

```bash
#!/usr/bin/env bash

directorio="${1:-.}"

if [ ! -d "${directorio}" ]; then
  echo "Error: ${directorio} no es un directorio válido." >&2
  exit 1
fi

encontrados=0
while IFS= read -r -d '' archivo; do
  printf '%s\n' "${archivo}"
  encontrados=$((encontrados + 1))
done < <(find "${directorio}" -type f -name '*.log' -print0)

echo "Total de archivos .log: ${encontrados}"
```

Ejecuta `./listar_logs.sh /var/log`. Si no se proporciona una ruta, se examina el directorio actual. La construcción `< <(...)` entrega la salida de `find` al bucle sin crear un archivo temporal.

---

## 6. Crear backups comprimidos

### Introducción

Un backup comprimido agrupa el contenido de una ruta en un único archivo. `tar` conserva la estructura de directorios y, con gzip, reduce el espacio utilizado.

### Explicación teórica

Las opciones más usadas son `-c` para crear un archivo, `-z` para comprimir con gzip, `-f` para indicar el nombre resultante y `-v` para mostrar los archivos procesados. La extensión convencional es `.tar.gz`.

Un backup debe almacenarse fuera del directorio de origen; de esa forma no se incluirá a sí mismo ni se perderá junto con la información respaldada. Antes de crear uno, se valida la ruta origen y se crea el directorio de destino si no existe.

### Ejemplo práctico: backup con fecha

Guarda este script como `backup_comprimido.sh`:

```bash
#!/usr/bin/env bash

origen="${1:-}"
destino="${2:-$HOME/backups}"

if [ -z "${origen}" ] || [ ! -d "${origen}" ]; then
  echo "Uso: $0 DIRECTORIO_ORIGEN [DIRECTORIO_DESTINO]" >&2
  exit 1
fi

mkdir -p "${destino}"
nombre=$(basename "${origen%/}")
fecha=$(date +%Y%m%d_%H%M%S)
archivo="${destino}/${nombre}_${fecha}.tar.gz"

tar -czf "${archivo}" -C "$(dirname "${origen%/}")" "${nombre}"

echo "Backup creado: ${archivo}"
echo "Contenido del backup:"
tar -tzf "${archivo}"
```

Por ejemplo, ejecuta:

```bash
./backup_comprimido.sh ~/Documentos ~/backups
```

`-C` cambia temporalmente al directorio padre del origen. Así, el archivo comprimido guarda una ruta relativa, no una ruta absoluta del equipo.

---

## 7. Limpiar logs anteriores a una fecha

### Introducción

Los archivos de registro pueden acumularse con el tiempo. La limpieza debe comenzar mostrando qué archivos coinciden; solo después se debe habilitar su eliminación.

### Explicación teórica

`find` admite referencias temporales. La opción `-mtime +N` selecciona archivos modificados hace más de `N` períodos completos de 24 horas. Por ejemplo, `-mtime +30` no equivale exactamente a una fecha del calendario: selecciona archivos con más de 31 períodos completos de 24 horas.

En sistemas GNU, `-not -newermt 'AAAA-MM-DD'` selecciona archivos cuya última modificación es anterior a esa fecha a las 00:00. Como puede variar entre implementaciones de `find`, verifica que el comando funcione en el sistema donde lo ejecutarás.

### Ejemplo práctico: vista previa y eliminación confirmada

Guarda este contenido como `limpiar_logs_por_fecha.sh`:

```bash
#!/usr/bin/env bash

directorio="${1:-}"
fecha_limite="${2:-}"

if [ -z "${directorio}" ] || [ -z "${fecha_limite}" ] || [ ! -d "${directorio}" ]; then
  echo "Uso: $0 DIRECTORIO_LOGS AAAA-MM-DD" >&2
  exit 1
fi

mapfile -d '' archivos < <(
  find "${directorio}" -type f -name '*.log' -not -newermt "${fecha_limite}" -print0
)

if [ "${#archivos[@]}" -eq 0 ]; then
  echo "No hay archivos .log anteriores a ${fecha_limite}."
  exit 0
fi

echo "Se encontraron estos archivos:"
printf '  %s\n' "${archivos[@]}"
read -r -p "Escribe ELIMINAR para borrarlos: " confirmacion

if [ "${confirmacion}" = "ELIMINAR" ]; then
  printf '%s\0' "${archivos[@]}" | xargs -0 rm -f --
  echo "Limpieza completada."
else
  echo "Operación cancelada: no se eliminó ningún archivo."
fi
```

Primero realiza una prueba sobre una carpeta creada para este fin, por ejemplo:

```bash
mkdir -p ~/prueba-logs
./limpiar_logs_por_fecha.sh ~/prueba-logs 2026-01-01
```

No apuntes este script a `/var/log` sin identificar cuáles archivos pueden eliminarse y sin considerar la política de retención del servicio que los genera.

---

## 8. Limpiar archivos temporales de forma controlada

### Introducción

Los directorios temporales de una aplicación pueden contener archivos que ya no son necesarios. Una limpieza segura restringe el tipo de archivo, el directorio y la antigüedad, y permite una ejecución de prueba.

### Explicación teórica

El script siguiente acepta una ruta, una antigüedad en días y una opción `--aplicar`. Sin esa opción solo muestra los archivos seleccionados. Este patrón se conoce como *dry run*: permite revisar el alcance real antes de modificar datos.

No uses este procedimiento sobre `/tmp` ni sobre rutas administradas por el sistema sin conocer qué procesos las utilizan. Trabajaremos sobre un directorio temporal perteneciente a una aplicación concreta.

### Ejemplo práctico: limpieza con modo de prueba

Guarda este contenido en `limpiar_temporales.sh`:

```bash
#!/usr/bin/env bash

directorio="${1:-}"
dias="${2:-}"
modo="${3:-}"

if [ -z "${directorio}" ] || [ -z "${dias}" ] || [ ! -d "${directorio}" ]; then
  echo "Uso: $0 DIRECTORIO DIAS [--aplicar]" >&2
  exit 1
fi

if ! [[ "${dias}" =~ ^[0-9]+$ ]]; then
  echo "Error: DIAS debe ser un número entero no negativo." >&2
  exit 1
fi

echo "Archivos temporales con más de ${dias} días:"
find "${directorio}" -type f \( -name '*.tmp' -o -name '*.temp' \) -mtime "+${dias}" -print

if [ "${modo}" = "--aplicar" ]; then
  find "${directorio}" -type f \( -name '*.tmp' -o -name '*.temp' \) -mtime "+${dias}" -delete
  echo "Limpieza aplicada."
else
  echo "Vista previa finalizada. Para borrar, agrega --aplicar al final."
fi
```

Ejecuta primero el modo de prueba:

```bash
./limpiar_temporales.sh ~/mi_aplicacion/tmp 14
```

Si la lista es correcta, aplica la limpieza:

```bash
./limpiar_temporales.sh ~/mi_aplicacion/tmp 14 --aplicar
```

---

## 9. Recomendaciones para scripts confiables

### Introducción

A medida que un script administra más archivos, pequeñas decisiones evitan errores difíciles de detectar.

### Explicación teórica

Al comienzo de scripts que modifiquen datos, podemos activar opciones de Bash para detectar problemas pronto:

```bash
set -euo pipefail
```

- `-e` detiene el script si un comando falla.
- `-u` considera un error usar una variable no definida.
- `pipefail` marca como fallida una tubería si falla cualquiera de sus comandos.

Estas opciones no reemplazan las validaciones: un script debe continuar verificando argumentos, rutas y permisos. Usa nombres descriptivos, cita las variables que contengan rutas (`"${ruta}"`), registra las operaciones importantes y prueba siempre sobre copias de datos.

### Ejemplo práctico: plantilla reutilizable

Usa esta plantilla como punto de partida para un script que reciba un directorio:

```bash
#!/usr/bin/env bash
set -euo pipefail

if [ "$#" -ne 1 ]; then
  echo "Uso: $0 DIRECTORIO" >&2
  exit 1
fi

directorio="$1"

if [ ! -d "${directorio}" ]; then
  echo "Error: no existe el directorio: ${directorio}" >&2
  exit 1
fi

echo "Procesando: ${directorio}"
# Agrega aquí la tarea específica.
```

Con esta estructura, haremos que cada nueva automatización tenga una entrada clara, validaciones iniciales y un comportamiento verificable antes de afectar archivos.
