# Netplan en Ubuntu

# 1. ¿Qué es Netplan?

**Netplan** es una herramienta de configuración de red utilizada por distribuciones modernas como **Ubuntu Server**.

Su función es permitir que la configuración de red se defina en archivos **YAML** dentro del directorio:

```bash
/etc/netplan/
```

Netplan no configura la red directamente, sino que genera la configuración para uno de sus backends:

* `systemd-networkd`
* `NetworkManager`

En servidores, lo más habitual es utilizar:

* `systemd-networkd`

---

# 2. ¿Para qué se utiliza?

Con Netplan se pueden configurar:

* direcciones IP estáticas
* direcciones IP por DHCP
* puerta de enlace
* servidores DNS
* interfaces Ethernet
* redes Wi-Fi
* bonds
* bridges
* VLANs

En un entorno de servidor, lo más común es configurar interfaces Ethernet con:

* DHCP
* IP estática

---

# 3. Ubicación de los archivos

Los archivos de Netplan se guardan en:

```bash
/etc/netplan/
```

Ejemplos frecuentes:

* `/etc/netplan/00-installer-config.yaml`
* `/etc/netplan/01-netcfg.yaml`

El nombre del archivo puede variar, pero siempre debe tener extensión:

```bash
.yaml
```

---

# 4. Estructura básica de un archivo Netplan

Un archivo sencillo tiene una estructura similar a esta:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: true
```

## Explicación

* `network:` indica el inicio de la configuración de red
* `version: 2` define la versión del formato de Netplan
* `renderer: networkd` indica que se utilizará `systemd-networkd`
* `ethernets:` agrupa las interfaces de red cableadas
* `enp0s3:` es el nombre de la interfaz
* `dhcp4: true` habilita DHCP para IPv4

---

# 5. Identificar la interfaz de red

Antes de configurar Netplan es importante identificar el nombre real de la interfaz.

Comandos útiles:

```bash
ip a
```

o:

```bash
ip link show
```

Ejemplos de nombres de interfaz:

* `enp0s3`
* `ens33`
* `eth0`

El nombre depende del hardware o de la máquina virtual.

---

# 6. Configuración por DHCP

Cuando el servidor obtiene su dirección IP automáticamente desde un router o un servidor DHCP, se puede usar una configuración como esta:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: true
```

Esto permite que la interfaz reciba automáticamente:

* dirección IP
* máscara de red
* gateway
* DNS

---

# 7. Configuración con IP estática

Si el servidor debe tener siempre la misma dirección IP, se utiliza una configuración estática.

Ejemplo:

```yaml
network:
  version: 2
  renderer: networkd
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
          - 8.8.8.8
          - 1.1.1.1
```

## Explicación

* `dhcp4: false` desactiva DHCP para IPv4
* `addresses:` define una o más direcciones IP con su prefijo
* `192.168.1.50/24` indica IP y máscara en formato CIDR
* `routes:` define rutas
* `to: default` representa la ruta por defecto
* `via: 192.168.1.1` indica la puerta de enlace
* `nameservers:` permite indicar DNS

---

# 8. Principales parámetros de Netplan

## 8.1 `renderer`

Define el backend que aplicará la configuración.

Valores comunes:

* `networkd`
* `NetworkManager`

En servidores se recomienda:

* `networkd`

---

## 8.2 `ethernets`

Se utiliza para agrupar interfaces cableadas.

Ejemplo:

```yaml
ethernets:
  enp0s3:
```

---

## 8.3 `dhcp4`

Activa o desactiva DHCP para IPv4.

Valores:

* `true`
* `false`

Ejemplo:

```yaml
dhcp4: true
```

---

## 8.4 `dhcp6`

Activa o desactiva DHCP para IPv6.

Ejemplo:

```yaml
dhcp6: false
```

---

## 8.5 `addresses`

Permite indicar una o varias direcciones IP manuales.

Ejemplo:

```yaml
addresses:
  - 192.168.1.50/24
```

---

## 8.6 `routes`

Se usa para definir rutas manuales.

Ejemplo de ruta por defecto:

```yaml
routes:
  - to: default
    via: 192.168.1.1
```

---

## 8.7 `gateway4`

En versiones anteriores se utilizaba mucho:

```yaml
gateway4: 192.168.1.1
```

Actualmente se recomienda usar:

* `routes`

porque es un método más moderno y flexible.

---

## 8.8 `nameservers`

Permite configurar los servidores DNS.

Ejemplo:

```yaml
nameservers:
  addresses:
    - 8.8.8.8
    - 1.1.1.1
```

También se puede agregar:

```yaml
search:
  - aula.local
```

para definir dominios de búsqueda.

---

## 8.9 `optional`

Indica si la interfaz es opcional durante el arranque.

Ejemplo:

```yaml
optional: true
```

Esto puede ser útil cuando una interfaz no siempre estará conectada y no se quiere retrasar el inicio del sistema.

---

# 9. Pasos para aplicar una configuración

## 9.1 Editar el archivo

Se puede usar, por ejemplo:

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

---

## 9.2 Verificar la sintaxis

Antes de aplicar cambios, conviene validar la configuración:

```bash
sudo netplan generate
```

Si no aparecen errores, la sintaxis es correcta.

---

## 9.3 Aplicar los cambios

```bash
sudo netplan apply
```

---

## 9.4 Probar de forma segura

Cuando se trabaja por acceso remoto, es recomendable usar:

```bash
sudo netplan try
```

Este comando aplica la configuración temporalmente y permite revertirla si hay problemas.

---

# 10. Verificación posterior

Después de aplicar la configuración, se puede comprobar el resultado con:

```bash
ip a
```

Para revisar la puerta de enlace:

```bash
ip route
```

Para comprobar conectividad:

```bash
ping 8.8.8.8
```

Para comprobar resolución DNS:

```bash
ping google.com
```

---

# 11. Errores comunes

## 11.1 Mala indentación

Netplan utiliza YAML, por lo tanto la indentación es fundamental.

Si los espacios son incorrectos, la configuración fallará.

Recomendaciones:

* usar espacios y no tabulaciones
* mantener una indentación consistente
* revisar cuidadosamente cada nivel

---

## 11.2 Nombre de interfaz incorrecto

Si el nombre no coincide con el de la interfaz real, la configuración no se aplicará.

Verificar siempre con:

```bash
ip a
```

---

## 11.3 Dirección o gateway incorrectos

Una IP fuera de rango o una puerta de enlace equivocada puede dejar al servidor sin conectividad.

---

## 11.4 DNS mal configurado

Si el servidor tiene conectividad IP pero no resuelve nombres, normalmente el problema está en:

* `nameservers`

---

# 12. Buenas prácticas

* realizar una copia del archivo antes de modificarlo
* usar `netplan try` cuando se trabaja por SSH
* verificar el nombre real de la interfaz antes de editar
* documentar las IP estáticas utilizadas
* evitar cambios apresurados en servidores en producción

---

# 13. Ejemplo final para Ubuntu Server

La siguiente configuración de ejemplo define una interfaz Ethernet con IP estática en una instalación típica de **Ubuntu Server**.

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
        search:
          - laboratorio.local
      optional: true
```

## Qué hace esta configuración

* asigna la IP `192.168.1.100`
* utiliza máscara `/24`
* configura como gateway `192.168.1.1`
* define los DNS `8.8.8.8` y `1.1.1.1`
* agrega el dominio de búsqueda `laboratorio.local`

Los estudiantes deben adaptar:

* el nombre de la interfaz
* la dirección IP
* el gateway
* los DNS

según la red disponible en su laboratorio o máquina virtual.
