---

title: 'Introducción'
category: 'Instalación'

---

Este documento te guiará a través de la instalación y configuración the la plataforma Camunda como aplicación web standalone.
La aplicación web Camunda combina [cockpit][] y [tasklist][]. La aplicación web contiene en sí misma e incluye
un [motor de procesos embebido][], el cual se configura dentro de la aplicación. El motor de procesos es configurado mediante
Spring Framework y se ejecutará automáticamente cuando se desplega la aplicación. El motor de procesos debe de ser configurado
para conectarse a una base de datos (ver la sección [configuración de base de datos][]). Por defecto el motor de procesos utilizará 
un servicio de identidad propio, el cual puede ser reemplazado por un LDAP (ver la sección [configuración de LDAP][]).

<div class="alert alert-danger">
  Desde que la aplicación web embebida de Camunda usa un <a href="ref:/guides/user-guide/#introduction-architecture-overview-embedded-process-engine">motor de procesos embebido</a>
  no puede ser instalado en un servidor de aplicaciones que contenga una distribución de Camunda. Las distribuciones de Camunda contienen un servidor de aplicaciones que proporciona un 
  <a href="ref:/guides/user-guide/#introduction-architecture-overview-shared-container-managed-process-engine">un motor de procesos compartido</a>.
</div>


[cockpit]: ref:/guides/user-guide/#cockpit
[tasklist]: ref:/guides/user-guide/#tasklist
[motor de procesos embebido]: ref:/guides/user-guide/#introduction-architecture-overview-embedded-process-engine
[configuración de base de datos]: ref:/guides/installation-guide/standalone/#configuration-database-configuration
[configuración de LDAP]: ref:/guides/installation-guide/standalone/#configuration-ldap-configuration