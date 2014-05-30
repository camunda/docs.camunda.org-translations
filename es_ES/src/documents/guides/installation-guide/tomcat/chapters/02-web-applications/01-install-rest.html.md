---

title: 'Instalar la aplicación Web REST API'
shortTitle: 'Instalación de REST API'
category: 'Aplicaciones Web'

---


Para instalar la API de REST, se requiere la instalación del módulo `org.camunda.bpm.camunda-engine` de Tomcat.
Mirar arriba, cómo [instalar la distribución pre-built](ref:#bpm-platform-install-the-pre-built-distro) o [Instalar la plataforma en Tomcat](ref:#bpm-platform-install-the-platform-on-a-vanilla-tomcat).

**Nota**: La distribución ya expone la API de REST en la siguiente ruta contexto (context path) `/engine-rest`.

Se requieren los siguientes pasos para desplegar la API de REST en una instancia de Tomcat:

1.  Descargar la aplicación web REST API desde [nuestro servidor](https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/camunda-engine-rest/).
   Escoger la versión adecuada con el nombre `$PLATFORM_VERSION/camunda-engine-rest-$PLATFORM_VERSION-tomcat.war`.
2.  Copia el fichero WAR en la siguiente ruta `$CATALINA_HOME/webapps`.
   Opcionalmente, se debería de cambiar la ruta contexto sobre la cual la API de REST se desplega como `/engine-rest`.
3.  Arrancar Tomcat.
4.  Accede a la API de REST mediante la ruta contexto configurada.
    Por ejemplo, http://localhost:8080/engine-rest/engine debería de devolver todos los nombres de los motores de procesos existentes en la plataforma, en el caso de que se tuviera la aplicación desplegada en la ruta contexto `/engine-rest`.