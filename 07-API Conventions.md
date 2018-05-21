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
logstash-2024.03.22`:
 

    # GET /<logstash-{now/d}>/_search
	GET /%3Clogstash-%7Bnow%2Fd%7D%3E/_search 
	{  
		"query": {"match": {"test": "data"}}  
	}

## Common options [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#common-options)
## URL-based access control [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/url-access-control.html#url-access-control)
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTM0MjM1NDUsMTM2MTE1MjQwNywtMTIxMD
Q2MTEyNCwtNzM1MTAzNTM1LC0xOTQ3ODk4MTYwLC0yMDgyNjc5
MzAyXX0=
-->