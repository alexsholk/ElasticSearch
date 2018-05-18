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
 - `max_score` — максимальное значение поля *_score* (релевантность)
 - `_shards` — сколько Shards участвовало в поиске и т. п.
 - `hits` — результаты поиска
 - `hits.total` — количество найденных документов (всего, без учёта ограничения *size*)
 - `hits.hits` — массив найденных документов 
 - `hits.sort` — сортировка 
 - `hits._score` — релевантность

Аналогичный запрос, но методом *REST request body*:

    GET /bank/_search 
    {  
        "query": {"match_all": {}},  
        "sort": [{"account_number": "asc"}] 
    }

#### Введение в Query Language (Query DSL)
Выборка 10 документов начиная с десятого, с обратной сортировкой по балансу: 

    GET /bank/_search 
    {  
        "query": {"match_all": {}}, 
        "sort": {"balance": {"order": "desc"}}, 
        "from": 10, 
        "size": 10  
    }
#### Выборка указанных полей
Укажите в запросе нужные поля:

    "_source": ["account_number", "balance"]

#### Поиск по точному значению поля

    "query": {"match": {"account_number": 20}}
    
#### Поиск по фразе
Поиск документов, в адресе которых присутствует "mill":

    "query": {"match": {"address": "mill"}}
Поиск документов,  в адресе которых присутствует "mill" или "lane":

    "query": {"match": {"address": "mill lane"}}
    
Поиск документов, в адресе которых присутствует фраза "mill lane":

    "query": {"match_phrase": {"address": "mill lane"}}

#### Bool query
Позволяет объединять условия используя булеву логику. Аналог **AND** в SQL запросах:

    GET /bank/_search 
    {  
	    "query": {  
		    "bool": {  
			    "must": [  
				    {"match": {"address": "mill"}},
				    {"match": {"address": "lane"}}  
				]
			}
		}
	}
Если **must** заменить на **should** получится аналог **OR**, то есть достаточно, чтобы выполнялось лишь одно условие, чтобы документы попали в результат. 
**must_not** — все условия должны НЕ выполняться, чтобы документы попали в результат. 
Внутри **bool** можно использовать секции must, should, must_not одновременно. Также можно создавать вложенные bool-запросы:

    GET products_v6/product/_search
    {
      "query": {
        "bool": {
          "must": [
            {"match": {"name": "dog"}},
            {"bool": {
              "should": [
                {"match": {"artist.name": "John"}},
                {"match": {"artist.name": "Octavian"}}
              ]
            }}
          ]
        }     
      },
      "_source": ["name", "artist.name"], 
      "sort": {"_id": {"order": "desc"}},
      "size": 5
    }
Найдёт картины с собаками, нарисованные Джоном или Октавианом. Следует отметить, что поле *_id* строковое и сортировка по нему производится посимвольно, что может не отвечать реальным потребностям. 
#### Фильтрация
  `_score` — числовой показателей релевантности документа (иными словами соответствие документа поисковому запросу). **Bool query** позволяет использовать помимо **must**, **should** и **must_not** условий, также и условие **filter**. Его особенность в том, что его использование не влияет на вычисление `_score`. Пример использования:
  

    GET /bank/_search 
    {  
	    "query": {
		    "bool": {
			    "must": {"match_all": {}}, 
			    "filter": {
				    "range": {
					    "balance": {"gte": 20000, "lte": 30000}
					}  
				}  
			} 
		}  
	}  
	
Типичное применение **range** — фильтрация по диапазону дат или числовых значений. 
#### Агрегация
Агрегация позволяет группировать данные и строить статистику по ним. Похоже на GROUP BY в SQL-запросах. ES позволяет производить поиск и возвращать как документы (hits), так и агрегированные данные (aggregations) одновременно (в отличие от MySQL). Пример:

    GET /bank/_search 
    {
        "size": 0,  
	    "aggs": {  
		    "group_by_state": {  
			    "terms": {"field": "state.keyword"}  
			}
		}
	}


## Conclusion [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/_conclusion.html#_conclusion)


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5NDkzNDUzNTYsNTk2MzAyNDk3LDE5ND
U0MjIzNyw3MzA2NTU0MDYsMTQwMDQ4Mjk1MCwtMTI5NjU0MjE1
OSwxNjUzNDA4NzEyLDQ3NTE0NTcyMiw5MzMwMjkyNTYsMTA0Nj
E5MTA4MSwtMzA5NjAwMzk3LDE3Njc2OTI1MjYsLTE0MTc4ODc3
NCw3NjQwNDQ1ODUsMTI2Mjc1MjM0NSwtNzEzMzA2OTgzLDEzMz
gwMDM1MzgsMjA3MzkzNjc3OCwxNDE3NzA4NzM1LDEzNzg0MjEx
MV19
-->