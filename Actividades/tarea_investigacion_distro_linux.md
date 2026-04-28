# Tarea grupal: investigación, instalación y presentación de una distribución GNU/Linux

## 1. Descripción general

En esta tarea trabajaremos con distribuciones GNU/Linux reales. Cada equipo deberá seleccionar una distribución desde la lista publicada en **DistroWatch**, investigar sus características principales, descargarla, instalarla en una máquina virtual o equipo de prueba, utilizarla durante un período breve y elaborar un informe técnico con capturas propias.

La actividad no consiste solamente en buscar información en internet. El objetivo principal es que el equipo pueda **probar la distribución**, observar su funcionamiento, comparar lo investigado con la experiencia real de instalación y uso, y presentar conclusiones fundamentadas.

---

## 2. Objetivos de aprendizaje

Al finalizar la tarea, cada equipo deberá ser capaz de:

1. Identificar las características principales de una distribución GNU/Linux.
2. Explicar el origen, propósito y público objetivo de la distribución elegida.
3. Descargar una imagen ISO desde una fuente confiable.
4. Instalar la distribución en una máquina virtual o equipo de prueba.
5. Documentar el proceso de instalación mediante capturas propias.
6. Analizar fortalezas, debilidades, requisitos técnicos y casos de uso.
7. Comparar la experiencia real de uso con la información obtenida durante la investigación.
8. Presentar conclusiones técnicas de forma clara y ordenada.
9. Usar herramientas de inteligencia artificial de forma ética, como apoyo y no como sustituto del trabajo propio.

---

## 3. Modalidad de trabajo

- **Trabajo:** grupal.
- **Producto final:** informe escrito y presentación oral.
- **Cantidad sugerida de integrantes:** 3 a 5 estudiantes por equipo.
- **Tema:** una distribución GNU/Linux tomada de la lista de DistroWatch.
- **Condición obligatoria:** cada equipo debe descargar, instalar y probar la distribución elegida.
- **Evidencia obligatoria:** el informe debe incluir capturas propias del proceso de instalación y uso.

---

## 4. Selección de la distribución

Cada equipo deberá elegir una distribución GNU/Linux desde DistroWatch.

Pueden elegir distribuciones conocidas, livianas, especializadas, educativas, de seguridad, de escritorio, para servidores o pensadas para equipos antiguos.

Ejemplos posibles:

- Ubuntu
- Debian
- Linux Mint
- Fedora
- openSUSE
- Arch Linux
- Manjaro
- MX Linux
- Zorin OS
- Pop!\_OS
- EndeavourOS
- Kali Linux
- Tails
- Lubuntu
- Xubuntu
- Puppy Linux
- Elementary OS
- AlmaLinux
- Rocky Linux
- Garuda Linux

La distribución elegida debe ser aprobada por el docente para evitar que varios grupos trabajen sobre la misma, salvo que el docente indique lo contrario.

---

## 5. Justificación de la elección

Cada grupo deberá explicar por qué eligió esa distribución.

La justificación puede considerar aspectos como:

- Interés del grupo.
- Popularidad de la distribución.
- Facilidad o dificultad de instalación.
- Requisitos de hardware.
- Uso en computadoras antiguas.
- Uso educativo.
- Uso profesional.
- Uso en ciberseguridad.
- Uso en servidores.
- Diseño visual.
- Comunidad y documentación disponible.
- Diferencias con otras distribuciones más conocidas.

No alcanza con escribir “la elegimos porque nos gustó”. La elección debe estar explicada con argumentos.

---

## 6. Instalación obligatoria

Cada equipo deberá descargar la imagen ISO oficial de la distribución e instalarla.

La instalación podrá realizarse en:

- Máquina virtual con VirtualBox.
- Máquina virtual con VMware.
- Live USB, solo si además se demuestra uso real del sistema y no únicamente arranque en modo live.

El equipo deberá registrar evidencias propias del proceso.

### Capturas mínimas obligatorias

El informe deberá incluir, como mínimo:

1. Página oficial o fuente de descarga de la ISO.
2. Archivo ISO descargado o medio de instalación preparado.
3. Configuración de la máquina virtual o equipo de prueba.
4. Pantalla inicial del instalador.
5. Selección de idioma, teclado o región.
6. Particionado o selección de disco.
7. Creación de usuario.
8. Finalización de la instalación.
9. Primer inicio de sesión.
10. Escritorio o entorno gráfico funcionando.
11. Terminal abierta mostrando un comando de identificación del sistema.
12. Gestor de software, gestor de paquetes o actualización del sistema.

### Comandos sugeridos para evidenciar el sistema

El equipo puede usar algunos de estos comandos y capturar la salida:

```bash
cat /etc/os-release
uname -a
lsblk
free -h
df -h
whoami
ip a
```

No es necesario usar todos, pero sí deben demostrar que el sistema fue instalado y probado por el equipo.

---

## 7. Uso permitido de inteligencia artificial

Los estudiantes podrán usar herramientas de inteligencia artificial para apoyar la investigación, pero no para hacer el trabajo completo.

### Usos permitidos

Se permite usar IA para:

- Buscar ideas iniciales.
- Explicar conceptos que el equipo no comprende.
- Comparar distribuciones.
- Sugerir criterios de análisis.
- Ayudar a revisar la redacción del informe.
- Preparar preguntas de investigación.
- Traducir o resumir documentación técnica, siempre verificando la información.

### Usos no permitidos

No se permite usar IA para:

- Generar el informe completo y entregarlo como propio.
- Inventar capturas de pantalla.
- Inventar pruebas de instalación.
- Copiar conclusiones sin haber probado la distribución.
- Responder sin verificar en fuentes oficiales.
- Presentar información que el grupo no pueda explicar oralmente.

### Declaración obligatoria de uso de IA

Al final del informe deberá incluirse una sección llamada **Uso de inteligencia artificial**, indicando:

- Qué herramienta se utilizó.
- Para qué se utilizó.
- Qué información fue verificada por el equipo.
- Qué partes del trabajo fueron realizadas directamente por los integrantes.

Ejemplo:

> Utilizamos IA para obtener una lista inicial de aspectos a comparar entre distribuciones Linux y para mejorar la redacción de algunos párrafos. La instalación, las capturas, las pruebas y las conclusiones fueron realizadas por el equipo. Verificamos la información técnica en la página oficial de la distribución y en DistroWatch.

---

## 8. Contenido obligatorio del informe

El informe deberá tener la siguiente estructura:

## Portada

Debe incluir:

- Nombre de la institución.
- Curso y grupo.
- Asignatura.
- Nombre de la tarea.
- Distribución investigada.
- Integrantes del equipo.
- Fecha de entrega.

## 1. Introducción

Breve presentación del trabajo. Debe explicar qué distribución se investigó y cuál fue el objetivo de la tarea.

## 2. Justificación de la elección

Explicar por qué el equipo eligió esa distribución y qué esperaba encontrar antes de instalarla.

## 3. Información general de la distribución

Incluir:

- Nombre de la distribución.
- País u origen del proyecto, si corresponde.
- Distribución base, si tiene una.
- Tipo de usuario al que apunta.
- Entorno de escritorio utilizado.
- Gestor de paquetes.
- Modelo de actualización.
- Sitio web oficial.
- Ubicación en DistroWatch o información relevante tomada de esa página.

## 4. Características principales

Describir las características más importantes de la distribución:

- Facilidad de uso.
- Apariencia visual.
- Herramientas incluidas.
- Instalador.
- Software preinstalado.
- Administración del sistema.
- Comunidad y documentación.
- Seguridad.
- Rendimiento.
- Personalización.

## 5. Requerimientos técnicos

Indicar los requisitos mínimos y recomendados, por ejemplo:

- Procesador.
- Memoria RAM.
- Espacio en disco.
- Tarjeta gráfica, si corresponde.
- Conectividad.
- Requisitos especiales.

También deben indicar con qué recursos fue probada por el equipo:

- RAM asignada.
- CPU asignada.
- Espacio en disco.
- Tipo de máquina virtual utilizada.

## 6. Proceso de instalación

Describir paso a paso cómo instalaron la distribución.

Debe incluir capturas propias y explicaciones breves de cada etapa:

- Descarga.
- Preparación de la máquina virtual o medio de instalación.
- Inicio del instalador.
- Configuración de idioma, teclado y zona horaria.
- Particionado.
- Creación de usuario.
- Instalación.
- Primer inicio.

## 7. Pruebas realizadas

El equipo deberá probar el sistema instalado y documentar qué hizo.

Pruebas mínimas sugeridas:

- Abrir el navegador web.
- Abrir la terminal.
- Ejecutar comandos básicos.
- Revisar el gestor de archivos.
- Instalar o actualizar un paquete.
- Revisar el consumo de RAM y disco.
- Cambiar alguna configuración del sistema.
- Probar una aplicación incluida.

## 8. Fortalezas

Indicar los puntos fuertes de la distribución. Deben estar justificados con evidencia o experiencia de uso.

Ejemplos:

- Fácil instalación.
- Bajo consumo de recursos.
- Buena documentación.
- Interfaz amigable.
- Buen soporte de hardware.
- Buen rendimiento.
- Herramientas útiles preinstaladas.

## 9. Debilidades

Indicar los puntos débiles o dificultades encontradas.

Ejemplos:

- Instalación compleja.
- Requiere muchos recursos.
- Poca documentación en español.
- Problemas con idioma o teclado.
- Gestor de paquetes difícil de usar.
- No es recomendable para principiantes.
- Comunidad pequeña.

## 10. Comparación con otra distribución

Comparar brevemente la distribución elegida con otra distribución conocida, por ejemplo Ubuntu, Debian, Linux Mint, Fedora o Windows si el docente lo permite como referencia de usuario final.

La comparación debe incluir al menos 5 criterios:

| Criterio                 | Distro elegida | Distro comparada |
| ------------------------ | -------------- | ---------------- |
| Facilidad de instalación |                |                  |
| Consumo de recursos      |                |                  |
| Facilidad de uso         |                |                  |
| Software incluido        |                |                  |
| Público objetivo         |                |                  |

## 11. Casos de uso recomendados

Explicar para qué situaciones recomendarían esta distribución.

Ejemplos:

- Computadoras antiguas.
- Uso diario de escritorio.
- Estudiantes.
- Programación.
- Seguridad informática.
- Servidores.
- Privacidad.
- Diseño o multimedia.
- Usuarios principiantes.
- Usuarios avanzados.

## 12. Conclusión crítica

La conclusión debe responder:

- ¿La distribución cumplió con lo que esperaban?
- ¿Fue fácil o difícil instalarla?
- ¿Qué aprendieron al probarla?
- ¿La recomendarían? ¿A quién sí y a quién no?
- ¿Qué cambiarían si tuvieran que repetir la experiencia?

## 13. Bibliografía y fuentes consultadas

Incluir las fuentes utilizadas:

- DistroWatch.
- Sitio oficial de la distribución.
- Documentación oficial.
- Foros o wikis consultadas.
- Videos o tutoriales utilizados.
- Herramientas de IA utilizadas, si corresponde.

## 14. Uso de inteligencia artificial

Declarar cómo se utilizó IA, según lo indicado en la sección 7.

---

## 9. Presentación oral

Cada equipo deberá realizar una presentación frente al grupo.

### Duración sugerida

Entre **8 y 12 minutos** por equipo.

### Contenido mínimo de la presentación

La presentación deberá incluir:

1. Nombre de la distribución.
2. Motivo de elección.
3. Características principales.
4. Requerimientos técnicos.
5. Proceso de instalación, con capturas propias.
6. Pruebas realizadas.
7. Fortalezas.
8. Debilidades.
9. Casos de uso recomendados.
10. Conclusión del equipo.

### Reglas de presentación

- Todos los integrantes deben participar.
- Deben usar capturas propias.
- No se permite leer todo el informe.
- Deben poder responder preguntas del docente y de sus compañeros.
- Deben explicar con sus palabras lo que hicieron.

---

## 10. Criterios de evaluación

| Criterio                             | Excelente                                                                         | Aceptable                                                     | Insuficiente                                                                        |
| ------------------------------------ | --------------------------------------------------------------------------------- | ------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Elección y justificación             | La elección está claramente fundamentada y conectada con el objetivo del trabajo. | La elección está explicada, pero con argumentos simples.      | No hay justificación clara o es demasiado superficial.                              |
| Investigación técnica                | Presenta información completa, verificada y bien organizada.                      | Presenta información básica, con algunas omisiones.           | Presenta información incompleta, confusa o no verificada.                           |
| Instalación real                     | Se demuestra claramente que la distro fue instalada y probada por el equipo.      | Hay evidencia parcial de instalación o uso.                   | No hay evidencia suficiente de instalación real.                                    |
| Capturas propias                     | Las capturas son claras, suficientes y están explicadas.                          | Incluye capturas, pero faltan algunas etapas o explicaciones. | Las capturas son escasas, ajenas o no demuestran el trabajo realizado.              |
| Análisis de fortalezas y debilidades | El análisis es crítico, concreto y basado en la experiencia de uso.               | El análisis es correcto, pero poco profundo.                  | El análisis es genérico o copiado de fuentes externas.                              |
| Requerimientos técnicos              | Identifica requisitos mínimos, recomendados y recursos usados en la prueba.       | Incluye algunos requisitos, pero faltan datos importantes.    | No identifica correctamente los requerimientos.                                     |
| Comparación                          | Compara con criterios claros y útiles.                                            | La comparación es básica.                                     | No compara o la comparación no aporta información relevante.                        |
| Conclusión crítica                   | Presenta una conclusión propia, fundamentada y reflexiva.                         | Presenta una conclusión simple.                               | La conclusión es copiada, vaga o no responde a la experiencia realizada.            |
| Presentación oral                    | Expone con claridad, participa todo el equipo y responde preguntas.               | Expone de forma comprensible, aunque con poca profundidad.    | Lee sin explicar, no participa todo el equipo o no puede responder preguntas.       |
| Uso ético de IA                      | Declara correctamente el uso de IA y demuestra trabajo propio.                    | Declara el uso de IA, pero con poca precisión.                | No declara IA, entrega contenido generado sin dominio o presenta trabajo no propio. |

---

## 11. Puntaje sugerido

| Componente                                  | Puntaje        |
| ------------------------------------------- | -------------- |
| Informe escrito                             | 40 puntos      |
| Evidencia de instalación y capturas propias | 20 puntos      |
| Análisis técnico y conclusión crítica       | 20 puntos      |
| Presentación oral                           | 15 puntos      |
| Uso ético de IA y fuentes                   | 5 puntos       |
| **Total**                                   | **100 puntos** |

---

## 12. Condiciones de entrega

El equipo deberá entregar:

1. Informe en PDF o documento digital indicado por el docente.
2. Presentación en formato digital.
3. Capturas propias incluidas en el informe.
4. Enlace o archivo con las fuentes consultadas, si el docente lo solicita.
5. Declaración de uso de IA.

---

## 13. Advertencias importantes

El trabajo podrá ser observado o rechazado si:

- No hay evidencia de instalación real.
- Las capturas parecen tomadas de internet.
- El equipo no puede explicar lo que entregó.
- El informe fue generado casi por completo con IA.
- No se citan las fuentes.
- Se entrega información falsa o inventada.
- No participa todo el equipo.

La inteligencia artificial puede ayudar a estudiar, ordenar ideas o mejorar la redacción, pero el aprendizaje se demuestra instalando, probando, analizando y explicando la distribución con trabajo propio.

---

## 14. Preguntas guía para orientar la investigación

Los equipos pueden usar estas preguntas para organizar el trabajo:

1. ¿Cuál es el objetivo principal de esta distribución?
2. ¿Está basada en otra distribución? ¿En cuál?
3. ¿A qué tipo de usuario apunta?
4. ¿Es adecuada para principiantes?
5. ¿Qué entorno gráfico utiliza?
6. ¿Qué gestor de paquetes usa?
7. ¿Qué requisitos mínimos tiene?
8. ¿Qué tan fácil fue instalarla?
9. ¿Qué problemas aparecieron durante la instalación?
10. ¿Qué aplicaciones trae preinstaladas?
11. ¿Consume muchos o pocos recursos?
12. ¿Tiene buena documentación?
13. ¿Tiene comunidad activa?
14. ¿Qué ventajas tiene frente a otras distribuciones?
15. ¿Qué desventajas presenta?
16. ¿Para qué casos la recomendarían?
17. ¿Para qué casos no la recomendarían?
18. ¿Qué aprendió el equipo durante la prueba?

---

## 15. Plantilla breve para la conclusión

El equipo puede utilizar esta guía para redactar la conclusión sin copiarla literalmente:

> Luego de investigar, instalar y probar la distribución \_\_\_\_\_\_\_\_\_\_\_\_, concluimos que es una distro orientada principalmente a \_\_\_\_\_\_\_\_\_\_\_\_. Su instalación nos resultó \_\_\_\_\_\_\_\_\_\_\_\_ porque \_\_\_\_\_\_\_\_\_\_\_\_. Entre sus principales fortalezas encontramos \_\_\_\_\_\_\_\_\_\_\_\_, mientras que sus debilidades más importantes fueron \_\_\_\_\_\_\_\_\_\_\_\_. Consideramos que sería recomendable para \_\_\_\_\_\_\_\_\_\_\_\_, pero no tanto para \_\_\_\_\_\_\_\_\_\_\_\_. La experiencia nos permitió comprender mejor \_\_\_\_\_\_\_\_\_\_\_\_ y comparar la información investigada con el uso real del sistema operativo.

