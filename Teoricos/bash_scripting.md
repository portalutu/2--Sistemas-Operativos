# Introducción a Bash Scripting

Bash es un intérprete de comandos habitual en GNU/Linux. Un script de Bash reúne comandos en un archivo para ejecutarlos en un orden definido, reutilizar tareas y registrar resultados. Trabajaremos con scripts pequeños al inicio y avanzaremos hacia tareas de administración habituales.

> **Precaución:** prueba los scripts con datos de ejemplo antes de usarlos en directorios del sistema o con información importante. Revisa siempre las rutas y evita ejecutar comandos destructivos como `rm` si no comprendes exactamente qué archivos alcanzarán.

---

## Índice

1. [Preparación y estructura mínima](#1-preparación-y-estructura-mínima)
2. [Variables, sustitución de comandos y argumentos](#2-variables-sustitución-de-comandos-y-argumentos)
3. [Manejo de parámetros](#3-manejo-de-parámetros)
4. [Estructuras de control](#4-estructuras-de-control)
5. [Iteraciones](#5-iteraciones)
6. [Listar carpetas y recorrer directorios](#6-listar-carpetas-y-recorrer-directorios)
7. [Abrir y leer archivos](#7-abrir-y-leer-archivos)
8. [Información básica del sistema](#8-información-básica-del-sistema)
9. [Decisiones y validación de entradas](#9-decisiones-y-validación-de-entradas)
10. [Repetición y procesamiento de archivos](#10-repetición-y-procesamiento-de-archivos)
11. [Crear backups comprimidos](#11-crear-backups-comprimidos)
12. [Limpiar logs anteriores a una fecha](#12-limpiar-logs-anteriores-a-una-fecha)
13. [Limpiar archivos temporales de forma controlada](#13-limpiar-archivos-temporales-de-forma-controlada)
14. [Recomendaciones para scripts confiables](#14-recomendaciones-para-scripts-confiables)
15. [Scripts prácticos de administración](#15-scripts-prácticos-de-administración)
    - [15.1 Migrar el contenido de una carpeta](#151-migrar-el-contenido-de-una-carpeta)
    - [15.2 Inventariar carpetas](#152-inventariar-carpetas)
    - [15.3 Backups con rotación](#153-backups-con-rotación)
    - [15.4 Restaurar un backup](#154-restaurar-un-backup)
    - [15.5 Limpiar logs del sistema](#155-limpiar-logs-del-sistema)
    - [15.6 Limpiar archivos temporales del sistema](#156-limpiar-archivos-temporales-del-sistema)
    - [15.7 Actualizar el sistema](#157-actualizar-el-sistema)
    - [15.8 Crear usuarios del grupo "tecnicos"](#158-crear-usuarios-del-grupo-tecnicos-con-carpetas-individuales-y-compartida)
    - [15.9 Revisar el tráfico de red](#159-revisar-el-tráfico-de-red)
    - [15.10 Obtener información del sistema](#1510-obtener-información-del-sistema)

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

FORMATO="${1:-%Y-%m-%d %H:%M:%S}"
MOMENTO=$(date "+${FORMATO}")

echo "Fecha y hora: ${MOMENTO}"
```

El valor `${1:-...}` usa el primer argumento si fue proporcionado; de lo contrario, usa el formato por defecto. Ejecútalo de estas dos formas:

```bash
chmod +x fecha_hora.sh  
./fecha_hora.sh
./fecha_hora.sh '%d/%m/%Y - %H:%M'
```

---

## 3. Manejo de parámetros

### Introducción

Un script útil rara vez trabaja siempre con los mismos datos. Los parámetros (también llamados argumentos) permiten indicarle al ejecutarlo qué archivo procesar, qué opciones activar o qué valores usar, sin modificar el código.

### Explicación teórica

**Parámetros posicionales y especiales**

| Variable | Contenido |
|----------|-----------|
| `$0` | nombre con el que se invocó el script |
| `$1`, `$2`, ... `${10}` | primer, segundo, ... argumento (a partir del décimo se requieren llaves) |
| `$#` | cantidad de argumentos recibidos |
| `"$@"` | todos los argumentos, cada uno como elemento separado |
| `"$*"` | todos los argumentos unidos en una sola cadena |
| `$?` | código de salida del último comando |

Usa casi siempre `"$@"` entre comillas: respeta los argumentos que contienen espacios.

**Valores por defecto y obligatorios**

- `${1:-valor}`: usa `valor` si `$1` no está definido o está vacío.
- `${1:?mensaje}`: detiene el script con `mensaje` si `$1` falta.
- `${1:+texto}`: devuelve `texto` solo si `$1` tiene valor.

**`shift`** descarta el primer argumento y desplaza los demás: `$2` pasa a ser `$1`. Se usa para consumir argumentos dentro de un bucle `while`.

**Opciones con nombre.** Hay dos enfoques habituales:

- `getopts`: integrado en Bash, maneja opciones cortas (`-v`, `-o archivo`). Una opción seguida de `:` en la cadena de definición espera un valor.
- `while` + `case` + `shift`: permite opciones largas (`--salida archivo`, `--verbose`) y mayor control.

Una buena práctica es validar siempre la cantidad y el tipo de los parámetros y mostrar un mensaje de uso (`Uso: ...`) cuando son incorrectos.

### Ejemplo 1: leer parámetros posicionales

Guarda como `parametros.sh`:

```bash
#!/usr/bin/env bash

echo "Script:             $0"
echo "Cantidad:           $#"
echo "Primer argumento:   ${1:-<no indicado>}"
echo "Segundo argumento:  ${2:-<no indicado>}"
echo "Todos:              $*"

i=1
for ARG in "$@"; do
  echo "  Argumento ${i}: '${ARG}'"
  i=$((i + 1))
done
```

Pruébalo con `./parametros.sh uno "dos con espacios" tres` y observa que el segundo argumento se conserva completo.

### Ejemplo 2: validar cantidad de parámetros

```bash
#!/usr/bin/env bash

if [ "$#" -lt 2 ] || [ "$#" -gt 3 ]; then
  echo "Uso: $0 ORIGEN DESTINO [--sobrescribir]" >&2
  exit 1
fi

ORIGEN="$1"
DESTINO="$2"
SOBRESCRIBIR="${3:-}"

if [ ! -f "${ORIGEN}" ]; then
  echo "Error: ${ORIGEN} no existe." >&2
  exit 1
fi

if [ -e "${DESTINO}" ] && [ "${SOBRESCRIBIR}" != "--sobrescribir" ]; then
  echo "Error: ${DESTINO} ya existe. Usa --sobrescribir para reemplazarlo." >&2
  exit 1
fi

cp -- "${ORIGEN}" "${DESTINO}"
echo "Copiado: ${ORIGEN} -> ${DESTINO}"
```

### Ejemplo 3: valores por defecto y parámetros obligatorios

```bash
#!/usr/bin/env bash

# Obligatorio: si falta, el script termina con ese mensaje
DIRECTORIO="${1:?Falta el directorio. Uso: $0 DIRECTORIO [EXTENSION] [DIAS]}"

# Opcionales con valor por defecto
EXTENSION="${2:-log}"
DIAS="${3:-7}"

echo "Buscando archivos .${EXTENSION} en ${DIRECTORIO} con más de ${DIAS} días"
find "${DIRECTORIO}" -type f -name "*.${EXTENSION}" -mtime "+${DIAS}"
```

### Ejemplo 4: procesar una cantidad variable de argumentos

```bash
#!/usr/bin/env bash

if [ "$#" -eq 0 ]; then
  echo "Uso: $0 ARCHIVO..." >&2
  exit 1
fi

TOTAL=0
FALLIDOS=0

for ARCHIVO in "$@"; do
  if [ -r "${ARCHIVO}" ]; then
    LINEAS=$(wc -l < "${ARCHIVO}")
    echo "${ARCHIVO}: ${LINEAS} líneas"
    TOTAL=$((TOTAL + LINEAS))
  else
    echo "${ARCHIVO}: no se puede leer" >&2
    FALLIDOS=$((FALLIDOS + 1))
  fi
done

echo "Total de líneas: ${TOTAL} (archivos con error: ${FALLIDOS})"
```

### Ejemplo 5: `shift` para consumir argumentos

```bash
#!/usr/bin/env bash

if [ "$#" -lt 2 ]; then
  echo "Uso: $0 PREFIJO PALABRA..." >&2
  exit 1
fi

PREFIJO="$1"
shift                     # ahora $1 es la primera PALABRA

echo "Quedan $# palabras"

for PALABRA in "$@"; do
  echo "${PREFIJO}${PALABRA}"
done
```

Con `./prefijar.sh pre_ uno dos tres` se imprime `pre_uno`, `pre_dos` y `pre_tres`.

### Ejemplo 6: opciones cortas con `getopts`

Guarda como `reporte.sh`:

```bash
#!/usr/bin/env bash

USO="Uso: $0 [-v] [-n LINEAS] [-o SALIDA] ARCHIVO"

VERBOSE=0
LINEAS=10
SALIDA=""

while getopts ":vn:o:h" OPCION; do
  case "${OPCION}" in
    v) VERBOSE=1 ;;
    n) LINEAS="${OPTARG}" ;;
    o) SALIDA="${OPTARG}" ;;
    h) echo "${USO}"; exit 0 ;;
    :) echo "La opción -${OPTARG} requiere un valor." >&2; exit 1 ;;
    \?) echo "Opción desconocida: -${OPTARG}" >&2; echo "${USO}" >&2; exit 1 ;;
  esac
done

# Descarta las opciones ya procesadas; quedan solo los argumentos posicionales
shift $((OPTIND - 1))

if [ "$#" -ne 1 ]; then
  echo "${USO}" >&2
  exit 1
fi

ARCHIVO="$1"

if ! [[ "${LINEAS}" =~ ^[0-9]+$ ]]; then
  echo "Error: -n requiere un número entero." >&2
  exit 1
fi

[ "${VERBOSE}" -eq 1 ] && echo "Leyendo ${ARCHIVO} (${LINEAS} líneas)..." >&2

if [ -n "${SALIDA}" ]; then
  head -n "${LINEAS}" "${ARCHIVO}" > "${SALIDA}"
  echo "Resultado guardado en ${SALIDA}"
else
  head -n "${LINEAS}" "${ARCHIVO}"
fi
```

Ejemplos de uso:

```bash
./reporte.sh datos.txt
./reporte.sh -v -n 5 datos.txt
./reporte.sh -n 20 -o resumen.txt datos.txt
./reporte.sh -vn 3 datos.txt          # las opciones cortas pueden agruparse
```

En `":vn:o:h"`, los dos puntos iniciales activan el manejo manual de errores; `n:` y `o:` indican que esas opciones reciben un valor, que queda en `OPTARG`. `OPTIND` indica el índice del siguiente argumento por procesar.

### Ejemplo 7: opciones largas con `while`, `case` y `shift`

```bash
#!/usr/bin/env bash

USO="Uso: $0 [--verbose] [--dias N] [--destino DIR] ORIGEN"

VERBOSE=0
DIAS=30
DESTINO="${HOME}/backups"
POSICIONALES=()

while [ "$#" -gt 0 ]; do
  case "$1" in
    -v|--verbose)
      VERBOSE=1
      shift
      ;;
    -d|--dias)
      [ "$#" -ge 2 ] || { echo "Falta valor para $1" >&2; exit 1; }
      DIAS="$2"
      shift 2
      ;;
    --destino=*)
      DESTINO="${1#*=}"      # toma lo que sigue al primer '='
      shift
      ;;
    -h|--help)
      echo "${USO}"
      exit 0
      ;;
    --)
      shift
      POSICIONALES+=("$@")   # todo lo que sigue son argumentos, no opciones
      break
      ;;
    -*)
      echo "Opción desconocida: $1" >&2
      echo "${USO}" >&2
      exit 1
      ;;
    *)
      POSICIONALES+=("$1")
      shift
      ;;
  esac
done

if [ "${#POSICIONALES[@]}" -ne 1 ]; then
  echo "${USO}" >&2
  exit 1
fi

ORIGEN="${POSICIONALES[0]}"

echo "Origen:  ${ORIGEN}"
echo "Destino: ${DESTINO}"
echo "Días:    ${DIAS}"
echo "Verbose: ${VERBOSE}"
```

Pruébalo con `./opciones.sh --verbose --dias 10 --destino=/tmp/bk /etc`. Este patrón admite `--opcion valor`, `--opcion=valor` y el separador `--`, que permite pasar nombres que empiezan con guion.

### Ejemplo 8: parámetros hacia funciones

Dentro de una función, `$1`, `$2`, `$#` y `"$@"` se refieren a los argumentos de la función, no a los del script:

```bash
#!/usr/bin/env bash

saludar() {
  local NOMBRE="${1:-invitado}"
  local SALUDO="${2:-Hola}"
  echo "${SALUDO}, ${NOMBRE}."
}

sumar() {
  local TOTAL=0
  for N in "$@"; do
    TOTAL=$((TOTAL + N))
  done
  echo "${TOTAL}"
}

saludar                      # Hola, invitado.
saludar "Ana"                # Hola, Ana.
saludar "Ana" "Buenos días"  # Buenos días, Ana.

RESULTADO=$(sumar 4 8 15 16 23 42)
echo "Suma: ${RESULTADO}"
```

`local` limita la variable a la función, evitando que modifique variables del script con el mismo nombre.

### Ejemplo 9: entrada interactiva cuando falta un parámetro

```bash
#!/usr/bin/env bash

NOMBRE="${1:-}"

if [ -z "${NOMBRE}" ]; then
  read -r -p "Ingresa tu nombre: " NOMBRE
fi

if [ -z "${NOMBRE}" ]; then
  echo "Error: el nombre no puede estar vacío." >&2
  exit 1
fi

echo "Hola, ${NOMBRE}."
```

### Ejemplo 10: variables de entorno como parámetros

Además de los argumentos, un script puede recibir valores desde el entorno, lo que es habitual en tareas automatizadas:

```bash
#!/usr/bin/env bash

NIVEL="${NIVEL_LOG:-info}"
DIRECTORIO="${DIRECTORIO_DATOS:-/var/tmp}"

echo "Nivel de log: ${NIVEL}"
echo "Directorio:   ${DIRECTORIO}"
```

```bash
./entorno.sh                          # usa los valores por defecto
NIVEL_LOG=debug ./entorno.sh          # define la variable solo para este comando
DIRECTORIO_DATOS=~/datos NIVEL_LOG=warn ./entorno.sh
```

---

## 4. Estructuras de control

### Introducción

Las estructuras de control permiten que un script elija qué hacer según el estado de los datos: si un archivo existe, si un número es mayor que otro, qué opción pidió el usuario. Sin ellas, un script solo ejecutaría comandos de forma lineal.

### Explicación teórica

Bash ofrece tres herramientas principales para decidir:

- **`if / elif / else / fi`**: ejecuta un bloque si la condición tiene éxito (código de salida `0`).
- **`case ... esac`**: compara un valor contra varios patrones. Es más claro que muchos `elif` cuando se evalúa una misma variable.
- **Operadores `&&` y `||`**: ejecutan un comando solo si el anterior tuvo éxito (`&&`) o falló (`||`).

Las condiciones se escriben dentro de `[ ... ]` (compatible con cualquier shell POSIX) o `[[ ... ]]` (propio de Bash, más seguro y con soporte de patrones y expresiones regulares). Los espacios dentro de los corchetes son obligatorios.

| Tipo | Operador | Significado |
|------|----------|-------------|
| Archivos | `-e` / `-f` / `-d` | existe / es archivo regular / es directorio |
| Archivos | `-r` / `-w` / `-x` | tiene permiso de lectura / escritura / ejecución |
| Archivos | `-s` | existe y no está vacío |
| Texto | `=` / `!=` | igual / distinto |
| Texto | `-z` / `-n` | cadena vacía / no vacía |
| Números | `-eq` `-ne` `-lt` `-le` `-gt` `-ge` | igual, distinto, menor, menor o igual, mayor, mayor o igual |
| Lógicos | `!` / `&&` / `\|\|` | negación / y / o |

Para comparar números también puede usarse `(( ... ))`, que permite escribir `>`, `<`, `==` de forma natural.

### Ejemplo 1: `if / elif / else` con números

Guarda como `clasificar_numero.sh`:

```bash
#!/usr/bin/env bash

if [ "$#" -ne 1 ]; then
  echo "Uso: $0 NUMERO" >&2
  exit 1
fi

N="$1"

if ! [[ "${N}" =~ ^-?[0-9]+$ ]]; then
  echo "Error: '${N}' no es un número entero." >&2
  exit 1
fi

if [ "${N}" -lt 0 ]; then
  echo "${N} es negativo"
elif [ "${N}" -eq 0 ]; then
  echo "${N} es cero"
elif (( N % 2 == 0 )); then
  echo "${N} es positivo y par"
else
  echo "${N} es positivo e impar"
fi
```

Prueba con `./clasificar_numero.sh -5`, `0`, `8` y `7`.

### Ejemplo 2: tipo de una ruta

Guarda como `tipo_ruta.sh`:

```bash
#!/usr/bin/env bash

RUTA="${1:-}"

if [ -z "${RUTA}" ]; then
  echo "Uso: $0 RUTA" >&2
  exit 1
fi

if [ ! -e "${RUTA}" ]; then
  echo "${RUTA}: no existe"
elif [ -d "${RUTA}" ]; then
  echo "${RUTA}: es un directorio"
elif [ -f "${RUTA}" ]; then
  echo "${RUTA}: es un archivo regular"
  [ -x "${RUTA}" ] && echo "  - tiene permiso de ejecución"
  [ -s "${RUTA}" ] || echo "  - está vacío"
else
  echo "${RUTA}: es otro tipo de archivo (enlace roto, dispositivo, socket...)"
fi
```

Las dos últimas líneas del bloque `elif` muestran el uso de `&&` y `||` como condicionales compactos.

### Ejemplo 3: combinar condiciones

Un script que solo continúa si el usuario es `root` y el directorio de destino es escribible:

```bash
#!/usr/bin/env bash

DESTINO="${1:-/var/backups}"

if [ "$(id -u)" -ne 0 ]; then
  echo "Este script debe ejecutarse como root." >&2
  exit 1
fi

if [ -d "${DESTINO}" ] && [ -w "${DESTINO}" ]; then
  echo "Destino válido: ${DESTINO}"
else
  echo "Error: ${DESTINO} no existe o no se puede escribir en él." >&2
  exit 1
fi
```

### Ejemplo 4: menú con `case`

Guarda como `menu.sh`:

```bash
#!/usr/bin/env bash

echo "Elige una opción:"
echo "  1) Fecha y hora"
echo "  2) Espacio en disco"
echo "  3) Usuarios conectados"
echo "  s) Salir"
read -r -p "Opción: " OPCION

case "${OPCION}" in
  1)
    date
    ;;
  2)
    df -h
    ;;
  3)
    who
    ;;
  s|S|salir)
    echo "Hasta luego."
    exit 0
    ;;
  *)
    echo "Opción no válida: ${OPCION}" >&2
    exit 1
    ;;
esac
```

Cada bloque termina con `;;`. El patrón `*` actúa como "cualquier otro valor" y debe ir al final. Se pueden agrupar alternativas con `|`.

### Ejemplo 5: `case` con patrones sobre extensiones

```bash
#!/usr/bin/env bash

ARCHIVO="${1:-}"

if [ -z "${ARCHIVO}" ]; then
  echo "Uso: $0 ARCHIVO" >&2
  exit 1
fi

case "${ARCHIVO}" in
  *.tar.gz|*.tgz) echo "Archivo tar comprimido con gzip" ;;
  *.zip)          echo "Archivo ZIP" ;;
  *.sh)           echo "Script de shell" ;;
  *.log|*.txt)    echo "Archivo de texto" ;;
  *)              echo "Tipo no reconocido" ;;
esac
```

### Ejemplo 6: actuar según el resultado de un comando

El `if` evalúa directamente el código de salida de cualquier comando, no solo de `[ ... ]`:

```bash
#!/usr/bin/env bash

USUARIO="${1:-}"

if [ -z "${USUARIO}" ]; then
  echo "Uso: $0 USUARIO" >&2
  exit 1
fi

if id "${USUARIO}" >/dev/null 2>&1; then
  echo "El usuario ${USUARIO} existe."
else
  echo "El usuario ${USUARIO} no existe."
fi

if grep -q "^${USUARIO}:" /etc/passwd; then
  echo "Aparece en /etc/passwd."
fi
```

`>/dev/null 2>&1` descarta la salida normal y la de error: aquí solo interesa el código de salida. `grep -q` no imprime nada y solo devuelve éxito o fallo.

---

## 5. Iteraciones

### Introducción

Un bucle repite un bloque de comandos. Se usa para procesar listas de archivos, repetir una acción un número de veces, leer un archivo línea por línea o esperar a que se cumpla una condición.

### Explicación teórica

- **`for ... in ...; do ... done`**: recorre una lista de elementos (palabras, archivos expandidos por un patrón, el resultado de un comando).
- **`for ((inicio; condición; paso))`**: bucle numérico al estilo C.
- **`while condición; do ... done`**: repite mientras la condición tenga éxito.
- **`until condición; do ... done`**: repite hasta que la condición tenga éxito.
- **`break`** termina el bucle; **`continue`** salta a la siguiente iteración.

Las expansiones útiles para generar listas son `{1..10}` (rangos), `{a,b,c}` (alternativas) y `*.txt` (patrones de archivos). Para recorrer los argumentos del script se usa `"$@"`, que conserva cada argumento como un elemento separado.

### Ejemplo 1: `for` sobre una lista de valores

```bash
#!/usr/bin/env bash

for FRUTA in manzana pera naranja; do
  echo "Fruta: ${FRUTA}"
done
```

### Ejemplo 2: `for` con rangos y estilo C

```bash
#!/usr/bin/env bash

# Rango con llaves
for I in {1..5}; do
  echo "Iteración ${I}"
done

# Con paso de 2
for I in {0..10..2}; do
  echo "Par: ${I}"
done

# Estilo C
for ((i = 1; i <= 5; i++)); do
  echo "Cuadrado de ${i}: $((i * i))"
done
```

### Ejemplo 3: tabla de multiplicar

Guarda como `tabla.sh`:

```bash
#!/usr/bin/env bash

N="${1:-}"

if ! [[ "${N}" =~ ^[0-9]+$ ]]; then
  echo "Uso: $0 NUMERO_ENTERO" >&2
  exit 1
fi

for ((i = 1; i <= 10; i++)); do
  printf '%d x %d = %d\n' "${N}" "${i}" $((N * i))
done
```

### Ejemplo 4: recorrer los argumentos del script

```bash
#!/usr/bin/env bash

if [ "$#" -eq 0 ]; then
  echo "Uso: $0 ARCHIVO..." >&2
  exit 1
fi

for ARCHIVO in "$@"; do
  if [ -f "${ARCHIVO}" ]; then
    echo "${ARCHIVO}: $(wc -l < "${ARCHIVO}") líneas"
  else
    echo "${ARCHIVO}: no es un archivo" >&2
  fi
done
```

Siempre usa `"$@"` entre comillas: así un argumento con espacios, como `"mi archivo.txt"`, no se separa en dos.

### Ejemplo 5: `for` sobre archivos con un patrón

Renombrar de forma segura todos los `.txt` de la carpeta actual a `.bak` mostrando primero lo que haría:

```bash
#!/usr/bin/env bash

for ARCHIVO in *.txt; do
  # Si no hay coincidencias, Bash deja el patrón literal "*.txt"
  [ -e "${ARCHIVO}" ] || continue

  NUEVO="${ARCHIVO%.txt}.bak"
  echo "Copiando ${ARCHIVO} -> ${NUEVO}"
  cp -- "${ARCHIVO}" "${NUEVO}"
done
```

`${ARCHIVO%.txt}` elimina el sufijo `.txt` del valor. El `continue` evita procesar el patrón literal cuando no existe ningún `.txt`.

### Ejemplo 6: `while` con contador

```bash
#!/usr/bin/env bash

CONTADOR=3

while [ "${CONTADOR}" -gt 0 ]; do
  echo "Cuenta regresiva: ${CONTADOR}"
  CONTADOR=$((CONTADOR - 1))
  sleep 1
done

echo "¡Despegue!"
```

### Ejemplo 7: `while` para validar una entrada del usuario

```bash
#!/usr/bin/env bash

while true; do
  read -r -p "Ingresa un número entre 1 y 10: " N

  if ! [[ "${N}" =~ ^[0-9]+$ ]]; then
    echo "No es un número."
    continue
  fi

  if [ "${N}" -ge 1 ] && [ "${N}" -le 10 ]; then
    break
  fi

  echo "Fuera de rango."
done

echo "Elegiste: ${N}"
```

### Ejemplo 8: `until` para esperar un recurso

Esperar hasta que exista un archivo, con un máximo de intentos:

```bash
#!/usr/bin/env bash

ARCHIVO="${1:-/tmp/listo.flag}"
INTENTOS=0
MAXIMO=10

until [ -e "${ARCHIVO}" ]; do
  INTENTOS=$((INTENTOS + 1))

  if [ "${INTENTOS}" -gt "${MAXIMO}" ]; then
    echo "Tiempo agotado esperando ${ARCHIVO}" >&2
    exit 1
  fi

  echo "Esperando ${ARCHIVO} (intento ${INTENTOS}/${MAXIMO})..."
  sleep 2
done

echo "${ARCHIVO} disponible."
```

### Ejemplo 9: bucles anidados

```bash
#!/usr/bin/env bash

for FILA in {1..3}; do
  for COL in {1..3}; do
    printf '(%d,%d) ' "${FILA}" "${COL}"
  done
  echo
done
```

Para salir de dos niveles a la vez se puede usar `break 2`.

---

## 6. Listar carpetas y recorrer directorios

### Introducción

Gran parte de la administración de sistemas consiste en examinar el contenido de directorios: saber qué hay, filtrar por tipo o tamaño y procesar cada elemento. Bash ofrece varias formas, con distintos grados de seguridad.

### Explicación teórica

- **Globbing** (`*`, `?`, `[a-z]`): el propio Bash expande el patrón a los nombres existentes. Es la forma más segura y simple de recorrer un directorio.
- **`ls`**: útil para ver contenido en la terminal, pero **no debe usarse para alimentar un bucle** (`for f in $(ls)`), porque los nombres con espacios o caracteres especiales se rompen.
- **`find`**: recorre recursivamente y filtra por tipo, nombre, tamaño, fecha o permisos. Con `-print0` y `read -d ''` maneja cualquier nombre de archivo.
- **`shopt -s nullglob`**: hace que un patrón sin coincidencias se expanda a nada en lugar de quedar literal.
- **`shopt -s dotglob`**: incluye en `*` los archivos ocultos (los que empiezan con punto).

Opciones útiles de `ls`: `-l` (formato largo), `-a` (incluye ocultos), `-h` (tamaños legibles), `-t` (ordena por fecha), `-S` (ordena por tamaño), `-R` (recursivo).

### Ejemplo 1: comandos básicos de listado

```bash
ls                 # contenido del directorio actual
ls -lah /etc       # formato largo, ocultos y tamaños legibles
ls -lt | head -5   # los 5 elementos más recientes
ls -d */           # solo los subdirectorios
ls -lS ~/Descargas # ordenado por tamaño, de mayor a menor
```

### Ejemplo 2: recorrer un directorio con globbing

Guarda como `listar_directorio.sh`:

```bash
#!/usr/bin/env bash

DIRECTORIO="${1:-.}"

if [ ! -d "${DIRECTORIO}" ]; then
  echo "Error: ${DIRECTORIO} no es un directorio." >&2
  exit 1
fi

shopt -s nullglob

for ELEMENTO in "${DIRECTORIO}"/*; do
  if [ -d "${ELEMENTO}" ]; then
    echo "[DIR]     $(basename "${ELEMENTO}")"
  elif [ -f "${ELEMENTO}" ]; then
    echo "[ARCHIVO] $(basename "${ELEMENTO}")"
  else
    echo "[OTRO]    $(basename "${ELEMENTO}")"
  fi
done
```

Observa que la ruta del directorio va entre comillas, pero el `*` queda fuera de ellas para que Bash lo expanda.

### Ejemplo 3: solo subdirectorios con su tamaño

```bash
#!/usr/bin/env bash

DIRECTORIO="${1:-.}"

shopt -s nullglob

for SUBDIR in "${DIRECTORIO}"/*/; do
  TAMANO=$(du -sh "${SUBDIR}" 2>/dev/null | cut -f1)
  echo "${TAMANO}	${SUBDIR}"
done
```

El patrón `*/` (con barra final) coincide solo con directorios. Para ordenar el resultado de mayor a menor, redirige la salida a `sort -hr`.

### Ejemplo 4: incluir archivos ocultos y contar

```bash
#!/usr/bin/env bash

DIRECTORIO="${1:-.}"

shopt -s nullglob dotglob

ARCHIVOS=0
DIRECTORIOS=0

for ELEMENTO in "${DIRECTORIO}"/*; do
  if [ -d "${ELEMENTO}" ]; then
    DIRECTORIOS=$((DIRECTORIOS + 1))
  elif [ -f "${ELEMENTO}" ]; then
    ARCHIVOS=$((ARCHIVOS + 1))
  fi
done

echo "En ${DIRECTORIO}: ${ARCHIVOS} archivos y ${DIRECTORIOS} directorios (incluye ocultos)."
```

### Ejemplo 5: recorrido recursivo con `find`

```bash
# Todos los archivos .sh bajo el directorio actual
find . -type f -name '*.sh'

# Solo directorios hasta 2 niveles de profundidad
find . -maxdepth 2 -type d

# Archivos mayores a 100 MB
find ~ -type f -size +100M

# Archivos modificados en las últimas 24 horas
find . -type f -mtime -1
```

### Ejemplo 6: `find` combinado con un bucle seguro

Guarda como `buscar_grandes.sh`:

```bash
#!/usr/bin/env bash

DIRECTORIO="${1:-.}"
TAMANO="${2:-50M}"

if [ ! -d "${DIRECTORIO}" ]; then
  echo "Error: ${DIRECTORIO} no es un directorio." >&2
  exit 1
fi

echo "Archivos en ${DIRECTORIO} mayores a ${TAMANO}:"

while IFS= read -r -d '' ARCHIVO; do
  printf '%s\t%s\n' "$(du -h "${ARCHIVO}" | cut -f1)" "${ARCHIVO}"
done < <(find "${DIRECTORIO}" -type f -size "+${TAMANO}" -print0)
```

Ejecuta, por ejemplo, `./buscar_grandes.sh ~ 100M`.

### Ejemplo 7: árbol de directorios simple

```bash
#!/usr/bin/env bash

mostrar() {
  local DIR="$1"
  local PREFIJO="$2"

  shopt -s nullglob
  for ELEMENTO in "${DIR}"/*; do
    echo "${PREFIJO}$(basename "${ELEMENTO}")"
    if [ -d "${ELEMENTO}" ]; then
      mostrar "${ELEMENTO}" "${PREFIJO}    "
    fi
  done
}

RAIZ="${1:-.}"
echo "${RAIZ}"
mostrar "${RAIZ}" "  "
```

Es una función recursiva: se llama a sí misma por cada subdirectorio, aumentando la sangría. Si el sistema tiene el comando `tree`, produce un resultado equivalente con mejor formato.

### Ejemplo 8: contar archivos por extensión

```bash
#!/usr/bin/env bash

DIRECTORIO="${1:-.}"

find "${DIRECTORIO}" -type f -name '*.*' \
  | sed 's/.*\.//' \
  | sort \
  | uniq -c \
  | sort -rn
```

Muestra cuántos archivos hay de cada extensión, de la más frecuente a la menos frecuente.

---

## 7. Abrir y leer archivos

### Introducción

Un script suele necesitar leer datos de un archivo (configuraciones, listas de usuarios, registros) o escribir resultados en uno. En Bash no se "abre" un archivo como en otros lenguajes: se redirige su contenido hacia un comando, o se asocia a un descriptor de archivo.

### Explicación teórica

Las formas más habituales de leer un archivo son:

- **`cat archivo`**: imprime todo el contenido. Sirve para archivos pequeños.
- **`head -n N` / `tail -n N`**: primeras o últimas `N` líneas. `tail -f` sigue un archivo mientras crece.
- **`while read ... done < archivo`**: lee el archivo línea por línea. Es la forma recomendada para procesarlo.
- **`mapfile -t arreglo < archivo`**: carga todas las líneas en un arreglo.
- **`grep`, `awk`, `cut`, `wc`**: filtran o extraen datos sin escribir un bucle.

Redirecciones relevantes:

| Operador | Efecto |
|----------|--------|
| `< archivo` | entrega el archivo como entrada estándar |
| `> archivo` | escribe la salida, reemplazando el contenido |
| `>> archivo` | agrega la salida al final |
| `2> archivo` | redirige la salida de error |
| `exec 3< archivo` | abre el archivo en el descriptor 3 para lecturas repetidas |

En `read`, la opción `-r` evita que se interpreten las barras invertidas, y `IFS=` evita que se recorten los espacios al inicio y final de cada línea.

Antes de abrir un archivo conviene comprobar que existe (`-f`) y que se puede leer (`-r`).

### Ejemplo 1: mostrar un archivo con validación

Guarda como `mostrar_archivo.sh`:

```bash
#!/usr/bin/env bash

ARCHIVO="${1:-}"

if [ -z "${ARCHIVO}" ]; then
  echo "Uso: $0 ARCHIVO" >&2
  exit 1
fi

if [ ! -f "${ARCHIVO}" ]; then
  echo "Error: ${ARCHIVO} no existe o no es un archivo regular." >&2
  exit 1
fi

if [ ! -r "${ARCHIVO}" ]; then
  echo "Error: no hay permiso de lectura sobre ${ARCHIVO}." >&2
  exit 1
fi

cat "${ARCHIVO}"
```

### Ejemplo 2: leer línea por línea con numeración

```bash
#!/usr/bin/env bash

ARCHIVO="${1:-}"

if [ ! -r "${ARCHIVO}" ]; then
  echo "Uso: $0 ARCHIVO_LEGIBLE" >&2
  exit 1
fi

NUMERO=0

while IFS= read -r LINEA; do
  NUMERO=$((NUMERO + 1))
  printf '%3d: %s\n' "${NUMERO}" "${LINEA}"
done < "${ARCHIVO}"
```

La redirección `< "${ARCHIVO}"` al final del `while` hace que `read` obtenga cada línea del archivo. Si el archivo no termina con salto de línea, la última línea podría omitirse; para cubrir ese caso se usa `while IFS= read -r LINEA || [ -n "${LINEA}" ]; do`.

### Ejemplo 3: procesar un archivo con campos separados

`/etc/passwd` separa sus campos con `:`. Se puede indicar el separador mediante `IFS`:

```bash
#!/usr/bin/env bash

while IFS=: read -r USUARIO _ UID_NUM _ _ HOGAR SHELL_USUARIO; do
  if [ "${UID_NUM}" -ge 1000 ]; then
    echo "Usuario: ${USUARIO} | Hogar: ${HOGAR} | Shell: ${SHELL_USUARIO}"
  fi
done < /etc/passwd
```

Se imprimen solo las cuentas con UID mayor o igual a 1000 (usuarios normales en la mayoría de las distribuciones Linux). El guion bajo `_` descarta los campos que no interesan.

### Ejemplo 4: leer un CSV simple

Suponiendo `alumnos.csv`:

```
nombre,grupo,nota
Ana,3A,9
Luis,3B,7
Marta,3A,10
```

```bash
#!/usr/bin/env bash

ARCHIVO="alumnos.csv"

# tail -n +2 omite la línea de encabezado
tail -n +2 "${ARCHIVO}" | while IFS=, read -r NOMBRE GRUPO NOTA; do
  if [ "${NOTA}" -ge 9 ]; then
    echo "${NOMBRE} (${GRUPO}) aprobó con distinción: ${NOTA}"
  fi
done
```

Este enfoque sirve para datos sencillos. Si los campos pueden contener comas entre comillas, conviene usar una herramienta específica para CSV.

### Ejemplo 5: cargar el archivo en un arreglo

```bash
#!/usr/bin/env bash

ARCHIVO="${1:-}"

if [ ! -r "${ARCHIVO}" ]; then
  echo "Uso: $0 ARCHIVO_LEGIBLE" >&2
  exit 1
fi

mapfile -t LINEAS < "${ARCHIVO}"

echo "El archivo tiene ${#LINEAS[@]} líneas."
echo "Primera línea: ${LINEAS[0]}"
echo "Última línea:  ${LINEAS[-1]}"

for LINEA in "${LINEAS[@]}"; do
  echo "> ${LINEA}"
done
```

### Ejemplo 6: leer una lista de archivos y verificarlos

Dado `lista.txt` con una ruta por línea:

```bash
#!/usr/bin/env bash

LISTA="${1:-lista.txt}"

if [ ! -r "${LISTA}" ]; then
  echo "No se puede leer ${LISTA}" >&2
  exit 1
fi

while IFS= read -r RUTA; do
  # Ignorar líneas vacías y comentarios
  [ -z "${RUTA}" ] && continue
  [[ "${RUTA}" == \#* ]] && continue

  if [ -e "${RUTA}" ]; then
    echo "OK       ${RUTA}"
  else
    echo "FALTANTE ${RUTA}"
  fi
done < "${LISTA}"
```

### Ejemplo 7: escribir y agregar contenido a un archivo

```bash
#!/usr/bin/env bash

REGISTRO="registro.log"

# > crea o reemplaza el archivo
echo "=== Inicio del registro ===" > "${REGISTRO}"

# >> agrega al final
echo "$(date '+%F %T') - Script iniciado" >> "${REGISTRO}"
echo "$(date '+%F %T') - Usuario: $(whoami)" >> "${REGISTRO}"

# Varias líneas a la vez con un here-document
cat >> "${REGISTRO}" <<EOF
$(date '+%F %T') - Host: $(hostname)
$(date '+%F %T') - Directorio: $(pwd)
EOF

echo "Contenido de ${REGISTRO}:"
cat "${REGISTRO}"
```

### Ejemplo 8: abrir un archivo con un descriptor

Cuando se necesita leer de un mismo archivo en distintos momentos del script, se puede abrir una vez en un descriptor y leer de él cuando sea necesario:

```bash
#!/usr/bin/env bash

ARCHIVO="${1:-}"

if [ ! -r "${ARCHIVO}" ]; then
  echo "Uso: $0 ARCHIVO_LEGIBLE" >&2
  exit 1
fi

exec 3< "${ARCHIVO}"     # abre el archivo para lectura en el descriptor 3

read -r -u 3 PRIMERA
read -r -u 3 SEGUNDA

echo "Línea 1: ${PRIMERA}"
echo "Línea 2: ${SEGUNDA}"

exec 3<&-                # cierra el descriptor
```

Cada `read -u 3` continúa donde terminó la lectura anterior. Para escribir, `exec 4> salida.txt` abre un descriptor de escritura que se usa con `echo "texto" >&4`.

### Ejemplo 9: extraer información sin escribir un bucle

```bash
ARCHIVO=/var/log/auth.log

grep -c "Failed password" "${ARCHIVO}"        # cuántos intentos fallidos
grep "Failed password" "${ARCHIVO}" | tail -5 # los últimos 5
wc -l < "${ARCHIVO}"                          # número de líneas
cut -d: -f1 /etc/passwd | sort                # solo los nombres de usuario
awk -F: '$3 >= 1000 {print $1}' /etc/passwd   # usuarios normales
```

Estas herramientas están pensadas para filtrar texto y suelen ser más cortas y rápidas que un bucle `while read` cuando el archivo es grande.

### Ejemplo 10: seguir un archivo de registro en vivo

```bash
#!/usr/bin/env bash

ARCHIVO="${1:-}"

if [ ! -r "${ARCHIVO}" ]; then
  echo "Uso: $0 ARCHIVO_LEGIBLE" >&2
  exit 1
fi

# -F sigue el archivo aunque sea rotado; -n 0 empieza desde el final
tail -n 0 -F "${ARCHIVO}" | while IFS= read -r LINEA; do
  if [[ "${LINEA}" == *ERROR* ]]; then
    echo "⚠ $(date '+%T') ${LINEA}"
  fi
done
```

Presiona `Ctrl+C` para detener el seguimiento.

---

## 8. Información básica del sistema

### Introducción

Un script puede reunir datos que normalmente consultaríamos con varios comandos: equipo, sistema operativo, tiempo de actividad, memoria y espacio disponible.

### Explicación teórica

`uname` informa sobre el kernel; `/etc/os-release` describe la distribución; `uptime` muestra cuánto tiempo lleva encendido el sistema y su carga; `free -h` resume la memoria; `df -h` informa del uso de los sistemas de archivos montados. El operador de redirección `>` crea o reemplaza un archivo, mientras que `>>` agrega contenido al final.

### Ejemplo práctico: informe del sistema

Guarda este script como `informe_sistema.sh`:

```bash
#!/usr/bin/env bash

ARCHIVO="${1:-informe_sistema_$(date +%Y%m%d_%H%M%S).txt}"

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
} > "${ARCHIVO}"

echo "Informe creado en: ${ARCHIVO}"
```

Ejecuta `./informe_sistema.sh`. Para elegir el nombre del informe, proporciona una ruta como primer argumento:

```bash
./informe_sistema.sh /tmp/estado_servidor.txt
```

La agrupación entre llaves permite redirigir toda la salida al mismo archivo. El fragmento `2>/dev/null` oculta el mensaje de error si `hostnamectl` no está disponible.

---

## 9. Decisiones y validación de entradas

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

DIRECTORIO="$1"

if [ ! -d "${DIRECTORIO}" ]; then
  echo "Error: no existe el directorio: ${DIRECTORIO}" >&2
  exit 1
fi

CANTIDAD=$(find "${DIRECTORIO}" -maxdepth 1 -type f | wc -l)
echo "Archivos regulares en ${DIRECTORIO}: ${CANTIDAD}"
```

Prueba con `./contar_archivos.sh /etc`. El texto `>&2` envía mensajes de uso y errores a la salida de error estándar, separándolos de la salida normal del script.

---

## 10. Repetición y procesamiento de archivos

### Introducción

Los bucles permiten aplicar una acción a una colección de archivos. Antes de modificar archivos, conviene mostrar la acción que se realizará.

### Explicación teórica

`for` repite un bloque para cada elemento de una lista. `find` localiza elementos en una ruta usando condiciones como el tipo (`-type f`) o la extensión (`-name '*.log'`). Cuando se trabaja con nombres que pueden incluir espacios, usa `find ... -print0` junto con `read -d ''` para conservar cada nombre intacto.

### Ejemplo práctico: listar archivos de registro

Guarda el contenido en `listar_logs.sh`:

```bash
#!/usr/bin/env bash

DIRECTORIO="${1:-.}"

if [ ! -d "${DIRECTORIO}" ]; then
  echo "Error: ${DIRECTORIO} no es un directorio válido." >&2
  exit 1
fi

ENCONTRADOS=0
while IFS= read -r -d '' ARCHIVO; do
  printf '%s\n' "${ARCHIVO}"
  ENCONTRADOS=$((ENCONTRADOS + 1))
done < <(find "${DIRECTORIO}" -type f -name '*.log' -print0)

echo "Total de archivos .log: ${ENCONTRADOS}"
```

Ejecuta `./listar_logs.sh /var/log`. Si no se proporciona una ruta, se examina el directorio actual. La construcción `< <(...)` entrega la salida de `find` al bucle sin crear un archivo temporal.

---

## 11. Crear backups comprimidos

### Introducción

Un backup comprimido agrupa el contenido de una ruta en un único archivo. `tar` conserva la estructura de directorios y, con gzip, reduce el espacio utilizado.

### Explicación teórica

Las opciones más usadas son `-c` para crear un archivo, `-z` para comprimir con gzip, `-f` para indicar el nombre resultante y `-v` para mostrar los archivos procesados. La extensión convencional es `.tar.gz`.

Un backup debe almacenarse fuera del directorio de origen; de esa forma no se incluirá a sí mismo ni se perderá junto con la información respaldada. Antes de crear uno, se valida la ruta origen y se crea el directorio de destino si no existe.

### Ejemplo práctico: backup con fecha

Guarda este script como `backup_comprimido.sh`:

```bash
#!/usr/bin/env bash

ORIGEN="${1:-}"
DESTINO="${2:-$HOME/backups}"

if [ -z "${ORIGEN}" ] || [ ! -d "${ORIGEN}" ]; then
  echo "Uso: $0 DIRECTORIO_ORIGEN [DIRECTORIO_DESTINO]" >&2
  exit 1
fi

mkdir -p "${DESTINO}"
nombre=$(basename "${ORIGEN%/}")
fecha=$(date +%Y%m%d_%H%M%S)
archivo="${DESTINO}/${nombre}_${fecha}.tar.gz"

tar -czf "${archivo}" -C "$(dirname "${ORIGEN%/}")" "${nombre}"

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

## 12. Limpiar logs anteriores a una fecha

### Introducción

Los archivos de registro pueden acumularse con el tiempo. La limpieza debe comenzar mostrando qué archivos coinciden; solo después se debe habilitar su eliminación.

### Explicación teórica

`find` admite referencias temporales. La opción `-mtime +N` selecciona archivos modificados hace más de `N` períodos completos de 24 horas. Por ejemplo, `-mtime +30` no equivale exactamente a una fecha del calendario: selecciona archivos con más de 31 períodos completos de 24 horas.

En sistemas GNU, `-not -newermt 'AAAA-MM-DD'` selecciona archivos cuya última modificación es anterior a esa fecha a las 00:00. Como puede variar entre implementaciones de `find`, verifica que el comando funcione en el sistema donde lo ejecutarás.

### Ejemplo práctico: vista previa y eliminación confirmada

Guarda este contenido como `limpiar_logs_por_fecha.sh`:

```bash
#!/usr/bin/env bash

DIRECTORIO="${1:-}"
fecha_limite="${2:-}"

if [ -z "${DIRECTORIO}" ] || [ -z "${fecha_limite}" ] || [ ! -d "${DIRECTORIO}" ]; then
  echo "Uso: $0 DIRECTORIO_LOGS AAAA-MM-DD" >&2
  exit 1
fi

mapfile -d '' archivos < <(
  find "${DIRECTORIO}" -type f -name '*.log' -not -newermt "${fecha_limite}" -print0
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

## 13. Limpiar archivos temporales de forma controlada

### Introducción

Los directorios temporales de una aplicación pueden contener archivos que ya no son necesarios. Una limpieza segura restringe el tipo de archivo, el directorio y la antigüedad, y permite una ejecución de prueba.

### Explicación teórica

El script siguiente acepta una ruta, una antigüedad en días y una opción `--aplicar`. Sin esa opción solo muestra los archivos seleccionados. Este patrón se conoce como *dry run*: permite revisar el alcance real antes de modificar datos.

No uses este procedimiento sobre `/tmp` ni sobre rutas administradas por el sistema sin conocer qué procesos las utilizan. Trabajaremos sobre un directorio temporal perteneciente a una aplicación concreta.

### Ejemplo práctico: limpieza con modo de prueba

Guarda este contenido en `limpiar_temporales.sh`:

```bash
#!/usr/bin/env bash

DIRECTORIO="${1:-}"
DIAS="${2:-}"
MODO="${3:-}"

if [ -z "${DIRECTORIO}" ] || [ -z "${DIAS}" ] || [ ! -d "${DIRECTORIO}" ]; then
  echo "Uso: $0 DIRECTORIO DIAS [--aplicar]" >&2
  exit 1
fi

if ! [[ "${DIAS}" =~ ^[0-9]+$ ]]; then
  echo "Error: DIAS debe ser un número entero no negativo." >&2
  exit 1
fi

echo "Archivos temporales con más de ${DIAS} días:"
find "${DIRECTORIO}" -type f \( -name '*.tmp' -o -name '*.temp' \) -mtime "+${DIAS}" -print

if [ "${MODO}" = "--aplicar" ]; then
  find "${DIRECTORIO}" -type f \( -name '*.tmp' -o -name '*.temp' \) -mtime "+${DIAS}" -delete
  echo "Limpieza aplicada."
else
  echo "Vista previa finalizada. Para borrar, agrega --aplicar al final."
fi
```

Ejecuta primero el MODO de prueba:

```bash
./limpiar_temporales.sh ~/mi_aplicacion/tmp 14
```

Si la lista es correcta, aplica la limpieza:

```bash
./limpiar_temporales.sh ~/mi_aplicacion/tmp 14 --aplicar
```

---

## 14. Recomendaciones para scripts confiables

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

Usa esta plantilla como punto de partida para un script que reciba un DIRECTORIO:

```bash
#!/usr/bin/env bash
set -euo pipefail

if [ "$#" -ne 1 ]; then
  echo "Uso: $0 DIRECTORIO" >&2
  exit 1
fi

DIRECTORIO="$1"

if [ ! -d "${DIRECTORIO}" ]; then
  echo "Error: no existe el directorio: ${DIRECTORIO}" >&2
  exit 1
fi

echo "Procesando: ${DIRECTORIO}"
# Agrega aquí la tarea específica.
```

Con esta estructura, haremos que cada nueva automatización tenga una entrada clara, validaciones iniciales y un comportamiento verificable antes de afectar archivos.

---

## 15. Scripts prácticos de administración

### Introducción

Esta sección reúne scripts completos de tareas habituales de un administrador: migrar y respaldar información, mantener el sistema limpio y actualizado, gestionar usuarios y observar el estado del equipo. Combinan lo visto en las secciones anteriores: parámetros, condicionales, bucles, manejo de directorios y archivos.

### Explicación teórica

Todos los scripts siguen las mismas reglas de seguridad:

- **Modo de prueba por defecto.** Los que modifican o borran datos solo muestran lo que harían; para ejecutarlos de verdad se agrega `--aplicar`.
- **Validación antes de actuar.** Se comprueban argumentos, rutas y permisos (por ejemplo, que se ejecute como `root` cuando hace falta).
- **Registro de operaciones.** Las tareas importantes escriben en un archivo de log con fecha.
- **`set -euo pipefail`** para detener el script ante errores no previstos.

Los ejemplos asumen un sistema GNU/Linux de la familia Debian/Ubuntu (usan `apt`); en otras distribuciones cambia el gestor de paquetes. Varios requieren privilegios de administrador y se ejecutan con `sudo`.

> **Precaución:** prueba cada script primero en una máquina virtual o con carpetas de ejemplo. Los que crean usuarios, borran archivos o actualizan paquetes modifican el sistema real.

### Funciones comunes

Varios scripts reutilizan estas funciones. Guárdalas en `lib_comun.sh` y cárgalas con `source "$(dirname "$0")/lib_comun.sh"`:

```bash
#!/usr/bin/env bash
# lib_comun.sh - funciones auxiliares compartidas

# Escribe un mensaje con fecha en pantalla y, si LOG está definida, en el archivo
log() {
  local MENSAJE
  MENSAJE="$(date '+%Y-%m-%d %H:%M:%S') $*"
  echo "${MENSAJE}"
  if [ -n "${LOG:-}" ]; then
    echo "${MENSAJE}" >> "${LOG}"
  fi
}

# Termina el script con un mensaje de error
error() {
  echo "Error: $*" >&2
  exit 1
}

# Exige privilegios de administrador
requiere_root() {
  [ "$(id -u)" -eq 0 ] || error "ejecuta este script con sudo."
}
```

---

### 15.1 Migrar el contenido de una carpeta

**Objetivo:** copiar todo el contenido de una carpeta a otra conservando permisos, fechas y propietarios, y verificar el resultado. Usa `rsync`, que solo transfiere lo que cambió y permite simular la operación.

Guarda como `migrar_carpeta.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail

USO="Uso: $0 ORIGEN DESTINO [--aplicar]"

ORIGEN="${1:-}"
DESTINO="${2:-}"
MODO="${3:-}"

[ -n "${ORIGEN}" ] && [ -n "${DESTINO}" ] || { echo "${USO}" >&2; exit 1; }
[ -d "${ORIGEN}" ] || { echo "Error: ${ORIGEN} no es un directorio." >&2; exit 1; }
command -v rsync >/dev/null || { echo "Error: instala rsync." >&2; exit 1; }

# La barra final en el origen copia el CONTENIDO, no la carpeta en sí
ORIGEN="${ORIGEN%/}/"
DESTINO="${DESTINO%/}/"

# Impide migrar una carpeta dentro de sí misma
case "$(realpath -m "${DESTINO}")/" in
  "$(realpath "${ORIGEN}")"/*) echo "Error: el destino está dentro del origen." >&2; exit 1 ;;
esac

if [ "${MODO}" != "--aplicar" ]; then
  echo "== SIMULACIÓN (no se copia nada) =="
  rsync -a --dry-run --itemize-changes --stats "${ORIGEN}" "${DESTINO}"
  echo
  echo "Para ejecutar la migración agrega --aplicar al final."
  exit 0
fi

mkdir -p "${DESTINO}"
echo "Migrando ${ORIGEN} -> ${DESTINO}"
rsync -a --info=progress2 "${ORIGEN}" "${DESTINO}"

# Verificación: compara el contenido con sumas de verificación (no modifica nada)
echo "Verificando..."
DIFERENCIAS=$(rsync -a --dry-run --checksum --itemize-changes "${ORIGEN}" "${DESTINO}" | wc -l)

if [ "${DIFERENCIAS}" -eq 0 ]; then
  echo "Migración verificada: origen y destino son idénticos."
else
  echo "Atención: se detectaron ${DIFERENCIAS} diferencias." >&2
  exit 1
fi
```

Uso:

```bash
./migrar_carpeta.sh ~/proyectos /mnt/disco_externo/proyectos           # simulación
./migrar_carpeta.sh ~/proyectos /mnt/disco_externo/proyectos --aplicar # migración real
```

El script no borra el origen: eso debe hacerse manualmente después de comprobar que el destino es correcto. Para que el destino sea un espejo exacto (eliminando lo que ya no exista en el origen) se agrega `--delete` a `rsync`, pero solo después de entender sus efectos.

---

### 15.2 Inventariar carpetas

**Objetivo:** generar un informe con el contenido de cada subcarpeta: cantidad de archivos, tamaño total y fecha de la última modificación. Se guarda en formato CSV, que puede abrirse en una hoja de cálculo.

Guarda como `inventario_carpetas.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail

RAIZ="${1:-}"
SALIDA="${2:-inventario_$(date +%Y%m%d_%H%M%S).csv}"

[ -n "${RAIZ}" ] && [ -d "${RAIZ}" ] || { echo "Uso: $0 DIRECTORIO [ARCHIVO.csv]" >&2; exit 1; }

shopt -s nullglob dotglob

echo "carpeta,archivos,subcarpetas,tamano_bytes,tamano_legible,ultima_modificacion" > "${SALIDA}"

for SUBDIR in "${RAIZ%/}"/*/; do
  SUBDIR="${SUBDIR%/}"

  ARCHIVOS=$(find "${SUBDIR}" -type f | wc -l)
  CARPETAS=$(find "${SUBDIR}" -mindepth 1 -type d | wc -l)
  BYTES=$(du -sb "${SUBDIR}" 2>/dev/null | cut -f1)
  LEGIBLE=$(du -sh "${SUBDIR}" 2>/dev/null | cut -f1)

  # Archivo modificado más recientemente (fecha en formato legible)
  ULTIMA=$(find "${SUBDIR}" -type f -printf '%TY-%Tm-%Td %TH:%TM\n' 2>/dev/null | sort | tail -n 1)
  ULTIMA="${ULTIMA:-sin archivos}"

  echo "\"$(basename "${SUBDIR}")\",${ARCHIVOS},${CARPETAS},${BYTES},${LEGIBLE},${ULTIMA}" >> "${SALIDA}"
done

echo "Inventario guardado en ${SALIDA}"
echo
column -s, -t "${SALIDA}"
```

Para listar además los 10 archivos más grandes de toda la ruta:

```bash
find "${RAIZ}" -type f -printf '%s\t%p\n' | sort -rn | head -10 | numfmt --to=iec --field=1
```

`find -printf` es una opción de GNU; en macOS se necesitaría `gfind` (paquete `findutils`).

---

### 15.3 Backups con rotación

**Objetivo:** crear un backup comprimido con fecha y eliminar automáticamente los más antiguos para que no ocupen espacio indefinidamente. Amplía el script de la sección 11 con registro y retención.

Guarda como `backup_rotativo.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail

ORIGEN="${1:-}"
DESTINO="${2:-$HOME/backups}"
CONSERVAR="${3:-7}"             # cantidad de backups a conservar

[ -n "${ORIGEN}" ] && [ -d "${ORIGEN}" ] || {
  echo "Uso: $0 ORIGEN [DESTINO] [CANTIDAD_A_CONSERVAR]" >&2; exit 1; }
[[ "${CONSERVAR}" =~ ^[1-9][0-9]*$ ]] || { echo "Error: CANTIDAD debe ser un entero positivo." >&2; exit 1; }

mkdir -p "${DESTINO}"
LOG="${DESTINO}/backup.log"

NOMBRE=$(basename "${ORIGEN%/}")
ARCHIVO="${DESTINO}/${NOMBRE}_$(date +%Y%m%d_%H%M%S).tar.gz"

echo "$(date '+%F %T') Inicio backup de ${ORIGEN}" >> "${LOG}"

tar -czf "${ARCHIVO}" -C "$(dirname "${ORIGEN%/}")" "${NOMBRE}"

# Comprueba que el archivo generado se pueda leer
if tar -tzf "${ARCHIVO}" >/dev/null 2>&1; then
  TAM=$(du -h "${ARCHIVO}" | cut -f1)
  echo "$(date '+%F %T') OK ${ARCHIVO} (${TAM})" | tee -a "${LOG}"
else
  echo "$(date '+%F %T') ERROR: backup corrupto, se elimina ${ARCHIVO}" | tee -a "${LOG}" >&2
  rm -f -- "${ARCHIVO}"
  exit 1
fi

# Rotación: ordena por nombre (la fecha está en él) y borra los más viejos
mapfile -t ANTIGUOS < <(
  ls -1 "${DESTINO}/${NOMBRE}"_*.tar.gz 2>/dev/null | sort | head -n -"${CONSERVAR}"
)

for VIEJO in "${ANTIGUOS[@]}"; do
  rm -f -- "${VIEJO}"
  echo "$(date '+%F %T') Eliminado por rotación: ${VIEJO}" | tee -a "${LOG}"
done
```

Uso:

```bash
./backup_rotativo.sh ~/Documentos ~/backups 5
```

Para ejecutarlo automáticamente cada día a las 02:00, se agrega una línea con `crontab -e`:

```
0 2 * * * /home/usuario/scripts-bash/backup_rotativo.sh /home/usuario/Documentos /home/usuario/backups 7
```

En `cron` conviene usar siempre rutas absolutas.

---

### 15.4 Restaurar un backup

**Objetivo:** listar los backups disponibles, elegir uno y restaurarlo en una carpeta de destino sin sobrescribir datos existentes sin confirmación.

Guarda como `restaurar_backup.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail

if [ "$#" -eq 1 ]; then
  # Un solo argumento: es el destino y los backups están en ~/backups
  DIR_BACKUPS="$HOME/backups"
  DESTINO="$1"
else
  DIR_BACKUPS="${1:-$HOME/backups}"
  DESTINO="${2:-}"
fi

[ -n "${DESTINO}" ] || { echo "Uso: $0 [DIR_BACKUPS] DIRECTORIO_DESTINO" >&2; exit 1; }
[ -d "${DIR_BACKUPS}" ] || { echo "Error: no existe ${DIR_BACKUPS}" >&2; exit 1; }

shopt -s nullglob
BACKUPS=("${DIR_BACKUPS}"/*.tar.gz)

[ "${#BACKUPS[@]}" -gt 0 ] || { echo "No hay backups en ${DIR_BACKUPS}" >&2; exit 1; }

echo "Backups disponibles:"
i=1
for B in "${BACKUPS[@]}"; do
  printf '  %2d) %s (%s)\n' "${i}" "$(basename "${B}")" "$(du -h "${B}" | cut -f1)"
  i=$((i + 1))
done

read -r -p "Número de backup a restaurar: " ELECCION

if ! [[ "${ELECCION}" =~ ^[0-9]+$ ]] || [ "${ELECCION}" -lt 1 ] || [ "${ELECCION}" -gt "${#BACKUPS[@]}" ]; then
  echo "Elección inválida." >&2
  exit 1
fi

ARCHIVO="${BACKUPS[$((ELECCION - 1))]}"

echo "Verificando integridad de ${ARCHIVO}..."
tar -tzf "${ARCHIVO}" >/dev/null || { echo "El backup está dañado." >&2; exit 1; }

echo "Primeros elementos del backup:"
tar -tzf "${ARCHIVO}" | head -10

mkdir -p "${DESTINO}"

# Si el destino ya tiene contenido, pide confirmación explícita
if [ -n "$(ls -A "${DESTINO}")" ]; then
  echo "Atención: ${DESTINO} no está vacío; los archivos con el mismo nombre serán reemplazados."
  read -r -p "Escribe RESTAURAR para continuar: " CONFIRMA
  [ "${CONFIRMA}" = "RESTAURAR" ] || { echo "Operación cancelada."; exit 0; }
fi

tar -xzf "${ARCHIVO}" -C "${DESTINO}"
echo "Restauración completada en ${DESTINO}"
```

Uso:

```bash
./restaurar_backup.sh ~/backups ~/restauracion
```

Se recomienda restaurar primero en una carpeta distinta de la original, comparar el contenido y recién entonces copiar lo necesario. Para recuperar un solo archivo: `tar -xzf backup.tar.gz -C destino ruta/dentro/del/backup.txt`.

---

### 15.5 Limpiar logs del sistema

**Objetivo:** liberar espacio de los registros sin perder los recientes: comprime los logs de más de cierto tiempo, elimina los muy antiguos y reduce el diario de `systemd`. Opera en modo de prueba salvo que se indique `--aplicar`.

Guarda como `limpiar_logs_sistema.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail
source "$(dirname "$0")/lib_comun.sh"

DIAS_COMPRIMIR="${1:-7}"
DIAS_BORRAR="${2:-60}"
MODO="${3:-}"
DIR_LOGS="/var/log"

requiere_root
[[ "${DIAS_COMPRIMIR}" =~ ^[0-9]+$ && "${DIAS_BORRAR}" =~ ^[0-9]+$ ]] \
  || error "los días deben ser números enteros."
[ "${DIAS_BORRAR}" -gt "${DIAS_COMPRIMIR}" ] \
  || error "DIAS_BORRAR debe ser mayor que DIAS_COMPRIMIR."

LOG="/var/log/limpieza_logs.log"

echo "Espacio usado en ${DIR_LOGS} antes: $(du -sh "${DIR_LOGS}" 2>/dev/null | cut -f1)"

echo
echo "== Logs .log sin comprimir con más de ${DIAS_COMPRIMIR} días =="
find "${DIR_LOGS}" -xdev -type f -name '*.log' -mtime "+${DIAS_COMPRIMIR}" -print

echo
echo "== Logs comprimidos (.gz) con más de ${DIAS_BORRAR} días =="
find "${DIR_LOGS}" -xdev -type f -name '*.gz' -mtime "+${DIAS_BORRAR}" -print

if [ "${MODO}" != "--aplicar" ]; then
  echo
  echo "Simulación terminada. Agrega --aplicar para ejecutar la limpieza."
  exit 0
fi

log "Comprimiendo logs de más de ${DIAS_COMPRIMIR} días"
find "${DIR_LOGS}" -xdev -type f -name '*.log' -mtime "+${DIAS_COMPRIMIR}" -exec gzip -f {} \;

log "Eliminando logs comprimidos de más de ${DIAS_BORRAR} días"
find "${DIR_LOGS}" -xdev -type f -name '*.gz' -mtime "+${DIAS_BORRAR}" -delete

if command -v journalctl >/dev/null; then
  log "Reduciendo el diario de systemd a ${DIAS_BORRAR} días"
  journalctl --vacuum-time="${DIAS_BORRAR}d"
fi

echo "Espacio usado en ${DIR_LOGS} después: $(du -sh "${DIR_LOGS}" 2>/dev/null | cut -f1)"
log "Limpieza de logs finalizada"
```

Uso:

```bash
sudo ./limpiar_logs_sistema.sh 7 60            # simulación
sudo ./limpiar_logs_sistema.sh 7 60 --aplicar  # ejecución
```

El script no toca el log activo de un servicio que se modificó recientemente. Antes de borrar logs de un servicio, revisa su política de retención o auditoría; en muchos casos es preferible configurar `logrotate`.

---

### 15.6 Limpiar archivos temporales del sistema

**Objetivo:** eliminar archivos temporales que nadie usa desde hace varios días y vaciar cachés de paquetes. Solo borra archivos regulares y no cruza a otros sistemas de archivos.

Guarda como `limpiar_temporales_sistema.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail
source "$(dirname "$0")/lib_comun.sh"

DIAS="${1:-7}"
MODO="${2:-}"

requiere_root
[[ "${DIAS}" =~ ^[0-9]+$ ]] || error "DIAS debe ser un número entero."

LOG="/var/log/limpieza_temporales.log"
DIRECTORIOS=(/tmp /var/tmp)

echo "Archivos en ${DIRECTORIOS[*]} sin acceso en más de ${DIAS} días:"

for DIR in "${DIRECTORIOS[@]}"; do
  [ -d "${DIR}" ] || continue
  # -atime: último acceso; -xdev: no sale del sistema de archivos
  find "${DIR}" -xdev -type f -atime "+${DIAS}" -print
done

if [ "${MODO}" != "--aplicar" ]; then
  echo
  echo "Simulación terminada. Agrega --aplicar para eliminar."
  exit 0
fi

for DIR in "${DIRECTORIOS[@]}"; do
  [ -d "${DIR}" ] || continue
  CANT=$(find "${DIR}" -xdev -type f -atime "+${DIAS}" | wc -l)
  find "${DIR}" -xdev -type f -atime "+${DIAS}" -delete
  # Elimina carpetas vacías, sin tocar el directorio raíz
  find "${DIR}" -xdev -mindepth 1 -type d -empty -delete
  log "${DIR}: ${CANT} archivos eliminados"
done

if command -v apt-get >/dev/null; then
  log "Limpiando caché de paquetes (apt)"
  apt-get clean
  apt-get -y autoremove
fi

log "Limpieza de temporales finalizada"
df -h /tmp /var/tmp 2>/dev/null
```

Uso:

```bash
sudo ./limpiar_temporales_sistema.sh 7            # simulación
sudo ./limpiar_temporales_sistema.sh 7 --aplicar  # ejecución
```

Los sockets, enlaces y archivos especiales que usan los programas en ejecución no se eliminan, ya que `-type f` selecciona solo archivos regulares. Aun así, un archivo temporal puede estar abierto por un proceso; por eso se limita por tiempo de acceso y no se borra todo `/tmp`.

---

### 15.7 Actualizar el sistema

**Objetivo:** actualizar los paquetes del sistema dejando un registro, detectar el gestor de paquetes disponible y avisar si hace falta reiniciar.

Guarda como `actualizar_sistema.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail
source "$(dirname "$0")/lib_comun.sh"

requiere_root

LOG="/var/log/actualizacion_sistema.log"
log "===== Inicio de actualización ====="

if command -v apt-get >/dev/null; then
  log "Gestor detectado: apt"
  export DEBIAN_FRONTEND=noninteractive

  apt-get update 2>&1 | tee -a "${LOG}"

  PENDIENTES=$(apt list --upgradable 2>/dev/null | tail -n +2 | wc -l)
  log "Paquetes con actualización disponible: ${PENDIENTES}"

  if [ "${PENDIENTES}" -gt 0 ]; then
    apt-get -y upgrade 2>&1 | tee -a "${LOG}"
    apt-get -y autoremove 2>&1 | tee -a "${LOG}"
  fi

elif command -v dnf >/dev/null; then
  log "Gestor detectado: dnf"
  dnf -y upgrade 2>&1 | tee -a "${LOG}"
  dnf -y autoremove 2>&1 | tee -a "${LOG}"

elif command -v pacman >/dev/null; then
  log "Gestor detectado: pacman"
  pacman -Syu --noconfirm 2>&1 | tee -a "${LOG}"

else
  error "no se encontró un gestor de paquetes compatible."
fi

# Debian/Ubuntu crean este archivo cuando una actualización requiere reinicio
if [ -f /var/run/reboot-required ]; then
  log "ATENCIÓN: el sistema necesita reiniciarse para completar la actualización."
fi

log "===== Fin de actualización ====="
```

Uso: `sudo ./actualizar_sistema.sh`. Antes de automatizarlo en un servidor importante, se recomienda ejecutar `apt list --upgradable` y revisar qué cambiará, además de contar con un backup. Para ver el registro: `less /var/log/actualizacion_sistema.log`.

---

### 15.8 Crear usuarios del grupo "tecnicos" con carpetas individuales y compartida

**Objetivo:** dado un archivo con nombres de usuario, crear el grupo `tecnicos`, cada cuenta, una carpeta personal privada y una carpeta común accesible solo por el grupo. Todo queda dentro de `/srv/tecnicos`.

Estructura resultante:

```
/srv/tecnicos/                  (root:tecnicos, 750)
├── compartida/                 (root:tecnicos, 2770: el grupo escribe)
├── ana/                        (ana:tecnicos, 700: solo ana)
├── luis/                       (luis:tecnicos, 700: solo luis)
└── marta/                      (marta:tecnicos, 700: solo marta)
```

El bit `setgid` (el `2` de `2770`) hace que los archivos creados en `compartida` pertenezcan al grupo `tecnicos`, de modo que todos los miembros puedan trabajar sobre ellos.

Crea `tecnicos.txt` con un usuario por línea (en minúsculas, sin espacios; admite comentarios con `#`):

```
# Técnicos del área
ana
luis
marta
```

Guarda como `crear_tecnicos.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail
source "$(dirname "$0")/lib_comun.sh"

LISTA="${1:-}"
GRUPO="tecnicos"
BASE="/srv/tecnicos"
COMPARTIDA="${BASE}/compartida"
LOG="/var/log/crear_tecnicos.log"

requiere_root
[ -n "${LISTA}" ] && [ -r "${LISTA}" ] || error "Uso: $0 ARCHIVO_DE_USUARIOS"

# 1) Grupo
if getent group "${GRUPO}" >/dev/null; then
  log "El grupo ${GRUPO} ya existe."
else
  groupadd "${GRUPO}"
  log "Grupo ${GRUPO} creado."
fi

# 2) Carpeta base y carpeta compartida
mkdir -p "${COMPARTIDA}"
chown root:"${GRUPO}" "${BASE}" "${COMPARTIDA}"
chmod 750 "${BASE}"
chmod 2770 "${COMPARTIDA}"
log "Carpetas ${BASE} y ${COMPARTIDA} listas."

# 3) Usuarios
while IFS= read -r USUARIO || [ -n "${USUARIO}" ]; do
  # Ignora vacías y comentarios
  [ -z "${USUARIO}" ] && continue
  [[ "${USUARIO}" == \#* ]] && continue

  # Valida el nombre: minúsculas, números, guion y guion bajo
  if ! [[ "${USUARIO}" =~ ^[a-z][a-z0-9_-]{1,30}$ ]]; then
    log "OMITIDO: nombre de usuario inválido '${USUARIO}'"
    continue
  fi

  CARPETA="${BASE}/${USUARIO}"

  if id "${USUARIO}" >/dev/null 2>&1; then
    log "El usuario ${USUARIO} ya existe; se agrega al grupo ${GRUPO}."
    usermod -aG "${GRUPO}" "${USUARIO}"
  else
    # -d: su carpeta personal es la de /srv/tecnicos; -m: la crea con los archivos base
    useradd -m -d "${CARPETA}" -g "${GRUPO}" -s /bin/bash "${USUARIO}"

    # Contraseña temporal aleatoria que debe cambiarse en el primer ingreso
    CLAVE=$(openssl rand -base64 9)
    echo "${USUARIO}:${CLAVE}" | chpasswd
    chage -d 0 "${USUARIO}"

    log "Usuario ${USUARIO} creado."
    echo "  Contraseña temporal de ${USUARIO}: ${CLAVE}"
  fi

  mkdir -p "${CARPETA}"
  chown "${USUARIO}":"${GRUPO}" "${CARPETA}"
  chmod 700 "${CARPETA}"
done < "${LISTA}"

log "Proceso finalizado. Resumen:"
ls -ld "${BASE}" "${COMPARTIDA}" "${BASE}"/*/
echo
echo "Miembros del grupo ${GRUPO}: $(getent group "${GRUPO}" | cut -d: -f4)"
```

Uso:

```bash
sudo ./crear_tecnicos.sh tecnicos.txt
```

Para comprobar el resultado:

```bash
getent group tecnicos          # miembros del grupo
ls -la /srv/tecnicos           # carpetas y permisos
sudo -u ana touch /srv/tecnicos/compartida/prueba.txt
sudo -u luis ls /srv/tecnicos/ana   # debe fallar: carpeta privada
```

Observaciones:

- Las contraseñas temporales se muestran una sola vez en pantalla. En un entorno real conviene entregarlas por un canal seguro y no guardarlas en logs.
- `chage -d 0` obliga a cambiar la contraseña en el primer inicio de sesión.
- Para eliminar una cuenta de prueba: `sudo userdel -r nombre`. Revisa antes qué carpeta se eliminará.
- Si el usuario ya existía, el script solo lo agrega al grupo y no modifica su contraseña ni su carpeta personal actual.

---

### 15.9 Revisar el tráfico de red

**Objetivo:** observar el estado de la red: interfaces, tráfico en vivo por segundo, conexiones activas y puertos en escucha.

**Consultas rápidas**

```bash
ip -br addr                 # interfaces y direcciones IP (formato breve)
ip -s link show eth0        # paquetes y bytes recibidos/enviados, errores
ss -tuln                    # puertos TCP/UDP en escucha
ss -tun state established   # conexiones establecidas
ip route                    # tabla de rutas
```

**Script de monitoreo.** Calcula la velocidad de cada interfaz leyendo los contadores de `/proc/net/dev` en dos momentos distintos.

Guarda como `trafico_red.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail

INTERVALO="${1:-2}"
MUESTRAS="${2:-5}"

[[ "${INTERVALO}" =~ ^[1-9][0-9]*$ && "${MUESTRAS}" =~ ^[1-9][0-9]*$ ]] || {
  echo "Uso: $0 [SEGUNDOS_ENTRE_MUESTRAS] [CANTIDAD_DE_MUESTRAS]" >&2; exit 1; }

# Imprime "interfaz bytes_recibidos bytes_enviados" por línea
leer_contadores() {
  awk -F'[: ]+' 'NR > 2 { print $2, $3, $11 }' /proc/net/dev
}

formato() {
  numfmt --to=iec --suffix=B/s "$1"
}

echo "=== Interfaces ==="
ip -br addr
echo
echo "=== Tráfico por interfaz (cada ${INTERVALO}s, ${MUESTRAS} muestras) ==="

declare -A RX_PREVIO TX_PREVIO

while read -r IFACE RX TX; do
  RX_PREVIO["${IFACE}"]="${RX}"
  TX_PREVIO["${IFACE}"]="${TX}"
done < <(leer_contadores)

for ((n = 1; n <= MUESTRAS; n++)); do
  sleep "${INTERVALO}"
  echo "--- $(date '+%H:%M:%S') ---"
  while read -r IFACE RX TX; do
    DRX=$(( (RX - RX_PREVIO["${IFACE}"]) / INTERVALO ))
    DTX=$(( (TX - TX_PREVIO["${IFACE}"]) / INTERVALO ))
    printf '%-12s bajada: %-12s subida: %s\n' \
      "${IFACE}" "$(formato "${DRX}")" "$(formato "${DTX}")"
    RX_PREVIO["${IFACE}"]="${RX}"
    TX_PREVIO["${IFACE}"]="${TX}"
  done < <(leer_contadores)
done

echo
echo "=== Puertos en escucha ==="
ss -tuln

echo
echo "=== Conexiones establecidas: direcciones remotas más frecuentes ==="
ss -tn state established 2>/dev/null \
  | awk 'NR > 1 { split($4, a, ":"); print a[1] }' \
  | sort | uniq -c | sort -rn | head -10
```

Uso:

```bash
./trafico_red.sh            # 5 muestras cada 2 segundos
./trafico_red.sh 1 10       # 10 muestras cada segundo
```

Para ver qué proceso usa cada puerto se necesitan privilegios: `sudo ss -tulnp`. El script usa `/proc/net/dev`, que existe en Linux; en macOS se utilizaría `netstat -ib`.

---

### 15.10 Obtener información del sistema

**Objetivo:** reunir en un informe el estado general del equipo: sistema operativo, CPU, memoria, discos, procesos que más consumen, usuarios conectados y estado de servicios. Es una versión ampliada del informe de la sección 8.

Guarda como `info_sistema.sh`:

```bash
#!/usr/bin/env bash
set -uo pipefail

SALIDA="${1:-}"

seccion() {
  echo
  echo "=============== $* ==============="
}

informe() {
  echo "INFORME DEL SISTEMA - $(hostname) - $(date '+%Y-%m-%d %H:%M:%S')"

  seccion "SISTEMA OPERATIVO"
  if [ -r /etc/os-release ]; then
    ( . /etc/os-release && echo "Distribución: ${PRETTY_NAME}" )
  fi
  echo "Kernel:       $(uname -srm)"
  echo "Arquitectura: $(uname -m)"
  echo "Encendido:    $(uptime -p 2>/dev/null || uptime)"
  echo "Carga (1/5/15 min): $(cut -d' ' -f1-3 /proc/loadavg)"

  seccion "CPU"
  echo "Modelo:   $(grep -m1 'model name' /proc/cpuinfo | cut -d: -f2 | sed 's/^ //')"
  echo "Núcleos:  $(nproc)"

  seccion "MEMORIA"
  free -h

  seccion "DISCOS"
  df -hT -x tmpfs -x devtmpfs -x squashfs 2>/dev/null

  seccion "10 CARPETAS MÁS GRANDES EN /var (puede tardar)"
  du -xh --max-depth=1 /var 2>/dev/null | sort -rh | head -10

  seccion "PROCESOS QUE MÁS CPU USAN"
  ps -eo pid,user,%cpu,%mem,comm --sort=-%cpu | head -6

  seccion "PROCESOS QUE MÁS MEMORIA USAN"
  ps -eo pid,user,%cpu,%mem,comm --sort=-%mem | head -6

  seccion "USUARIOS CONECTADOS"
  who

  seccion "ÚLTIMOS INGRESOS"
  last -n 5 2>/dev/null

  seccion "RED"
  ip -br addr
  echo
  echo "Puerta de enlace: $(ip route | awk '/default/ {print $3; exit}')"

  seccion "SERVICIOS CON FALLAS"
  if command -v systemctl >/dev/null; then
    systemctl --failed --no-pager --no-legend | sed 's/^/  /'
    [ -z "$(systemctl --failed --no-legend)" ] && echo "  Ninguno"
  else
    echo "  systemctl no disponible"
  fi
}

if [ -n "${SALIDA}" ]; then
  informe > "${SALIDA}"
  echo "Informe guardado en ${SALIDA}"
else
  informe
fi
```

Uso:

```bash
./info_sistema.sh                       # por pantalla
./info_sistema.sh informe_$(hostname).txt   # a un archivo
```

Las funciones `seccion` e `informe` agrupan el código y permiten redirigir toda la salida de una sola vez. Se usa `set -uo pipefail` sin `-e` para que el informe continúe aunque alguno de los comandos no esté disponible.

---

### Integración de los scripts

Una forma práctica de organizar estos scripts:

```
~/admin-scripts/
├── lib_comun.sh
├── migrar_carpeta.sh
├── inventario_carpetas.sh
├── backup_rotativo.sh
├── restaurar_backup.sh
├── limpiar_logs_sistema.sh
├── limpiar_temporales_sistema.sh
├── actualizar_sistema.sh
├── crear_tecnicos.sh
├── trafico_red.sh
└── info_sistema.sh
```

Todos se marcan como ejecutables con `chmod +x *.sh`. Para programar tareas periódicas con `cron` (`sudo crontab -e`):

```
# Backup diario a las 02:00
0 2 * * *  /root/admin-scripts/backup_rotativo.sh /srv/tecnicos /var/backups 7

# Limpieza de temporales y logs, domingos a las 03:00
0 3 * * 0  /root/admin-scripts/limpiar_temporales_sistema.sh 7 --aplicar
30 3 * * 0 /root/admin-scripts/limpiar_logs_sistema.sh 7 60 --aplicar

# Informe del sistema, lunes a las 07:00
0 7 * * 1  /root/admin-scripts/info_sistema.sh /var/log/info_sistema_semanal.txt
```

Antes de programar una tarea con `--aplicar`, ejecútala varias veces en modo de simulación y verifica que los archivos seleccionados son realmente los que se desea eliminar.
