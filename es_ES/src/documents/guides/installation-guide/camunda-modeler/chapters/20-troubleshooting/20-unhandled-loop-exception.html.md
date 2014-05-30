---

title: 'unhandled loop exception'
category: 'Resolución de Problemas'

---


Si el Modeler se comporta de una manera extraña y aparecen excepciones como [http://stackoverflow.com/questions/84147/org-eclipse-swt-swterror-item-not-added](http://stackoverflow.com/questions/84147/org-eclipse-swt-swterror-item-not-added), por favor intenta lo siguiente para resolverlo:

* Limpia tu Eclipse.
* Arranca tu Eclipse con la opción de línea de comandos `-clean` una vez.
* Dependiendo de tus modelos, puede ser necesario asignar a tu Eclipse más recursos de memoria. Para ello, deberías de añadir las siguientes líneas al fichero eclipse.ini que se encuentra en la misma carpeta que el ejecutable de tu Eclipse.
```
-Xms768m
-Xmx1024m
```

Si estas opciones ya existen, primero elimínalas.


