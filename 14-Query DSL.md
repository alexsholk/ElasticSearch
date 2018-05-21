# Query DSL [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html#query-dsl)
Query DSL — синтаксис запросов, основанный на JSON, представляющий собой древовидную структуру, состоящую из двух типов узлов:

 - Простые (Leaf) — запрос, направленный на поиск определенного значения в определенном поле (match, term, range). Могут использоваться сами по себе. 
 - Составные (Compound) — содержит простые или составные запросы, для логического объединения (bool) либо для изменения поведения (constant_score). 
## Query and filter context [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html#query-filter-context)
Поведение условий запроса (query clause) зависит от того, в каком контексте они используются. 
#### Query-контекст 
В этом контексте условие запроса отвечает за то, *насколько хорошо* документ соответствует нашим требованиям. Помимо отбора подходящих документов также расчитывается и параметр _score, показывающий, 

## Match All Query [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-all-query.html#query-dsl-match-all-query)
## Full text queries [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/full-text-queries.html#full-text-queries)
## Term level queries [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/term-level-queries.html#term-level-queries)
## Compound queries [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/compound-queries.html#compound-queries)
## Joining queries [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/joining-queries.html#joining-queries)
## Geo queries [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/geo-queries.html#geo-queries)
## Specialized queries [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/specialized-queries.html#specialized-queries)
## Span queries [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/span-queries.html#span-queries)
## Minimum Should Match [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-minimum-should-match.html#query-dsl-minimum-should-match)
## Multi Term Query Rewrite [#](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-multi-term-rewrite.html#query-dsl-multi-term-rewrite)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY0NzQ3ODE0LDEyNjAwODMzOTBdfQ==
-->