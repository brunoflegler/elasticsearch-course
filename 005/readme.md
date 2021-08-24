**Combinando filtros na busca do Elasticsearch**

Antes de continuarmos é importante ler [#004: Adicionando filtro na busca do Elasticsearch](https://github.com/brunoflegler/elasticsearch-course/blob/master/004/readme.md).

Dessa vez vamos aprender como combinar mais de um filtro na sua pesquisa, suponha que queremos filtrar todos os documentos que foram criados no dia 21/08/2021 e que o cliente tenha 49 anos.

```sh
GET customers/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "age": 49
          }
        },
        {
          "term": {
            "createdAt": "2021-08-21"
          }
        }
      ]
    }
  }
}
```

Encontramos 2 clientes com idade igual a 49 anos e que foram criados no mesmo dia.

```json
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 2,
      "relation" : "eq"
    },
    "max_score" : 2.0,
    "hits" : [
      {
        "_index" : "customers",
        "_type" : "_doc",
        "_id" : "4",
        "_score" : 2.0,
        "_source" : {
          "id" : 4,
          "name" : "Mrs. Zachary Schneider",
          "email" : "Ruthie47@yahoo.com",
          "age" : 49,
          "createdAt" : "2021-08-21T03:16:33.102Z"
        }
      },
      {
        "_index" : "customers",
        "_type" : "_doc",
        "_id" : "92",
        "_score" : 2.0,
        "_source" : {
          "id" : 92,
          "name" : "Charlotte Hansen",
          "email" : "Adelbert82@gmail.com",
          "age" : 49,
          "createdAt" : "2021-08-21T03:16:33.126Z"
        }
      }
    ]
  }
}
```

Quando queremos combinar filtros onde **TODOS** os valores deverão estar nos resultados, iremos usar o termo `must`. Ex: Os clientes com 49 anos que não foram criados no dia 21 não serão retornados. O `must` equivale ao famoso `AND` se fizermos uma analogia aos bancos de dados relacionais.

Agora ficou fácil, para filtrar por mais atributos é só adicioná-los dentro do array.

```json
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "campo 1": "valor 1"
          }
        },
        {
          "term": {
            "campo 2": "valor 2"
          }
        },
        {
          "term": {
            "campo 3": "valor 3"
          }
        },
      ]
    }
  }
}
```

Sua vez agora, pratiquem. E te vejo no próximo artigo.