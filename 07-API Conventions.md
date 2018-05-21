# API Conventions [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/api-conventions.html#api-conventions)
Обращение к Elasticsearch API идёт в формате JSON поверх HTTP.    
## Multiple Indices [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/multi-index.html#multi-index)
Большинство API, имеющие параметр index, также могут работать и с несколькими индексами одновременно. Для этого указывается _all вместо конкретного индекса; список индексов через запятую (index1, index2); шаблоны со звёздочкой и исключения с минусом (index*,-index5). 
Все мультииндексовые API поддерживают следующие query-string-параметры:
- ignore_unavailable — проигнорировать несуществующие индексы (не выдавать ошибку)
- allow_no_indices — проигнорировать отсутствие индексов, соответствующих шаблону со звездочкой либо значению _all.
- expand_wildcards — контролирует поведение шаблонов со звёздочкой: open — совпадение с открытыми индексами, closed — с закрытыми, all — со всеми, none — совпадение отключено.

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
В условиях наподобие gt и lt можно использовать выражения такого плана: now+1h (через час), now/d (начало сегодняшнего дня 00:00). Подробнее в [оригинале](https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#date-math).  
#### Фильтрация ответа
Query-string-параметр позволяет указать поля, которые нужно вернуть в ответе, например:

    GET /_search?q=*&filter_path=took,hits.hits._id,hits.hits._score
При указании полей можно использовать шаблоны со звёздочкой и даже с двумя звёздочками (когда путь к полю неизвестен/произвольный). Также можно исключить некоторые поля поставив минус перед именем. Подробности [тут](https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#common-options-response-filtering). 

## URL-based access control [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/url-access-control.html#url-access-control)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzY2NTIzNDE4LDMwODE1NjAxMiwxMzYxMT
UyNDA3LC0xMjEwNDYxMTI0LC03MzUxMDM1MzUsLTE5NDc4OTgx
NjAsLTIwODI2NzkzMDJdfQ==
-->