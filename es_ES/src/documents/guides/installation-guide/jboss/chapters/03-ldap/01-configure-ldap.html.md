---

title: 'Configurando LDAP para Camunda Cockpit y Tasklist'
category: 'Configuración LDAP'

---


Para poder configurar un LDAP para la distribución de Servidor de Aplicaciones JBoss, deberías de seguir los siguientes pasos:

<strong>Ajuste de la Configuración del Motor de Procesos</strong>
Modifica el fichero `standalone.xml` (o `domain.xml`) proporcionado por el Servidor de Aplicaciones JBoss y añade el [LDAP Identity Provider Plugin](/guides/user-guide/#process-engine-identity-service-the-ldap-identity-service) y el [Administrator Authorization Plugin](/guides/user-guide/#process-engine-authorization-service-the-administrator-authorization-plugin).

```xml
<subsystem xmlns="urn:org.camunda.bpm.jboss:1.1">
  <process-engines>
    <process-engine name="default" default="true"> ...
      <properties>...</properties>
      <plugins>
        <plugin>
          <class>org.camunda.bpm.identity.impl.ldap.plugin.LdapIdentityProviderPlugin</class>
          <properties>

            <property name="serverUrl">ldap://localhost:4334/</property>
            <property name="managerDn">uid=jonny,ou=office-berlin,o=camunda,c=org</property>
            <property name="managerPassword">s3cr3t</property>

            <property name="baseDn">o=camunda,c=org</property>

            <property name="userSearchBase">ou=employees</property>
            <property name="userSearchFilter">(objectclass=person)</property>

            <property name="userIdAttribute">uid</property>
            <property name="userFirstnameAttribute">cn</property>
            <property name="userLastnameAttribute">sn</property>
            <property name="userEmailAttribute">mail</property>
            <property name="userPasswordAttribute">userpassword</property>

            <property name="groupSearchBase">ou=roles</property>
            <property name="groupSearchFilter">(objectclass=groupOfNames)</property>
            <property name="groupIdAttribute">ou</property>
            <property name="groupNameAttribute">cn</property>

            <property name="groupMemberAttribute">member</property>

          </properties>
        </plugin>
        <plugin>
          <class>org.camunda.bpm.engine.impl.plugin.AdministratorAuthorizationPlugin</class>
          <properties>
            <property name="administratorUserName">admin</property>
          </properties>
        </plugin>
      </plugins>
    </process-engine>
  </process-engines> ...
</subsystem>
```


La propiedad `administratorUserName` debe de contener el identificador de usuario de LDAP que quieres que conceda las autorizaciones de administración. Puedes usar este usuario para logearte en la aplicación web y conceder autorizaciones a usuarios adicionales.

Ver la guía de usuario para obtener una documentación completa de [LDAP Identity Provider Plugin](ref:/guides/user-guide/#process-engine-identity-service-the-ldap-identity-service) y de [Administrator Authorization Plugin](ref:/guides/user-guide/#process-engine-authorization-service-the-administrator-authorization-plugin).