# Getting Started [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started.html#getting-started)
Elasticsearch — масштабируемый полнотекстовый поисково-аналитический движок. ES работает практически в реальном времени. 
## Basic Concepts  [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_basic_concepts.html#_basic_concepts)
**Near Realtime (NRT)** — близко к реальному времени; небольшая задержка (около секунды) с момента индексации документа до момента, когда он будет доступен для поиска.  
**Cluster** — кластер — совокупность одного или нескольких узлов (Node). У кластера есть уникальное имя, по умолчанию "elasticsearch".
**Node** — узел — один сервер, являющийся частью кластера. Учавствует в хранении, индексации и поиске.
**Index** — коллекция документов с одинаковыми/схожими характеристиками. Например, вы можете создать один индекс для хранения пользователей, другой индекс для продуктов и т. д.
**Type** — тип — использовался как подкатегория для хранения нескольких типов документов в одном индексе, однако на данный момент нельзя создать более одного типа, а в будущем от этой концепции планируют отказаться. 
**Document** — базовая единица информации, которая может быть проиндексирована. Формат — JSON. Документу должен быть назначен тип внутри индекса. 
**Shards & Replicas** — ES предоставляет возможность подразделить ваш индекс на несколько частей, называемых Shards ("осколками"). При создании индекса можно указать количество Shards. Shards могут распологаться на разных узлах, тем самым обеспечивая горизонтальное масштабирование. Replica — копия данных, для отказоустойчивости и увеличения производительности. 
## Installation [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_installation.html#_installation)
Для установки требуется Java 8. Подробнее об установке можно прочитать [здесь](https://www.elastic.co/guide/en/elasticsearch/reference/current/_installation.html).
## Exploring Your Cluster [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_installation.html#_installation)
Взаимодействие с ES происходит через REST API. Следующие команды удобно выполнять в [консоли Kibana](https://www.elastic.co/guide/en/kibana/6.2/console-kibana.html). 


    GET /_cat/health?v

 
## Modifying Your Data [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_modifying_your_data.html#_modifying_your_data)
## Exploring Your Data [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_exploring_your_data.html#_exploring_your_data)
## Conclusion [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_conclusion.html#_conclusion)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MTk1NzMxNiwxNDE3NzA4NzM1LDEzNz
g0MjExMV19
-->