---

title: 'Instalar la plataforma BPM en JBoss'
shortTitle: 'Instalación en JBoss'
category: 'Plataforma BPM'

---


1. Descargar la distribución Camunda para JBoss de [nuestro servidor](https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/jboss/camunda-bpm-jboss/).
   Escoger la versión adecuada con el nombre `$PLATFORM_VERSION/camunda-bpm-jboss-$PLATFORM_VERSION.zip` o `$PLATFORM_VERSION/camunda-bpm-jboss-$PLATFORM_VERSION.tar.gz`.
2. Desempaquetar la carpeta `modules` del archivo descargado.
3. Mezclar todo el contenido de la carpeta anterior en el directorio $JBOSS_HOME/modules/.
4. Modificar según se requiera el fichero `$JBOSS_HOME/standalone/configuration/standalone.xml` (o la configuración aplicable a tu instalación de JBoss) como se describe abajo.
5. Modificar los datasources según se requiera (ver abajo), por defecto se hace uso de la base de datos H2.
6. Arrancar el servidor.


## Configurando standalone.xml

Aquí describimos los cambios necesarios en el fichero de configuración de JBoss `$JBOSS_HOME/standalone/configuration/standalone.xml`. Estas modificaciones vienen realizadas en la distribución del servidor pre-empaquetado.

Añadir el subsistema Camunda como extensión:

```xml
<server xmlns="urn:jboss:domain:1.1">
  <extensions>
    ...
    <extension module="org.camunda.bpm.jboss.camunda-jboss-subsystem"/>
```

Añadir los siguientes elementos para crear un pool de threads para el Job Executor en la sección `<subsystem xmlns="urn:jboss:domain:threads:1.1">`:

```xml
<subsystem xmlns="urn:jboss:domain:threads:1.1">
  <bounded-queue-thread-pool name="job-executor-tp" allow-core-timeout="true">
    <core-threads count="3" />
    <queue-length count="3" />
    <max-threads count="10" />
    <keepalive-time time="10" unit="seconds" />
  </bounded-queue-thread-pool>
</subsystem>
```

El nombre del pool de threads es referenciado en la configuración del Job Executor del subsistema Camunda BPM. Esta parte también configura el motor de procesos por defecto.

```xml
<subsystem xmlns="urn:org.camunda.bpm.jboss:1.1">
  <process-engines>
    <process-engine name="default" default="true">
      <datasource>java:jboss/datasources/ProcessEngine</datasource>
      <history-level>full</history-level>
      <properties>
        <property name="jobExecutorAcquisitionName">default</property>
        <property name="isAutoSchemaUpdate">true</property>
        <property name="authorizationEnabled">true</property>
      </properties>
    </process-engine>
  </process-engines>
  <job-executor>
    <thread-pool-name>job-executor-tp</thread-pool-name>
    <job-acquisitions>
      <job-acquisition name="default">
        <acquisition-strategy>SEQUENTIAL</acquisition-strategy>
        <properties>
          <property name="lockTimeInMillis">300000</property>
          <property name="waitTimeInMillis">5000</property>
          <property name="maxJobsPerAcquisition">3</property>
        </properties>
      </job-acquisition>
    </job-acquisitions>
  </job-executor>
</subsystem>
```


## Creando un datasource

La plataforma BPM de Camunda necesita crear un data-source con el siguiente nombre `java:jboss/datasources/ProcessEngine`.
La siguiente configuración de datasource muestra un ejemplo de uso de la base de datos H2, utilizando un fichero que contiene la carpeta `./`,
usualmente `bin`.

**Nota**: si se ejecuta el script desde una ruta diferente, la base de datos se almacenerá allí!

```xml
<datasource jta="true" enabled="true" use-java-context="true" use-ccm="true"
            jndi-name="java:jboss/datasources/ProcessEngine" 
            pool-name="ProcessEngine">
  <connection-url>jdbc:h2:./camunda-h2-dbs/process-engine;DB_CLOSE_DELAY=-1;MVCC=TRUE;DB_CLOSE_ON_EXIT=FALSE</connection-url>
  <driver>h2</driver>
  <security>
    <user-name>sa</user-name>
    <password>sa</password>
  </security>
</datasource>
```

<div class="alert alert-info">
  <strong>Cycle</strong><br/>
  En el caso de que se requiera utilizar Camunda Cycle, necesitas configurar un datasource adicional. Mira la sección destinada a ello, la <a href="ref:#web-applications-install-camunda-cycle">sección Cycle</a> te guiará.
</div>

El uso de la base de datos H2 es ideal para propósitos de desarrollo, pero se desaconseja su uso en entornos de producción.
Los siguientes enlaces muestran los recursos necesarios para el uso de otras bases de datos:

*   [Cómo configurar una base de datos Oracle](http://blog.foos-bar.com/2011/08/jboss-as-7-and-oracle-datasource.html)
*   [Cómo configurar una base de datos MySQL](http://javathreads.de/2011/09/jboss-as-7-mysql-datasource-konfigurieren/)
*   **Importante**: Para DB2, comprueba las incidencias existentes en [Installation Troubleshooting and FAQ](https://app.camunda.com/confluence/display/foxUserGuide/Installation+Troubleshooting+and+FAQ)


## Usando un datasource XA

Desde Camunda **recomendamos encarecidamente** el uso de un data-source XA en entornos de producción.
Desde que el acceso se realiza normalmente desde dentro de los procesos contra recursos transaccionales, el riesgo de tener incosistencias en este caso es alto.
Por ejemplo, para H2 se podría usar con la siguiente configuración:

```xml
<xa-datasource jndi-name="java:jboss/datasource/ProcessEngine" pool-name="ProcessEngine" enabled="true" use-ccm="false">
  <xa-datasource-property name="URL">jdbc:h2:./camunda-h2-dbs/process-engine;DB_CLOSE_DELAY=-1;MVCC=TRUE;DB_CLOSE_ON_EXIT=FALSE</xa-datasource-property>
  <driver>h2</driver>
  <xa-pool>
    <max-pool-size>10</max-pool-size>
    <is-same-rm-override>false</is-same-rm-override>
    <interleaving>false</interleaving>
    <pad-xid>false</pad-xid>
    <wrap-xa-resource>false</wrap-xa-resource>
  </xa-pool>
  <security>
    <user-name>sa</user-name>
    <password>sa</password>
  </security>
  <validation>
    <validate-on-match>false</validate-on-match>
    <background-validation>false</background-validation>
    <background-validation-millis>0</background-validation-millis>
  </validation>
  <statement>
    <prepared-statement-cache-size>0</prepared-statement-cache-size>
    <share-prepared-statements>false</share-prepared-statements>
  </statement>
</xa-datasource>
```

Para otras bases de datos, aplicar los siguientes recursos:

*   [Oracle XA datasource](http://docs.jboss.org/ironjacamar/userguide/1.0/en-US/html_single/#ex_datasources_oracle_xa)
*   [MySQL XA datasource](http://docs.jboss.org/ironjacamar/userguide/1.0/en-US/html_single/#ex_datasources_mysql_xa)
*   [DB2 XA datasource](http://docs.jboss.org/ironjacamar/userguide/1.0/en-US/html_single/#ex_datasources_db2_xa)