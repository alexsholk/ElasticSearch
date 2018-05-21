# API Conventions [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/api-conventions.html#api-conventions)
Обращение к Elasticsearch API идёт в формате JSON поверх HTTP.    
## Multiple Indices [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/multi-index.html#multi-index)
Большинство API, имеющие параметр index, также могут работать и с несколькими индексами одновременно. Для этого указывается `_all` вместо конкретного индекса; список индексов через запятую (`index1, index2`); шаблоны со звёздочкой и исключения с минусом (`index*,-index5`). 
Все мультииндексовые API поддерживают следующие query-string параметры:
- `ignore_unavailable` — проигнорировать несуществующие индексы (не выдавать ошибку)
- `allow_no_indices` — проигнорировать отсутствие индексов, соответствующих шаблону со звездочкой либо значению `_all`.
- `expand_wildcards` — контролирует поведение шаблонов со звёздочкой: `open` — совпадение с открытыми индексами, `closed` — с закрытыми, `all` — со всеми, `none` — совпадение отключено.

## Date math support in index names [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/date-math-index-names.html#date-math-index-names)
Эта фича позволяет указать определенные индексы, в имени которых присутствует дата, с помощью специального выражения следующего формата:

    <static_name{date_math_expr{date_format|time_zone}}>

 Например, поиск по сегодняшнему индексу с именем `  
logstash-2018.05.21` выглядит следующим образом:
 

    # GET /<logstash-{now/d}>/_search
	GET /%3Clogstash-%7Bnow%2Fd%7D%3E/_search 
	{  
		"query": {"match": {"test": "data"}}  
	}
Остальные опции можно посмотреть в оригинальной [статье](https://www.elastic.co/guide/en/elasticsearch/reference/current/date-math-index-names.html#date-math-index-names). 

## Common options [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#common-options)
#### Общие опции, применимые ко всем REST API:
- ?pretty=true — форматированный JSON в ответе
- ?format=yaml — ответ в формате YAML
- ?human=true — возврат значений даты, количества байт в удобном формате (1h, 1kb), помимо значений в миллисекундах и байтах. По умолчанию выключен. 
#### Даты и математика
В условиях наподобие `gt` и `lt` можно использовать выражения такого плана: `now+1h` (через час), `now/d` (начало сегодняшнего дня 00:00). Подробнее в [оригинале](https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#date-math).  
#### Фильтрация ответа
Query-string параметр `filter_path` позволяет указать поля, которые нужно вернуть в ответе, например:

    GET /_search?q=*&filter_path=took,hits.hits._id,hits.hits._score
При указании полей можно использовать шаблоны со звёздочкой и даже с двумя звёздочками (когда путь к полю неизвестен/произвольный). Также можно исключить некоторые поля поставив минус перед именем. Подробности [тут](https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#common-options-response-filtering). 
Однако если нужно отфильтровать поля из _source, нужно воспользоваться комбинацией параметров `filter_path` и `_source`:

    GET /_search?filter_path=hits.hits._source&_source=title

#### Flat settings
Query-string параметр `flat_settings` уменьшает вложенность массивов в ответе касательно настроек:

    GET twitter/_settings?flat_settings=true

#### Fuzziness 
Параметр `fuzziness` позволяет управлять нечётким совпадением; может принимать значения 0, 1, 2, являющиеся расстоянием Левенштейна (количество правок). 
#### Трассировка ошибок
Для включения трассировки ошибок в ответе сервера добавьте query-string параметр `error_trace=true`
#### Передача тела запроса по URL
В случаях, когда тело запроса невозможно передать в методах, отличных от POST, его можно пережать как query-string параметр `source`. Также необходимо указать параметр `source_content_type=application/json`. 

## URL-based access control [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/url-access-control.html#url-access-control)
Некоторые пользователи имеют доступ к ES API через прокси, контролирующий доступ по URL к тем или иным возможностям. Для некоторых API возможно указать индексы в теле запроса, что затрудняет контроль доступа на основе URL. Чтобы запретить это следует изменить (добавить) следующий параметр в `elasticsearch.yml`:

    rest.action.multi.allow_explicit_index: false

В таком случае ES будет отклонять запросы с указанием индекса в теле запроса. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY2OTUxOTAxNywxNTk2NzYyMTUwLDEyMD
YzMzY0NDMsMTEyMTYzODY4MiwtMjQxOTI4MzUwLDMwODE1NjAx
MiwxMzYxMTUyNDA3LC0xMjEwNDYxMTI0LC03MzUxMDM1MzUsLT
E5NDc4OTgxNjAsLTIwODI2NzkzMDJdfQ==
-->