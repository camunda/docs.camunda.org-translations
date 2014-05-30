---

title: 'Instalar Camunda Cockpit y Tasklist'
category: 'Aplicaciones Web'

---

Para instalar Camunda Cockpit y Tasklist, se requiere la instalación del módulo `org.camunda.bpm.camunda-engine` de Tomcat.
Mirar arriba, cómo [instalar la distribución pre-built](ref:#bpm-platform-install-the-pre-built-distro) o [Instalar la plataforma en Tomcat](ref:#bpm-platform-install-the-platform-on-a-vanilla-tomcat).

**Nota**: La distribución ya proporciona las aplicaciones. Se debe de acceder a ellas mediante `/camunda/app/cockpit` y `/camunda/app/tasklist`, respectivamente.

Se requieren los siguientes pasos para desplegar las aplicaciones en una instancia de Tomcat:

1. Descargar la aplicación web Camunda que contiene ambas aplicaciones desde [nuestro servidor](https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/webapp/camunda-webapp-tomcat/).
   Escoger la versión adecuada con el nombre `$PLATFORM_VERSION/camunda-webapp-tomcat-$PLATFORM_VERSION.war`.
2. Copia el fichero WAR en la siguiente ruta `$CATALINA_HOME/webapps/camunda.war`.
   Opcionalmente puedes nombrarlo de manera diferente o extraerlo en una carpeta para desplegarlo con una ruta de contexto diferente.
3. Arrancar Tomcat.
4. Accede a Cockpit y Tasklist mediante la dirección `/camunda/app/cockpit` y `/camunda/app/tasklist` o a través del contexto que se haya configurado.
