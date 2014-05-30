---

title: 'Instalar Camunda Cockpit y Tasklist'
category: 'Aplicaciones Web'

---


Para instalar Camunda Cockpit y Tasklist, se requiere la instalación del módulo `org.camunda.bpm.camunda-engine` de JBoss.
Mirar arriba, cómo [instalar la distribución pre-built](ref:#bpm-platform-install-the-pre-built-distro) o [Instalar la plataforma en JBoss](ref:#bpm-platform-install-the-platform-on-a-vanilla-jboss).

**Nota**: La distribución ya proporciona las aplicaciones. Se debe de acceder a ellas mediante `/camunda/app/cockpit` y `/camunda/app/tasklist`, respectivamente.

Se requieren los siguientes pasos para desplegar las aplicaciones en una instancia de JBoss:

1.  Descargar la aplicación web Camunda que contiene ambas aplicaciones desde [nuestro servidor](https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/webapp/camunda-webapp-jboss/).
    Escoger la versión adecuada con el nombre `$PLATFORM_VERSION/camunda-webapp-jboss-$PLATFORM_VERSION.war`.
2.  Opcionalmente, se debería de cambiar la ruta contexto sobre la cual las aplicaciones se desplegan (por defecto se encuentra en `/camunda`).
   	Edita el fichero `WEB-INF/jboss-web.xml` que está contenido en el fichero WAR y actualiza el valor de `context-root` de manera adecuada.
2.  Copia el fichero WAR en la siguiente ruta `$JBOSS_HOME/standalone/deployments`.
3.  Arranca el servidor JBoss AS.
4.  Accede a Cockpit y Tasklist mediante la dirección `/camunda/app/cockpit` y `/camunda/app/tasklist` o a través del contexto que se haya configurado.