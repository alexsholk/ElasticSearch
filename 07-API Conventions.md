# API Conventions [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/api-conventions.html#api-conventions)
Обращение к Elasticsearch API идёт в формате JSON поверх HTTP.    
## Multiple Indices [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/multi-index.html#multi-index)
Большинство API, имеющие параметр index, также могут работать и с несколькими индексами одновременно. Для этого указывается _all вместо конкретного индекса; список индексов через запятую (index1, index2); шаблоны со звёздочкой и исключения с минусом (index*,-index5). 
Все мультииндексовые API поддерживают следующие query-string-параметры:
- ignore_unavailable — проигнорировать несуществующие индексы (не выдавать ошибку)
- allow_no_indices — проигнорировать отсутствие индексов, соответствующих шаблону со звездочкой либо значению _all.
- expand_wildcards — контролирует поведение шаблонов со звёздочкой: open — совпадение с открытыми индексами, closed — с закрытыми, all — со всеми, none — совпадение отключено.

## Date math support in index names [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/date-math-index-names.html#date-math-index-names)
Эта фича поз

## Common options [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#common-options)
## URL-based access control [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/url-access-control.html#url-access-control)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc0MTg3NTIwNywxMzYxMTUyNDA3LC0xMj
EwNDYxMTI0LC03MzUxMDM1MzUsLTE5NDc4OTgxNjAsLTIwODI2
NzkzMDJdfQ==
-->