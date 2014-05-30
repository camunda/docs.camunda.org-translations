---

title: 'Instalar la plataforma BPM en Tomcat'
shortTitle: 'Instalación en Tomcat'
category: 'Plataforma BPM'

---


Esta sección describirá cómo puedes instalar la plataforma BPM de Camunda en [vanilla Tomcat 7](http://tomcat.apache.org/), en el caso de que no puedas utilizar la distribución pre-empaquetada de Tomcat. Independientemente recomendamos el uso de los módulos requeridos [descargar la distribución Tomcat 7](http://camunda.org/download/).


## Crear el esquema de base de datos para la plataforma BPM Camunda

Si no quieres usar la base de datos H2, primero tienes que crear un esquema de base de datos para la plataforma BPM de Camunda. La distribución BPM de Camunda ofrece un conjunto de scripts SQL de creación que pueden ser ejecutados por un administrador de base de datos.

Los scripts de creación de base de datos se encuentran en la carpeta `sql/create`:

`$TOMCAT_DISTRIBUTION/sql/create/*_engine_$PLATFORM_VERSION.sql`
`$TOMCAT_DISTRIBUTION/sql/create/*_identity_$PLATFORM_VERSION.sql`

Hay un script SQL individual para cada base de datos soportada. Selecciona el script apropiado para tu base de datos y ejecútalo con la herramienta de administración de base de datos que se requiera. (por ejemplo SqlDeveloper para Oracle).

## Añadir BPM Bootstrap Server Listener

Añadir la entrada `org.camunda.bpm.container.impl.tomcat.TomcatBpmPlatformBootstrap` como Listener antes de la entrada `GlobalResourcesLifecycleListener` en tu `$TOMCAT_HOME/conf/server.xml`. Esta clase se encarga de ejecutar / parar la plataforma BPM de Camunda cuando Tomcat se ejecute / pare.

```xml
<Server port="8005" shutdown="SHUTDOWN">
  ...
  <Listener className="org.camunda.bpm.container.impl.tomcat.TomcatBpmPlatformBootstrap" />
  ...
```

## Configurando un recurso JDBC

Para configurar un recurso JDBC tienes que modificar el archivo `$TOMCAT_HOME/conf/server.xml`. Esto puede hacerse como ejemplo para una base de datos H2 como sigue:

```xml
<Server>
  ...
  <GlobalNamingResources>
    ...
    <Resource name="jdbc/ProcessEngine"
              auth="Container"
              type="javax.sql.DataSource"
              factory="org.apache.tomcat.jdbc.pool.DataSourceFactory"
              uniqueResourceName="process-engine"
              driverClassName="org.h2.Driver"
              url="jdbc:h2:./camunda-h2-dbs/process-engine;MVCC=TRUE;TRACE_LEVEL_FILE=0"
              username="sa"
              password=""
              maxPoolSize="20"
              minPoolSize="5" />
  </GlobalNamingResources>
</Server>
```


## Añadir las librerías necesarias a Tomcat 7

Primero tienes que copiar las siguientes librerías en la carpeta de librerías de Tomcat `$TOMCAT_HOME/lib`:

`$TOMCAT_DISTRIBUTION/lib/camunda-engine-$PLATFORM_VERSION.jar`

`$TOMCAT_DISTRIBUTION/lib/camunda-identity-ldap-$PLATFORM_VERSION.jar` (if you want to use LDAP)

`$TOMCAT_DISTRIBUTION/lib/camunda-bpmn-model-$PLATFORM_VERSION.jar`

`$TOMCAT_DISTRIBUTION/lib/camunda-xml-model-$PLATFORM_VERSION.jar`

`$TOMCAT_DISTRIBUTION/lib/java-uuid-generator-VERSION.jar`

`$TOMCAT_DISTRIBUTION/lib/joda-time-VERSION.jar`

`$TOMCAT_DISTRIBUTION/lib/mybatis-VERSION.jar`

Después de esto, tienes que copiar el correspondiente driver JDBC en la carpeta `$TOMCAT_HOME/lib`.


## Añadir bpm-platform.xml

Tienes que añadir el archivo `bpm-platform.xml` en la carpeta `$TOMCAT_HOME/conf` u opcionalmente puedes configurar la localización a través de la cual se pueden disponer de ciertos mecanismos, ver [Configurar la localización del fichero bpm-platform.xml](ref:/api-references/deployment-descriptors/#descriptors-bpm-platformxml-configure-location-of-the-bpm-platformxml-file):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<bpm-platform xmlns="http://www.camunda.org/schema/1.0/BpmPlatform"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.camunda.org/schema/1.0/BpmPlatform http://www.camunda.org/schema/1.0/BpmPlatform ">

  <job-executor>
    <job-acquisition name="default" />
  </job-executor>

  <process-engine name="default">
    <job-acquisition>default</job-acquisition>
    <configuration>org.camunda.bpm.engine.impl.cfg.StandaloneProcessEngineConfiguration</configuration>
    <datasource>java:jdbc/ProcessEngine</datasource>

    <properties>
      <property name="history">full</property>
      <property name="databaseSchemaUpdate">true</property>
      <property name="authorizationEnabled">true</property>
    </properties>

  </process-engine>

</bpm-platform>
```
