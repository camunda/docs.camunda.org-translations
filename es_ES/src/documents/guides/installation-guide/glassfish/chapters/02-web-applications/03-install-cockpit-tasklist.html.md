---

title: 'Instalar Camunda Cockpit y Tasklist'
category: 'Aplicaciones Web'

---

Para instalar Camunda Cockpit y Tasklist, se requiere la instalación del módulo `org.camunda.bpm.camunda-engine` de Glassfish.
Mirar arriba, cómo [Instalar la distribución pre-built](ref:#bpm-platform-install-the-pre-built-distro) o [Instalar la plataforma en Glassfish](ref:#bpm-platform-install-the-platform-on-a-vanilla-glassfish).

**Nota**: La distribución ya proporciona las aplicaciones. Se debe de acceder a ellas mediante `/camunda/app/cockpit` y `/camunda/app/tasklist`, respectivamente.

Se requieren los siguientes pasos para desplegar las aplicaciones en una instancia de Glassfish:

1. Descargar la aplicación web Camunda que contiene ambas aplicaciones desde [nuestro servidor](https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/webapp/camunda-webapp-glassfish/).
   Escoger la versión adecuada con el nombre `$PLATFORM_VERSION/camunda-webapp-glassfish-$PLATFORM_VERSION.war`.
2. Opcionalmente, se debería de cambiar la ruta contexto sobre la cual las aplicaciones se desplegan (por defecto se encuentra en `/camunda`).
   Edita el fichero `WEB-INF/sun-web.xml` que está contenido en el fichero WAR y actualiza el valor de `context-root` de manera adecuada.
2. Copia el fichero WAR en la siguiente ruta `$GLASSFISH_HOME/domains/domain1/autodeploy`.
3. Arranca el Servidor de Aplicaciones Glassfish.
4. Accede a Cockpit y Tasklist mediante la dirección `/camunda/app/cockpit` y `/camunda/app/tasklist` o a través del contexto que se haya configurado.
