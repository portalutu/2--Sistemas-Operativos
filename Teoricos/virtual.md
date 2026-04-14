# Virtualización, contenedores y orquestación

# 1. ¿Qué es la virtualización?

La **virtualización** es una técnica que permite crear versiones virtuales de recursos informáticos, por ejemplo:

* servidores
* sistemas operativos
* redes
* almacenamiento

Gracias a la virtualización, una sola computadora física puede ejecutar varios entornos independientes al mismo tiempo.

Cada entorno virtual puede tener:

* su propio sistema operativo
* sus propias aplicaciones
* su propia configuración

**La virtualización es la base de las tecnologías de "nube"**

---

# 2. ¿Qué es un hipervisor?

Un **hipervisor** es el software encargado de crear, ejecutar y administrar máquinas virtuales.

Su función es:

* abstraer el hardware físico
* repartir recursos como CPU, RAM, disco y red
* aislar unas máquinas virtuales de otras

Las máquinas virtuales creadas por un hipervisor se llaman frecuentemente:

* VM
* guest
* invitadas

El sistema físico sobre el que corren se llama:

* host
* anfitrión

---

# 3. Tipos de hipervisores

Existen dos grandes tipos de hipervisores.

## 3.1 Hipervisor tipo 1

El **hipervisor tipo 1** se ejecuta directamente sobre el hardware físico.

También se lo conoce como:

* bare metal

Características:

* mayor rendimiento
* mejor aislamiento
* uso frecuente en servidores y centros de datos

Ejemplos:

* VMware ESXi
* Microsoft Hyper-V Server
* Proxmox VE
* Xen

---

## 3.2 Hipervisor tipo 2

El **hipervisor tipo 2** se instala sobre un sistema operativo existente.

Características:

* fácil de instalar
* ideal para escritorio, laboratorios y capacitación
* rendimiento generalmente menor que un tipo 1

Ejemplos:

* VirtualBox
* VMware Workstation Pro
* VMware Fusion

---

# 4. Ejemplos de hipervisores del mercado

## 4.1 VirtualBox

Sitio oficial:

* https://www.virtualbox.org/

Descripción breve:

* hipervisor de tipo 2 muy utilizado en educación, laboratorios y pruebas
* permite ejecutar máquinas virtuales en Windows, Linux y otros sistemas anfitriones

Licenciamiento:

* el paquete base se distribuye como software libre bajo **GPLv3**
* el **Extension Pack** se distribuye bajo **PUEL** para uso personal y educativo

Uso típico:

* laboratorios académicos
* aprendizaje de Linux y redes

---

## 4.2 VMware Workstation Pro

Sitio oficial:

* https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion

Preguntas frecuentes oficiales:

* https://www.vmware.com/docs/desktop-hypervisor-faqs

Descripción breve:

* hipervisor de tipo 2 orientado a escritorio
* muy utilizado en entornos profesionales y de laboratorio

Licenciamiento:

* producto propietario
* según la documentación oficial vigente, **Workstation Pro** y **Fusion Pro** se ofrecen sin cargo de licencia para uso personal y comercial, bajo los términos del fabricante

Uso típico:

* laboratorios avanzados
* pruebas de compatibilidad
* entornos de desarrollo

---

## 4.3 Proxmox VE

Sitio oficial:

* https://www.proxmox.com/en/proxmox-virtual-environment/overview

Documentación introductoria:

* https://pve.proxmox.com/wiki/Introduction

Descripción breve:

* plataforma de virtualización de tipo 1 orientada a servidores
* permite administrar máquinas virtuales y contenedores desde una interfaz web
* combina virtualización con herramientas de almacenamiento, red y clustering

Licenciamiento:

* software **open source** bajo **AGPLv3**
* la empresa ofrece suscripciones comerciales de soporte y acceso al repositorio empresarial

Uso típico:

* servidores institucionales
* laboratorios de infraestructura
* clústeres de virtualización

---

## 4.4 VMware ESXi

Sitio oficial:

* https://www.vmware.com/products/cloud-infrastructure/vsphere/esxi-and-esx

Descripción breve:

* hipervisor de tipo 1 orientado a centros de datos y virtualización empresarial
* forma parte del ecosistema VMware vSphere

Licenciamiento:

* software propietario
* su uso empresarial se asocia normalmente a licenciamiento y soporte comercial

Uso típico:

* virtualización de servidores
* infraestructura empresarial

---

# 5. Máquinas virtuales y contenedores

Aunque ambos conceptos permiten aislar aplicaciones o sistemas, no funcionan igual.

## 5.1 Máquina virtual

Una máquina virtual:

* virtualiza hardware
* ejecuta un sistema operativo completo
* tiene su propio kernel

Ventajas:

* fuerte aislamiento
* permite ejecutar sistemas operativos distintos sobre el mismo host

Desventajas:

* consume más recursos
* arranque más lento

---

## 5.2 Contenedor

Un contenedor:

* virtualiza a nivel de sistema operativo
* comparte el kernel del host
* empaqueta una aplicación con sus dependencias

Ventajas:

* menor consumo de recursos
* inicio rápido
* portabilidad

Desventajas:

* aislamiento generalmente menor que una VM
* depende del kernel del host

---

# 6. ¿Qué son los contenedores?

Los **contenedores** son unidades de ejecución livianas que encapsulan:

* una aplicación
* bibliotecas
* configuraciones
* dependencias necesarias para ejecutarla

Esto permite que una aplicación funcione de forma similar en distintos entornos.

Los contenedores son muy usados en:

* desarrollo
* pruebas
* despliegue de aplicaciones
* microservicios
* automatización

---

# 7. Tipos de contenedores

De manera simplificada, podemos distinguir dos enfoques frecuentes.

## 7.1 Contenedores de aplicación

Se enfocan en ejecutar una aplicación o servicio específico.

Ejemplos:

* Docker
* Podman
* containerd

Son comunes en:

* desarrollo de software
* despliegues cloud-native
* microservicios

---

## 7.2 Contenedores de sistema

Buscan comportarse de forma más parecida a una pequeña instancia de sistema operativo.

Ejemplos:

* LXC
* Incus

Son comunes en:

* laboratorios
* aislamiento liviano de servicios
* escenarios donde se desea un entorno parecido a una mini VM

---

# 8. Ejemplos de tecnologías de contenedores del mercado

## 8.1 Docker

Sitio oficial:

* https://www.docker.com/

Descripción breve:

* plataforma muy popular para crear, distribuir y ejecutar contenedores de aplicación
* incluye herramientas como Docker Engine, Docker Desktop, Docker Hub y Compose

Licenciamiento:

* Docker Engine y varios componentes son open source
* Docker Desktop se distribuye bajo un esquema de suscripción
* existe un plan **Personal** gratuito para uso individual, educación, open source no comercial y pequeñas organizaciones dentro de ciertos límites
* organizaciones mayores o ciertos usos comerciales requieren suscripción paga

Uso típico:

* desarrollo local
* empaquetado de aplicaciones
* laboratorios de contenedores

---

## 8.2 Podman

Sitio oficial:

* https://podman.io/

Descripción breve:

* herramienta open source para ejecutar contenedores y pods
* compatible con muchos flujos de trabajo similares a Docker
* muy utilizada en entornos Linux y ecosistemas Red Hat

Licenciamiento:

* software open source bajo **Apache License 2.0**

Uso típico:

* administración de contenedores en Linux
* laboratorios
* entornos donde se busca una alternativa abierta a Docker Desktop

---

## 8.3 containerd

Sitio oficial:

* https://containerd.io/

Repositorio oficial:

* https://github.com/containerd/containerd

Descripción breve:

* runtime de contenedores orientado a simplicidad, robustez y portabilidad
* es una pieza de bajo nivel utilizada por distintas plataformas del ecosistema cloud-native

Licenciamiento:

* software open source bajo **Apache License 2.0**

Uso típico:

* ejecución de contenedores
* integración con plataformas como Kubernetes

---

## 8.4 LXC

Sitio oficial:

* https://linuxcontainers.org/lxc/

Descripción breve:

* tecnología de contenedores de sistema en Linux
* permite ejecutar entornos aislados parecidos a pequeños sistemas independientes

Licenciamiento:

* la licencia principal del proyecto es **LGPLv2.1+**
* algunos componentes adicionales usan otras licencias libres, como GPLv2 o BSD

Uso típico:

* contenedores de sistema
* laboratorios Linux
* aislamiento liviano

---

# 9. ¿Qué es un orquestador?

Un **orquestador** es una plataforma que administra múltiples contenedores de forma automática.

Permite tareas como:

* desplegar aplicaciones
* escalar servicios
* reiniciar contenedores fallidos
* distribuir carga
* administrar redes y almacenamiento

Cuando una organización tiene muchos contenedores, hacerlo manualmente se vuelve difícil. Por eso se utilizan orquestadores.

---

# 10. Ejemplos de orquestadores

## 10.1 Kubernetes

Sitio oficial:

* https://kubernetes.io/

Descripción breve:

* orquestador de contenedores muy extendido en la industria
* automatiza despliegue, escalado y administración de aplicaciones en contenedores
* es una tecnología central del ecosistema cloud-native

Licenciamiento:

* software open source bajo **Apache License 2.0**

Uso típico:

* plataformas empresariales
* clústeres on-premise
* nubes públicas y privadas

---

## 10.2 Docker Compose

Sitio oficial:

* https://docs.docker.com/compose/

Descripción breve:

* herramienta para definir y ejecutar aplicaciones multicontenedor mediante archivos YAML
* no es un orquestador completo al nivel de Kubernetes, pero resulta muy útil para laboratorios y desarrollo local

Licenciamiento:

* forma parte del ecosistema Docker
* su uso práctico depende del esquema de licenciamiento de los componentes Docker utilizados en cada entorno

Uso típico:

* laboratorios
* desarrollo local
* pruebas de aplicaciones con varios servicios

---

## 10.3 Nomad

Sitio oficial:

* https://www.nomadproject.io/

Descripción breve:

* orquestador ligero desarrollado por HashiCorp
* permite ejecutar contenedores y otras cargas de trabajo

Licenciamiento:

* el proyecto tiene ediciones y componentes con distintos esquemas según el producto y la versión
* conviene revisar siempre la documentación oficial vigente antes de adoptarlo institucionalmente

Uso típico:

* orquestación liviana
* entornos que buscan una alternativa más simple a Kubernetes

---

# 11. Relación entre estas tecnologías

Podemos resumirlo así:

* **hipervisor**: crea y administra máquinas virtuales
* **máquina virtual**: sistema completo con su propio sistema operativo
* **contenedor**: entorno aislado que comparte el kernel del host
* **orquestador**: administra muchos contenedores de forma automática

Ejemplo práctico:

* VirtualBox puede ejecutar una VM con Ubuntu Server
* dentro de esa VM se puede instalar Docker
* luego se pueden ejecutar contenedores
* si el entorno crece, Kubernetes puede administrarlos

---

# 12. Comparación general

## 12.1 Hipervisores

Ventajas:

* gran aislamiento
* permiten ejecutar sistemas operativos diferentes
* útiles para laboratorios y servidores virtualizados

Desventajas:

* mayor consumo de recursos
* más sobrecarga que los contenedores

---

## 12.2 Contenedores

Ventajas:

* rapidez
* menor consumo de recursos
* facilidad para distribuir aplicaciones

Desventajas:

* dependen del kernel del host
* pueden requerir herramientas adicionales para administración a gran escala

---

## 12.3 Orquestadores

Ventajas:

* automatización
* escalabilidad
* alta disponibilidad

Desventajas:

* mayor complejidad
* curva de aprendizaje más alta

---

# 13. Conclusión

En la práctica actual conviven varias tecnologías:

* hipervisores para virtualizar sistemas completos
* contenedores para empaquetar aplicaciones
* orquestadores para administrar grandes conjuntos de contenedores

Comprender estas diferencias permite elegir la herramienta adecuada según el objetivo:

* laboratorio personal
* servidor institucional
* despliegue de aplicaciones
* infraestructura empresarial
