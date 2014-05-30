---

title: 'NoClassDefFoundError: graphiti'
category: 'Resolución de Problemas'

---

<div class="row">
  <div class="col-xs-6 col-sm-6 col-md-3">
    <img data-img-thumb src="ref:asset:/assets/img/modeler/exception-graphiti.png" />
  </div>
  <div class="col-xs-6 col-sm-6 col-md-9">
  	<p>
    	Si obtienes un NoClassDefFoundErrors como el que aparece aquí, significa que Graphiti no ha sido correctamente instalado. Graphiti es un framework usado por <strong>Camunda Modeler</strong>. Este error raramente aparece cuando tienes otros plugins que utilizan diferentes versiones de Graphiti previamente instaladas a Camunda Modeler (el ejemplo más común es tener instalado Activiti Designer).
    </p>
  </div>
</div>

<div class="row">
  <div class="col-xs-6 col-sm-6 col-md-3">
    <img data-img-thumb src="ref:asset:/assets/img/modeler/install-graphiti.png" />
  </div>
  <div class="col-xs-6 col-sm-6 col-md-9">
		<p>		
			Si ocurre tienes dos opciones:
			<ul>
	      <li>Arranca con un Eclipse <em>limpio</em>.</li>
	      <li>Instala manualmente Graphiti como se muestra en el screenshot de la izquierda (ten en cuenta que la versión puede cambiar, actualmente deberías de usar la última versión 0.8.x o comprueba qué versión utiliza <strong>Camunda Modeler</strong> en el site de actualización).</li>
      </ul>
    </p>
  </div>
</div>
