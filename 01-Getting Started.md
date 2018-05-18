# Getting Started [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started.html#getting-started)
**Elasticsearch** — масштабируемый полнотекстовый поисково-аналитический движок. ES работает практически в реальном времени. 
## Basic Concepts  [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_basic_concepts.html#_basic_concepts)
**Near Realtime (NRT)** — близко к реальному времени; небольшая задержка (около секунды) с момента индексации документа до момента, когда он будет доступен для поиска.  

**Cluster (кластер)** — совокупность одного или нескольких узлов (Node). У кластера есть уникальное имя, по умолчанию "elasticsearch". 

**Node (узел)** — один сервер, являющийся частью кластера. Учавствует в хранении, индексации и поиске.

**Index** — коллекция документов с одинаковыми/схожими характеристиками. Например, вы можете создать один индекс для хранения пользователей, другой индекс для продуктов и т. д.

**Type (тип)** — использовался как подкатегория для хранения нескольких типов документов в одном индексе, однако на данный момент нельзя создать более одного типа, а в будущем от этой концепции планируют отказаться. 

**Document** — базовая единица информации, которая может быть проиндексирована. Формат — JSON. Документу должен быть назначен тип внутри индекса. 

**Shards & Replicas** — ES предоставляет возможность подразделить ваш индекс на несколько частей, называемых Shards ("осколками"). При создании индекса можно указать количество Shards. Shards могут распологаться на разных узлах, тем самым обеспечивая горизонтальное масштабирование. Replica — копия данных, для отказоустойчивости и увеличения производительности. 
## Installation [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_installation.html#_installation)
Для установки требуется Java 8. Подробнее об установке можно прочитать [здесь](https://www.elastic.co/guide/en/elasticsearch/reference/current/_installation.html).
## Exploring Your Cluster [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_installation.html#_installation)
Взаимодействие с ES происходит через REST API. Следующие команды удобно выполнять в [консоли Kibana](https://www.elastic.co/guide/en/kibana/6.2/console-kibana.html). 

#### Информация о кластере, узлах и индексах:
    GET /_cat/health?v
    GET /_cat/nodes?v
    GET /_cat/indices?v
#### Создание индекса 

    PUT /customer?pretty
#### Индексирование документа
Создание документа в индексе *customer* с типом *_doc* и *id=1*:

    PUT /customer/_doc/1?pretty 
    {  
        "name":  "John Doe"  
    }

В ответе будет содержаться информация о выполнении данного запроса.
#### Получение документа 

    GET /customer/_doc/1?pretty
В поле *_source* ответа хранится исходный документ. 
#### Удаление индекса 

    DELETE /customer?pretty

## Modifying Your Data [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_modifying_your_data.html#_modifying_your_data)

#### Изменение документа 

    POST /customer/_doc/1/_update?pretty 
    {  
        "doc": {"name": "John Doe", "age": 20}  
    }
Джонни постарел на 5 лет:

    POST /customer/_doc/1/_update?pretty 
    {  
        "script": "ctx._source.age += 5"
    }
(*ctx._source* ссылается на текущий документ)
#### Удаление документа 

    DELETE /customer/_doc/2?pretty

#### Пакетные операции 
Индексация двух документов:

    POST /customer/_doc/_bulk?pretty 
    {"index":{"_id":"1"}}  
    {"name": "John Doe"}
    {"index":{"_id":"2"}}  
    {"name": "Jane Doe"}
Обновление первого и удаление второго документа:

    POST /customer/_doc/_bulk?pretty 
    {"update": {"_id":"1"}}  
    {"doc": {"name": "John Doe becomes Jane Doe"}}  
    {"delete": {"_id":"2"}}
Bulk API не прерывает выполнение операций, если какая-нибудь из них не выполнилась. В ответе будут содержаться статусы всех операций в исходном порядке. 

## Exploring Your Data [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_exploring_your_data.html#_exploring_your_data)
Тестовые данные можно сгенерировать с помощью этого [сервиса](https://www.json-generator.com/) или скачать готовый JSON [здесь](https://github.com/elastic/elasticsearch/blob/master/docs/src/test/resources/accounts.json?raw=true).
#### Загрузка данных из файла

    curl -H "Content-Type: application/json"  
    -XPOST "localhost:9200/bank/_doc/_bulk?pretty&refresh"  
    --data-binary "@accounts.json"
    
#### Простой поиск 
Поиск может производиться путём передачи параметров в *REST request URI* либо в *REST request body*. Первый вариант:

    GET /bank/_search?q=*&sort=account_number:asc&pretty

Вернёт все документы (*q=\**), отсортированные по *account_number*. В ответе также присутствуют следующие поля:

 - `took` — время выполнения запроса, мс
 - `timed_out` — время вышло или нет
 - `_shards` — сколько Shards участвовало в поиске и т. п.
 - `hits` — результаты поиска
 - `hits.total` — количество найденных документов
 - `hits.hits` — массив найденных документов 
 - `hits.sort` — сортировка 
 -  

## Conclusion [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_conclusion.html#_conclusion)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ2ODkwNzkxNyw3NjQwNDQ1ODUsMTI2Mj
c1MjM0NSwtNzEzMzA2OTgzLDEzMzgwMDM1MzgsMjA3MzkzNjc3
OCwxNDE3NzA4NzM1LDEzNzg0MjExMV19
-->