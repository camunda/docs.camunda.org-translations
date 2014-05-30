---

title: 'Despliegue'
category: 'Instalación'

---

Una vez has descargado el archivo `camunda-webapp-SERVER-standalone-VERSION.war`
deberás desplegarlo en tu servidor de aplicaciones. Nota: Asegúrate de utilizar una
distribución de tu servidor de aplicaciones, no un servidor de aplicaciones descargado
desde Camunda. El procedimiento de despliegue para las aplicaciones web depende de
tu servidor de aplicaciones.  Por favor, dirígete a la documentación de tu servidor de aplicaciones
si no estás seguro de cómo instalar la aplicación.

La ruta del contexto por defecto para la aplicación web de Camunda es `/camunda`. Nota:
Si se instala la aplicación web standalone en Apache Tomcat dejando el archivo WAR
en la carpeta `webapps`, Tomcat asignará el nombre del fichero WAR como
ruta de contexto. Si quieres que la ruta de contexto sea `/camunda` debes renombrar el fichero
WAR a `camunda.war`.

Ahora, tenemos la aplicación desplegada en localhost y corriendo en el puerto 8080
y la ruta de contexto es `/camunda`. Es ahora cuando puedes acceder a la aplicación web
standalone de Camunda utilizando la siguiente URL:

[http://localhost:8080/camunda/](http://localhost:8080/camunda/)
