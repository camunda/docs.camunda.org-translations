---

title: 'Descripción General'
category: 'Introducción'

---

Este documento te guiará a través de la instalación de Camunda BPM y sus componentes en el servidor de aplicaciones <a href="http://glassfish.java.net/">Glassfish 3.1</a>.

<div class="alert alert-info">
  <strong>Leyendo la guía</strong> A lo largo de esta guía utilizaremos un cierto número de variables para identificar nombres comunes de rutas y constantes.
  <code>$GLASSFISH_HOME</code> apunta al directorio principal donde se encuentra el servidor de aplicaciones (típicamente <code>glassfish3/glassfish</code> cuando lo extraemos de una distribución Glassfish).
  <code>$PLATFORM_VERSION</code> identifica la versión de la plataforma Camunda BPM que se desea instalar o que ya se tiene instalada, ejemplo <code>7.0.0</code>. <code>$GLASSFISH_DISTRIBUTION</code> representa la distribiución pre-empaquetada de Camunda para Glassfish, por ejemplo <code>camunda-bpm-glassfish-$PLATFORM_VERSION.zip</code> o <code>camunda-bpm-glassfish-$PLATFORM_VERSION.tar.gz</code>.
</div>