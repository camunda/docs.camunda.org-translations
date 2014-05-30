---

title: 'Instalar Camunda Cycle'
category: 'Aplicaciones Web'

---


**Nota**: La distribución de Camunda ofrece la aplicación web Camunda Cycle. Para poder acceder a ella, se debe de hacer mediante la ruta contexto `/cycle`. Ver [aquí](ref:#web-applications-install-camunda-cycle-configuring-the-pre-packaged-distribution), cómo configurar la distribución.</br>No recomendamos la instalación de Camunda Cycle junto con otros componentes de la plataforma (webapps, engine, REST Api) en el mismo entorno de ejecución. Este tipo de instalaciones compartidas no están soportadas.


## Creación del esquema de base de datos Camunda Cycle

A menos que no se esté utilizando la distribucion pre-empaquetada y no se requiera cambiar la base de datos proporcionada H2, se debe de crear un esquema de base de datos para Camunda Cycle.
La distribución BPM Camunda ofrece un conjunto de scripts SQL de creación que pueden ser ejecutados por un administrador de bases de datos.

Los scripts de creación de base de datos los podemos encontrar en la siguiente carpeta:

```
camunda-bpm-jboss-$PLATFORM_VERSION.zip/sql/create/*_cycle_$PLATFORM_VERSION.sql
```

En esta carpeta encontraremos scripts SQL individuales para cada tipo de base de datos soportada. Selecciona el script apropiado para tu base de datos y ejecútalo mediante la herramienta de administración de base de datos que convenga. (por ejemplo SqlDeveloper para Oracle).

Recomendamos la creación de una base de datos, o esquema de base de datos, separada para Camunda Cycle.

<div class="alert alert-info">
  Si no dispones de la distribución, también puedes descargarte un fichero que contiene estos
  scripts desde [nuestro servidor](https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/cycle/camunda-cycle-sql-scripts/).
  Escoge la versión adecuada con el nombre <code>$PLATFORM_VERSION/camunda-cycle-sql-scripts-$PLATFORM_VERSION.war</code>.
</div>


## Configurando la distribución pre-empaquetada

La distribución pre-empaquetada se ofrece con una base de datos H2 preconfigurada para ser usada por Cycle.

El driver JDBC para H2 se encuentra en `camunda-bpm-jboss-$PLATFORM_VERSION.zip/server/jboss-as-VERSION/modules/com/h2database/h2`.

Éste contiene un datasource preconfigurado con el siguiente nombre `java:jboss/datasources/CycleDS`.

### Cambio de base de datos

Para cambiar la base de datos preconfigurada H2 por una propia, por ejemplo Oracle, tienes que hacer lo siguiente:

1. Desplega tu driver de base de datos. (Comprueba cómo hacerlo con el Manual del Servidor de Aplicaciones JBoss).
2. Define un datasource con el nombre `java:jboss/datasources/CycleDS` para tu base de datos. (Elimina el datasource existente de H2).


## Instalación de Camunda Cycle en JBoss AS 7

Puedes descargar la aplicación web Camunda Cycle desde [nuestro servidor](https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/cycle/camunda-cycle-jboss/).
Escoge la versión adecuada con el nombre `$PLATFORM_VERSION/camunda-cycle-jboss-$PLATFORM_VERSION.war`.

### Crear un datasource

Primero, debes definir un datasource para el Servidor de Aplicaciones JBoss. Aquí asumimos que ya estás familiarizado con el procedimiento.
En caso de duda, comprueba las secciones apropiadas en el manual de tu servidor de aplicaciones.

Cycle requiere un datasource con el nombre `java:jboss/datasources/CycleDS`.

Para utilizar un nombre de datasource customizado, debes editar el fichero que se encuentra en la siguiente ruta de la aplicación web de Camunda Cycle `WEB-INF/classes/META-INF/cycle-persistence.xml`.

### Instalar la aplicación web

1.  Copia el fichero WAR en la siguiente ruta `$JBOSS_HOME/standalone/deployments`.
    Opcionalmente, se debería de cambiar la ruta contexto sobre la cual Cycle se desplega (por defecto se encuentra en `/cycle`).
    Edita el fichero `WEB-INF/jboss-web.xml` que está contenido en el fichero WAR y actualiza el valor de `context-root` de manera adecuada.
2.  Arranca el servidor JBoss AS.
3.  Puedes acceder a Camunda Cycle mediante el contexto anteriormente configurado. Si Cycle se ha instalado correctamente, debería de aparecer una pantalla que te permita crear un usuario inicial.
    El usuario inicial posee privilegios de administrador y puede ser usado para crear más usuarios una vez esté logeado en el sistema.


## Configurando Cycle

### Configurando email

**Nota**: Este paso es opcional y puede saltarse si no se requiere que Cycle envie mails de bienvenida a los nuevos usuarios creados.

Para poder usar el servicio de email de Cycle, debes configurar una sesión de correo en el servidor de aplicaciones y referenciarla en la configuración de Cycle.

Para poder conseguir esto, debes de configurar una nueva sesión de correo en el Servidor de Aplicaciones JBoss. En la configuración por defecto, JBoss AS 7 define una sesión de correo como `java:jboss/mail/Default`.

```xml
<subsystem xmlns="urn:jboss:domain:mail:1.0">
  <mail-session jndi-name="java:jboss/mail/Default">
    <smtp-server outbound-socket-binding-ref="mail-smtp"/>
  </mail-session>
</subsystem>
```

Por defecto, Cycle intentará buscar esta sesión de correo.
El nombre de la sesión de correo a buscar puede ser modificado mediante la edición del siguiente fichero dentro de la aplicación web Cycle:

```
WEB-INF/classes/spring/configuration.xml
```

El fichero define un Bean de Spring con el nombre `cycleConfiguration`. En este Bean de Spring, se setea el nombre JNDI de la Mail Session con un nombre customizado:

```xml
<bean id="cycleConfiguration" class="org.camunda.bpm.cycle.configuration.CycleConfiguration">
  <!-- ... -->
  <!-- Cycle email service configuration -->
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
