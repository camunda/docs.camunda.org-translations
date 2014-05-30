---

title: 'Instalar la aplicación Web REST API'
shortTitle: 'Instalación de REST API'
category: 'Aplicaciones Web'

---

Para instalar la API de REST, se requiere la instalación del módulo `org.camunda.bpm.camunda-engine` de Glassfish.
Mirar arriba, cómo [instalar la distribución pre-built](ref:#bpm-platform-install-the-pre-built-distro) o [Instalar la plataforma en Glassfish](ref:#bpm-platform-install-the-platform-on-a-vanilla-glassfish).

**Nota**: La distribución ya expone la API de REST en la siguiente ruta contexto (context path) `/engine-rest`.

Se requieren los siguientes pasos para desplegar la API de REST en una instancia de Glassfish:

1. Descargar la aplicación web REST API desde [nuestro servidor](https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/camunda-engine-rest/).
   Escoger la versión adecuada con el nombre `$PLATFORM_VERSION/camunda-engine-rest-$PLATFORM_VERSION.war`.
2. Opcionalmente, se debería de cambiar la ruta contexto sobre la cual la API de REST se desplega (por defecto se encuentra en `/engine-rest`).
   Edita el fichero `WEB-INF/sun-web.xml` que está contenido en el fichero WAR y actualiza el valor de `context-root` de manera adecuada.
3. Copia el fichero WAR en la siguiente ruta `$GLASSFISH_HOME/domains/domain1/autodeploy`.
4. Arranca el Servidor de Aplicaciones Glassfish.
5. Accede a la API de REST mediante la ruta contexto configurada.
   Por ejemplo, la siguiente llamada a servicios REST expuestos <a href="http://localhost:8080/engine-rest/engine">http://localhost:8080/engine-rest/engine</a> debería de devolver todos los nombres de los motores de procesos existentes en la plataforma,
   en el caso de que se tuviera la aplicación desplegada en la ruta contexto `/engine-rest`. Para más información de cómo usar la API de REST
