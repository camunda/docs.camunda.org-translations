---

title: 'Configuración de Base de Datos'
category: 'Configuración'

---

La aplicación web standalone de Camunda está configurada inicialmente con una base de datos basada en archivos `h2`
y un datasource Apache Commons DBCP. La base de datos `h2` sólo se debe de utilizar para propósitos de demostración.
Si quieres utilizar la aplicación web standalone en un entorno de producción, recomendamos
el uso de una base de datos diferente.

Para poder configurar otra base de datos, se debe editar el fichero con el nombre
`WEB-INF/applicationContext.xml` que se encuentra dentro de
`camunda-webapp-SERVER-standalone-VERSION.war`. Modifica la siguiente
sección con los valores de configuración apropiados para tu base de datos.

```xml
<bean id="dataSource" class="org.springframework.jdbc.datasource.TransactionAwareDataSourceProxy">
  <property name="targetDataSource">
    <bean class="org.apache.commons.dbcp.BasicDataSource">
      <property name="driverClassName" value="org.h2.Driver" />
      <property name="url" value="jdbc:h2:./camunda-h2-dbs/process-engine;MVCC=TRUE;TRACE_LEVEL_FILE=0;DB_CLOSE_ON_EXIT=FALSE" />
      <property name="username" value="sa" />
      <property name="password" value="" />
    </bean>
  </property>
</bean>
```

<div class="alert alert-warning">
  Si configuras una base de datos diferente, no olvides añadir el correspondiente driver de base de datos al classpath de la aplicación web.
</div>

Como alternativa también puedes configurar un datasource dentro de tu servidor de aplicaciones y hacer la búsqueda de éste desde
la aplicación web.
