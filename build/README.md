# Building the translations

This folder hosts a simple [apache ant][apache-ant] based build system for the translations.

If you have setup your environment as outlined in the *prerequisites*, you can build a translation by
typing

```shell
ant -Dtranslations.locale=es_ES
```

If the build is successful, the generated files can be found in 

```shell
./docs/site/out
```

## Prerequisites

1. Make sure you have installed a [Java Runtime Environment (JDK) version 1.6+][java]
2. Make sure [apache ant][apache-ant] is installed and in your `PATH`.
3. Make sure [git][git] is installed and in your `PATH`.
4. Make sure [Node.js][node] is installed and in your `PATH`.

[java]: http://www.oracle.com/technetwork/java/javase/downloads/
[apache-ant]: http://ant.apache.org/
[git]: http://git-scm.com/
[node]: http://nodejs.org/


