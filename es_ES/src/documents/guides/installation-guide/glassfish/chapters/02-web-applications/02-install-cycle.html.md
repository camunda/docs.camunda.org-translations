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
camunda-bpm-glassfish-$PLATFORM_VERSION.zip/sql/create/*_cycle_$PLATFORM_VERSION.sql
```

En esta carpeta encontraremos scripts SQL individuales para cada tipo de base de datos soportada. Selecciona el script apropiado para tu base de datos y ejecútalo mediante la herramienta de administración de base de datos que convenga. (por ejemplo SqlDeveloper para Oracle).

Recomendamos la creación de una base de datos, o esquema de base de datos, separada para Camunda Cycle.

<div class="alert alert-info">
  If you have not got the distro at hand, you can also download a file that packages these
  scripts from <a href="https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/cycle/camunda-cycle-sql-scripts/">our server</a>.
  Choose the correct version named <code>$PLATFORM_VERSION/camunda-cycle-sql-scripts-$PLATFORM_VERSION.war</code>.
</div>


## Configurando la distribución pre-empaquetada

La distribución pre-empaquetada se ofrece con una base de datos H2 preconfigurada para ser usada por Cycle.

El driver JDBC para H2 se encuentra en `camunda-bpm-glassfish-$PLATFORM_VERSION.zip/server/glassfish3/glassfish/lib/h2-VERSION.jar`.

Cuando abres la consola de administración de Glassfish (por defecto http://localhost:4848), puedes encontrar en
`Resources -> JDBC -> JDBC Connection Pools` la preconfiguración del pool de conexiones H2CyclePool
y en `Resources -> JDBC -> JDBC Resources` el nombre correspondiente del recurso JDBC `jdbc/CycleDS`.

### Intercambio de datasource

Para intercambiar el datasource H2 preconfigurado, por ejemplo Oracle, tienes que hacer lo siguiente:

1. Copia tu fichero del driver JDBC de base de datos en `$GLASSFISH_HOME/glassfish/lib` o en el caso de que quieras integrar el driver JDBC dentro de Glassfish como Servidor de Dominio,
   cópialo dentro del directorio `$GLASSFISH_HOME/glassfish/domains/DOMAIN_DIR/lib`, y reinicia el servidor.
2. Abre la consola de administración de Glassfish (por defecto http://localhost:4848).
3. Ves a `Resources -> JDBC -> JDBC Connection Pools` y haz click en **New**. ([ver imagen](assets/img/jdbc_connection_pools.png))
4. En la siguiente página, debajo de *General Settings*, introduce un nombre en *Pool Name*, que identificará el *Connection Pool*, en nuestro caso es *OracleCyclePool*.
   Elige también la propiedad *Resource Type*, por ejemplo `javax.sql.XADatasource` porque vamos a usar un datasource XA en este ejemplo.
   Se debe de especificar un *Database Driver Vendor* para el driver, por ejemplo Oracle en nuestro caso. Haz click en **Next**. ([ver imagen](assets/img/jdbc_new_pool_step_1.png))
5. En el siguiente paso cuando el driver de base de datos no se reconocido por Glassfish, necesitas introducir un nombre para *Datasource Classname* o para *Driver Classname*,
   en cualquier otro caso se autorellenan. Las opciones de *Pool Settings and Transaction* las puedes dejar como están. ([ver imagen](assets/img/jdbc_new_pool_step_2_1.png))
6. Al final de la página, encontrarás un tabla con el nombre *Additional Properties*.
   Aquí puedes introducir propiedades para el driver de base de datos como puedan ser el usuario y la contraseña, la url de la base de datos, etc. ([ver imagen](assets/img/jdbc_new_pool_step_2_2.png))
7. Haz click en **Save** para guardar los cambios.
8. Para probar si la configuración de la conexión a base de datos es correcta, ves a `Resources -> JDBC -> JDBC Connection Pools -> Your Connection Pool` y haz click en el botón **Ping** arriba del todo.
   Si no se muestra ningún error, procede con el paso 9, en caso contrario consulta el manual de Glassfish o de la base de datos.
9. Ahora tenemos que cambiar el pool de conexiones preconfigurado para el recién creado recurso de JDBC. Para hacer esto, ves a `Resources -> JDBC -> JDBC Resources -> jdbc/CycleDS`
   y edita el nombre de *Pool Name* `H2CyclePool` al nuevo nombre del pool de conexiones, `OracleCyclePool`. ([ver imagen](assets/img/jdbc_edit_resource.png))
10. **Save** guarda los cambios.
11. Felicidades! Ahora Cycle se puede conectar a tu base de datos.


## Instalación de Camunda Cycle en GlassFish

Puedes descargar la aplicación web Camunda Cycle desde [nuestro servidor](https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/cycle/camunda-cycle-glassfish/).
Escoge la versión adecuada con el nombre `$PLATFORM_VERSION/camunda-cycle-glassfish-$PLATFORM_VERSION.war`.

### Crear un datasource

Primero, debes definir un datasource para el Servidor de Aplicaciones Glassfish. Aquí asumimos que ya estás familiarizado con el procedimiento.
En caso de duda, comprueba las secciones apropiadas en el manual de tu servidor de aplicaciones.

Cycle requiere un datasource con el nombre `jdbc/CycleDS`.

Para utilizar un nombre de datasource customizado, debes editar el fichero que se encuentra en la siguiente ruta de la aplicación web de Camunda Cycle `WEB-INF/classes/META-INF/cycle-persistence.xml`.

### Modificar default-web.xml

<div class="alert alert-info">
    Para conseguir que Cycle funcione, <strong>ES OBLIGATORIO</strong> modificar el fichero <code>$GLASSFISH_HOME/glassfish/domains/domain1/config/default-web.xml</code>,
    en caso contrario obtendrás una ventana de AUTENTICACIÓN BÁSICA, cuando intentes logarte en Cycle.
</div>

Abre el fichero `default-web.xml` y elimina los comentarios de las siguientes líneas del final del fichero.

```xml
<login-config>
  <auth-method>BASIC</auth-method>
</login-config>
```

### Instalar la aplicación web

1. Opcionalmente, podrías cambiar la ruta contexto en la cual se despliega Cycle (por defecto es `/cycle`).
   Edita el fichero `WEB-INF/sun-web.xml` que se encuentra dentro del fichero War y actualiza el atributo `context-root` según corresponda.
2. Arranca Glassfish.
3. Abre la Consola de Administración de Glassfish.
4. Navega hasta *Applications* y haz click en **Deploy**.
5. En la siguiente página, debajo de *Location*, selecciona la opción apropiada y busca tus ficheros y selecciona el fichero `camunda-cycle-glassfish-$PLATFORM_VERSION.war`.
   ([show image](assets/img/deploy_app.png))
6. Deja el resto de propiedades como están y haz click en **Ok**. Opcionalmente, puedes cambiar la ruta contexto para Cycle aquí. Proponemos `/cycle`.
7. Accede a Camunda Cycle a través del contexto que has configurado. Si Cycle se ha instalado correctamente, debería de aparecer una ventana que te permita crear el usuario inicial.
   El usuario inicial posee privilegios de administrador y puede ser usado para crear más usuarios una vez esté logeado en el sistema.

## Configurando Cycle

### Configurando email

**Nota**: Este paso es opcional y puede saltarse si no se requiere que Cycle envie mails de bienvenida a los nuevos usuarios creados.

Para poder usar el servicio de email de Cycle, debes configurar una sesión de correo en el servidor de aplicaciones y referenciarla en la configuración de Cycle.

Para poder conseguir esto, debes de configurar una nueva sesión de correo en el Servidor de Aplicaciones Glassfish.

1.  Abre la Consola de Administración de Glassfish.
2.  Ves a `Resources -> JavaMail Sessions` y haz click en **New**.
3.  Introduce un nombre JNDI para laMail Session, por ejemplo `mail/Session` y rellena los campos obligatorios para tu configuración del servidor de correo. Después, el nombre JNDI deberá de ser referenciado en la configuración de Cycle.
    Para una descripción general completa de las propiedades de la sesión de correo, como protocolos de encriptación etc, consulta el manual de Glassfish.
    Debajo de *Additional Properties*, añade esas propiedades si tu servidor de correo requiere auntenticación por contraseña.

    ```
    mail.smtp.port:port-number
    mail.smtp.auth true
    mail.smtp.password: myMailServerPassword
    ```

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
