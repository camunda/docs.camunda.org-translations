---

title: 'Instalación de la distribución pre-built'
category: 'Plataforma BPM'

---


1. Descargar la distribución empaquetada del siguiente enlace http://camunda.org/release/camunda-bpm/jboss/.
2. Desempaquetar la distribución en un directorio.
3. Configurar el datasource según las necesidades (ver abajo).
4. Arrancar el servidor ejecutando `camunda-welcome.bat` o mediante el script `$JBOSS_HOME/bin/standalone.{bat/sh}`.


## Accediendo a la consola H2

Con el servidor de aplicaciones JBoss podemos acceder de una manera fácil a la consola H2 para inspeccionar la base de datos local H2 (utilizada en escenarios de demostración/evaluación):

1.  Acceder a http://localhost:8080/h2/h2
2.  Hacer login con los siguientes datos:
    *   jdbc:h2:./camunda-h2-dbs/process-engine
    *   Usuario: sa
    *   Password: sa