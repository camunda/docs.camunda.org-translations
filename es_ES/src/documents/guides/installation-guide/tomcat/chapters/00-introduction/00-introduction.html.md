---

title: 'Descripción General'
category: 'Introducción'

---

Este documento te guiará a través de la instalación de Camunda BPM y sus componentes en el servidor de aplicaciones <a href="http://tomcat.apache.org/">Apache Tomcat 7 Server</a>.

<div class="alert alert-info">
  <strong>Leyendo la guía</strong> A lo largo de esta guía utilizaremos un cierto número de variables para identificar nombres comunes de rutas y constantes.
  <code>$CATALINA_HOME</code> apunta al directorio principal donde se encuentra el servidor Tomcat.
  <code>$PLATFORM_VERSION</code> identifica la versión de la plataforma Camunda BPM que se desea instalar o que ya se tiene instalada, ejemplo <code>7.0.0</code>.
  <code>$TOMCAT_DISTRIBUTION</code> representa la distribución de la plataforma Camunda BPM pre-empaquetada descargada para Tomcat, por ejemplo, <code>camunda-bpm-tomcat-$PLATFORM_VERSION.zip</code> o <code>camunda-bpm-tomcat-$PLATFORM_VERSION.tar.gz</code>.
</div>