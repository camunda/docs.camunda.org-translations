---

title: 'Instalar la plataforma BPM en Glassfish'
shortTitle: 'Instalación en Glassfish'
category: 'Plataforma BPM'

---

Esta sección describirá cómo puedes instalar la plataforma BPM de Camunda en [vanilla Glassfish 3.1](http://glassfish.java.net/), en el caso de que no puedas utilizar la distribución pre-empaquetada de Glassfish. Independientemente recomendamos el uso de los módulos requeridos [descargar la distribución Glassfish 3.1](http://camunda.org/download/).

### <a id="database-camunda-bpm-platform"></a>Crear el esquema de base de datos para la plataforma BPM Camunda

Si no quieres usar la base de datos H2, primero tienes que crear un esquema de base de datos para la plataforma BPM de Camunda. La distribución BPM de Camunda ofrece un conjunto de scripts SQL de creación que pueden ser ejecutados por un administrador de base de datos.

Los scripts de creación de base de datos se encuentran en la carpeta `sql/create`:

`$GLASSFISH_DISTRIBUTION/sql/create/*_engine_$PLATFORM_VERSION.sql`
`$GLASSFISH_DISTRIBUTION/sql/create/*_identity_$PLATFORM_VERSION.sql`

Hay un script SQL individual para cada base de datos soportada. Selecciona el script apropiado para tu base de datos y ejecútalo con la herramienta de administración de base de datos que se requiera. (por ejemplo SqlDeveloper para Oracle).

<!--Las siguientes secciones describen cómo configurar Glassfish e instalar la plataforma BPM de Camunda en Glassfish. Si lo prefieres puedes hacer lo siguiente [se puede configurar vía Consola de Administración de Glassfish](ref:#configuring-admin-console) y [skip](ref:#configuring-admin-console) las siguientes secciones.-->

### <a id="configuring-jdbc"></a>Configurando el Pool de Conexiones y los Recursos JDBC

El Pool de Conexiones y los Recursos JDBC pueden ser configurados editando el fichero `domain.xml` que se encuentra dentro de la carpeta `$GLASSFISH_HOME/glassfish/domains/<domain>/config/`.

El siguiente ejemplo muestra la configuración basada en la base de datos H2.

```xml
    <domain>
      ...
      <resources>
        ...
        <jdbc-resource pool-name="ProcessEnginePool"
                       jndi-name="jdbc/ProcessEngine"
                       enabled="true">
        </jdbc-resource>

        <jdbc-connection-pool is-isolation-level-guaranteed="false"
                              datasource-classname="org.h2.jdbcx.JdbcDataSource"
                              res-type="javax.sql.DataSource"
                              non-transactional-connections="true"
                              name="ProcessEnginePool">
          <property name="Url"
                    value="jdbc:h2:./camunda-h2-dbs/process-engine;DB_CLOSE_DELAY=-1;MVCC=TRUE;DB_CLOSE_ON_EXIT=FALSE">
          </property>
          <property name="User" value="sa"></property>
          <property name="Password" value="sa"></property>
        </jdbc-connection-pool>
      </resources>

      <servers>
        <server>
          ...
          <resource-ref ref="jdbc/ProcessEngine"></resource-ref>
        </server>
      </servers>
    </domain>
```

En el caso de que utilices otra base de datos diferente a H2 (por ejemplo DB2, MySQL etc.) tienes que modificar la propiedad `datasource-classname` y `res-type` con las correspondientes clases de base de datos y setear las propiedades específicas de base de datos (como la url etc.) dentro del Pool de Conexiones JDBC. Además tienes que añadir el correspondiente driver de JDBC en `$GLASSFISH_HOME/glassfish/lib/`. Por ejemplo puedes añadir el driver de JDBC para H2 el cual puedes encontrar en `$GLASSFISH_DISTRIBUTION/server/glassfish3/glassfish/lib/h2-VERSION.jar` para ejecutarlo con la base de datos H2.

### <a id="configuring-thread-pool"></a>Configurando Thread Pool para Job Executor

Para ello tienes que editar el fichero `$GLASSFISH_HOME/glassfish/domains/<domain>/config/domain.xml` y añadir los siguientes elementos a la sección `resources`.

```xml
    <domain>
      ...
      <resources>
        ...
        <resource-adapter-config
          enabled="true"
          resource-adapter-name="camunda-jobexecutor-rar"
          thread-pool-ids="platform-jobexecutor-tp" >
        </resource-adapter-config>

        <connector-connection-pool
            enabled="true"
            name="platformJobExecutorPool"
            resource-adapter-name="camunda-jobexecutor-rar"
            connection-definition-name=
                "org.camunda.bpm.container.impl.threading.jca.outbound.JcaExecutorServiceConnectionFactory"
            transaction-support="NoTransaction" />

        <connector-resource
            enabled="true"
            pool-name="platformJobExecutorPool"
            jndi-name="eis/JcaExecutorServiceConnectionFactory" />
      </resources>

      <servers>
        <server>
          ...
          <resource-ref ref="eis/JcaExecutorServiceConnectionFactory"></resource-ref>
        </server>
      </servers>
    </domain>
```

Para configurar el thread pool para el job executor tienes que añadir los correspondientes elementos `config` del fichero `domain.xml`.

```xml
    <domain>
      ...
      <configs>
        ...
        <config name="server-config">
          ...
          <thread-pools>
            ...
            <thread-pool max-thread-pool-size="6"
                         name="platform-jobexecutor-tp"
                         min-thread-pool-size="3"
                         max-queue-size="10">
            </thread-pool>
          </thread-pools>
        </config>
      </configs>
    </domain>
```

### Instalando la plataforma BPM de Camunda

Se requieren los siguientes pasos para poder desplegar la plataforma BPM de Camunda en una instancia de Glassfish:

1. Mezclar las librerías compartidas de `$GLASSFISH_DISTRIBUTION/modules/lib` en el directorio `GLASSFISH_HOME/glassfish/lib` (por ejemplo, copia el contenido en el directorio de las librerías de Glassfish).
2. Copia el resource adapter del jobexecutor `$GLASSFISH_DISTRIBUTION/modules/camunda-jobexecutor-rar-$PLATFORM_VERSION.rar` en `$GLASSFISH_HOME/glassfish/domains/<domain>/autodeploy`. El resource adapter del jobexecutor se desplegará primero porque el artefacto `camunda-glassfish-ear-$PLATFORM_VERSION.ear` depende de él y no puede ser desplegado correctamente sin el resource adapter. Si intentas desplegar ambos componentes con la propiedad auto-deploy en un mismo paso, deberías de ser consciente que el orden de despliegue no se define en este caso.Debido a esto proponemos arrancar Glassfish para desplegar inicialmente el resource adapter del jobexecutor. Después de un arranque correcto, para el servidor Glassfish.
3. Copia el artefacto `$GLASSFISH_DISTRIBUTION/modules/camunda-glassfish-ear-$PLATFORM_VERSION.ear` en `$GLASSFISH_HOME/glassfish/domains/<domain>/autodeploy`.
4. (Opcional) [COnfigura la localización del fichero bpm-platform.xml](ref:/api-references/deployment-descriptors/#descriptors-bpm-platformxml-configure-location-of-the-bpm-platformxml-file)
5. Arranca Glassfish.
6. Después de que el arranque finalice correctamente, la plataforma BPM de Camunda está instalada.

Como siguiente paso, puedes instalar la [REST API](ref:#web-applications-install-the-rest-api-web-application) en Glassfish.