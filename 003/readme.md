  **Buscando todos os documentos no Elasticsearch**

No artigo anterior vimos como indexar os documentos no elasticsearch e hoje vamos aprender a retornar todos os registros indexados. Se você ainda não leu segue a referência [#002: Indexando o primeiro documento](https://github.com/brunoflegler/elasticsearch-course/blob/master/002/readme.md).

É importante indexar mais de um documento para conseguir observar os resultados da pesquisa.

*...five minutes later*

Use o `curl` para pesquisar os documentos. O intuito nesse momento não é obter performance na query é apenas entender como fazer uma busca total sem filtro e identificar os atríbutos principais.

```sh
$ curl -X GET "http://localhost:9202/customers/_search?pretty" -H "Content-Type: application/json" -d '
{
  "query": {
    "match_all": {}
  }
}'
```

```json
{
  "took": 524,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 3,
      "relation": "eq"
    },
    "max_score": 1.0,
    "hits": [
      {
        "_index": "customers",
        "_type": "_doc",
        "_id": "1",
        "_score": 1.0,
        "_source": {
          "id": 1,
          "name": "Christopher M. Bass",
          "age": 45,
          "createdAt": "2021-08-17T02: 36: 39.104"
        }
      },
      {
        "_index": "customers",
        "_type": "_doc",
        "_id": "2",
        "_score": 1.0,
        "_source": {
          "id": 2,
          "name": "Alberto D. Wilson",
          "age": 77,
          "createdAt": "2021-08-18T17: 36: 39.104"
        }
      },
      {
        "_index": "customers",
        "_type": "_doc",
        "_id": "3",
        "_score": 1.0,
        "_source": {
          "id": 3,
          "name": "Darla G. Barney",
          "age": 19,
          "createdAt": "2021-08-18T17: 40: 39.104"
        }
      }
    ]
  }
}
```

O resultado retorna dentre eles o atributo `total` onde identificamos o total de documentos encontrados de acordo com o filtro. Nesse exemplo foram retornados 3 documentos.

```json
"total": {
  "value": 3,
  "relation": "eq"
},
```

Por `default` o cluster irá retornar 10 documentos, mesmo que o total seja maior. Para aumentar esse valor precisamos adicionar a propriedade no corpo da requisição, mas por questão de performance o valor limite é 10000.

```sh
$ curl -X GET "http://localhost:9202/customers/_search?pretty" -H "Content-Type: application/json" -d '
{
  "query": {
    "match_all": {}
  },
  "size": 100,
}'
```

Nos próximos artigos iremos entender mais sobre atributos retornados na pesquisa, além de entender como filtrar por um atributo simples. Até lá pratiquem!