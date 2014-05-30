---

title: 'Instalar Camunda Cycle'
category: 'Aplicaciones Web'

---


**Nota**: La distribución de Camunda ofrece la aplicación web Camunda Cycle. Para poder acceder a ella, se debe de hacer mediante la ruta contexto `/cycle`. Ver [aquí](ref:#web-applications-install-camunda-cycle-configuring-the-pre-packaged-distribution), cómo configurar la distribución.</br>No recomendamos la instalación de Camunda Cycle junto con otros componentes de la plataforma (webapps, engine, REST Api) en el mismo entorno de ejecución. Este tipo de instalaciones compartidas no están soportadas.


## Creación del esquema de base de datos Camunda Cycle

A menos que no se esté utilizando la distribucion pre-empaquetada y no se requiera cambiar la base de datos proporcionada H2, se debe de crear un esquema de base de datos para Camunda Cycle.
La distribución BPM Camunda ofrece un conjunto de scripts SQL de creación que pueden ser ejecutados por un administrador de bases de datos.

Los scripts de creación de base de datos los podemos encontrar en la carpeta `sql/create`:

```
camunda-bpm-tomcat-$PLATFORM_VERSION.zip/sql/create/*_cycle_$PLATFORM_VERSION.sql
```

En esta carpeta encontraremos scripts SQL individuales para cada tipo de base de datos soportada. Selecciona el script apropiado para tu base de datos y ejecútalo mediante la herramienta de administración de base de datos que convenga. (por ejemplo SqlDeveloper para Oracle).

Recomendamos la creación de una base de datos, o esquema de base de datos, separada para Camunda Cycle.

<div class="alert alert-info">
  Si no dispones de la distribución, también puedes descargarte un fichero que contiene estos
  scripts desde <a href="https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/cycle/camunda-cycle-sql-scripts/">nuestro servidor</a>.
  Escoge la versión adecuada con el nombre <code>$PLATFORM_VERSION/camunda-cycle-sql-scripts-$PLATFORM_VERSION.war</code>.
</div>


## Configurando la distribución pre-empaquetada

La distribución pre-empaquetada se ofrece con una base de datos H2 preconfigurada para ser usada por Cycle.

El driver JDBC para H2 se encuentra en `camunda-bpm-tomcat-$PLATFORM_VERSION.zip/server/apache-tomcat-VERSION/lib/h2-VERSION.jar`.

### Cambio de base de datos

Para cambiar la base de datos preconfigurada H2 por una propia, por ejemplo Oracle, tienes que hacer lo siguiente:

1.  Copia tu driver JDBC de base de datos en `$CATALINA_HOME/lib`.
2.  Abre `$CATALINA_HOME/webapps/cycle/META-INF/context.xml` and edit the properties of the `jdbc/CycleDS` datasource definition.


## Instalación de Camunda Cycle en Tomcat 7

Puedes descargar la aplicación web Camunda Cycle desde [nuestro servidor](https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/cycle/camunda-cycle-tomcat/).
Escoge la versión adecuada con el nombre `$PLATFORM_VERSION/camunda-cycle-tomcat-$PLATFORM_VERSION.war`.

### Crear un datasource

El datasource de Cycle está configurado en la aplicación web mediante el fichero `META-INF/context.xml`. Este datasource debería de nombrarse como `jdbc/CycleDS`.

Para utilizar un nombre de datasource customizado, debes editar el fichero que se encuentra en la siguiente ruta de la aplicación web de Camunda Cycle `WEB-INF/classes/META-INF/cycle-persistence.xml`.

Para poder usar el `org.apache.tomcat.jdbc.pool.DataSourceFactory`, necesitas añadir el driver de la base de datos que utilices en la carpeta `$CATALINA_HOME/lib`.
Por ejemplo, si decides usar la base de datos H2, deberías de añadir el fichero h2-VERSION.jar.

<div class="alert alert-info">
  <strong>Tomcat 6.x</strong>
  <br/>
  En Tomcat 6, también deberás añadir el fichero tomcat-jdbc.jar, el cual se proporciona con Tomcat 7 y con la distribución pre-empaquetada de Camunda BPM, en
  <code>$CATALINA_HOME/lib</code>.
</div>

### Instalar la aplicación web

1.  Copia el fichero WAR en la siguiente ruta `$CATALINA_HOME/webapps`.
    Opcionalmente, se debería de cambiar la ruta contexto sobre la cual Cycle se desplega como `/cycle`.
2.  Arrancar Tomcat.
3.  Puedes acceder a Camunda Cycle mediante el contexto anteriormente configurado. Si Cycle se ha instalado correctamente, debería de aparecer una pantalla que te permita crear un usuario inicial.
    El usuario inicial posee privilegios de administrador y puede ser usado para crear más usuarios una vez esté logeado en el sistema.


## Configurando Cycle

### Configurando email

**Nota**: Este paso es opcional y puede saltarse si no se requiere que Cycle envie mails de bienvenida a los nuevos usuarios creados.

<div class="alert alert-info">
  Si no estás utilizando la distribución pre-empaquetada de Tomcat, necesitas instalar la librería java-mail.
  Descarga manualmente la versión 1.4.x de http://mvnrepository.com/artifact/javax.mail/mail y cópiala en tu carpeta <code>$CATALINA_HOME/lib</code>.
</div>

Para poder usar el servicio de de email de Cycle, tienes que configurar una sesión de correo en el fichero de la aplicación web `META-INF/context.xml`.

Por defecto, Cycle busca la sesión de correo mediante JNDI Name `mail/Session`.
El nombre de la sesión de correo a buscar puede ser modificado mediante la edición del siguiente fichero dentro de la aplicación web Cycle:

```
WEB-INF/classes/spring/configuration.xml
```

El fichero define un Bean de Spring con el nombre `cycleConfiguration`. En este Bean de Spring, se setea el nombre JNDI de la Mail Session con un nombre customizado:

```xml
<bean id="cycleConfiguration" class="org.camunda.bpm.cycle.configuration.CycleConfiguration">
  <!-- ... -->
  <!-- cycle email service configuration -->
  <property name="emailFrom" value="cycle@localhost" />
  <property name="mailSessionName" value="my/mail/Session" />
  <!-- ... -->
</bean>
```

### Configurando Connector Password Encryption

Las contraseñas de los conectores son encriptadas mediante la utilización del algoritmo PBEWithMD5AndDES antes de ser guardadas en la base de datos Cycle.

<div class="alert alert-info">
  <strong>Clave de Encriptación</strong>
  <br/>
  Cycle utiliza una clave por defecto para encriptar contraseñas (contenida en el código fuente y, por lo tanto, no es segura).
  Si quieres mejorar la seguridad puedes cambiar la contraseña encriptada creando un fichero en <code>$USER_HOME/cycle.password</code> que contenga una contraseña ASCII que sea auto invocada.
</div>
