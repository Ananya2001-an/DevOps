# HELM

Packages yaml files and distributes them in public and private repositories.

**Helm charts** are used to bundle Yaml files and we can easily push them to Helm repository or even download someone else’s charts for our own use.

Helm chart directory:

* Chart.yaml -> metadata about the chart
* values.yaml -> values to replace in template&#x20;
* charts -> has dependencies on other charts
* templates -> template files

HELM has 2 main benefits:

1. Can be used as a templating engine for creating a single yaml file template for various micro services that are supposed to run inside our cluster and have almost similar values except few.&#x20;
2. Can create our own chart with all yaml files for app’s components and use it in different environements directly like in development, staging and production. This reduces load of writing those configurations again and again.
3. Release management
