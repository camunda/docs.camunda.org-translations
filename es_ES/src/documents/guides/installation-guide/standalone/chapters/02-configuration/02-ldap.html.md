---

title: 'Configuración LDAP'
category: 'Configuración'

---

Inicialmente la aplicación web standalone está configurada para usar un servicio de identidades propio.
Si quieres utilizar LDAP, debes activar el servicio de identidades LDAP de Camunda. El fichero
`WEB-INF/applicationContext.xml` contienen una configuración ejemplo la cual está desactivada. Para
poder activarla descomenta las líneas que ves abajo:

```xml
<bean id="processEngineConfiguration" class="org.camunda.bpm.engine.spring.SpringProcessEngineConfiguration">
  <!-- ... -->
  <property name="processEnginePlugins">
    <list>
      <!-- Uncomment the following next two lines to activate LDAP -->
      <!--<ref bean="ldapIdentityProviderPlugin" />-->
      <!--<ref bean="administratorAuthorizationPlugin" />-->
    </list>
  </property>
</bean>
```

Para configurar el servicio LDAP, por favor, modifica correctamente los valores del bean con el nombre `ldapIdentityProviderPlugin` tal
y como se describe en la [guía de usuario](ref:/guides/user-guide/#process-engine-identity-service-configuration-properties-of-the-ldap-plugin).
Tampoco olvides configurar el plugin de autorizaciones (ver la [documentación](ref:/guides/user-guide/#process-engine-authorization-service)).
