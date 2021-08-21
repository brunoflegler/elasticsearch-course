**Adicionando filtro na busca do Elasticsearch**

Antes de continuarmos não esqueça de ler [#003:Buscando todos os documentos no Elasticsearch](https://github.com/brunoflegler/elasticsearch-course/blob/master/003/readme.md).

**Bônus**

Vamos instalar e iniciar o Kibana. O Kibana é uma interface de usuário gratuita e aberta para você visualizar seus documentos no Elasticsearch.


1. Edite o arquivo docker-compose.yml e atualize:

```
version: '3.3'

services:
  cluster-elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.14.0
    container_name: cluster-elasticsearch
    ports:
      - "9202:9200"
    environment:
      - discovery.type=single-node

  kibana:
    image: docker.elastic.co/kibana/kibana:7.14.0
    container_name: kibana
    ports: 
      - "5601:5601"
    environment: 
      - ELASTICSEARCH_HOSTS=http://cluster-elasticsearch:9200
```

Acesse em http://localhost:5601, no menu principal clique em **Dev Tools** -> **Console**

![Kibana](https://github.com/brunoflegler/elasticsearch-course/blob/master/004/kibana.png)


Antes de fazermos a busca, vamos adicionar mais documentos. Podemos usar `_bulk` que cria N documentos em uma única requisição. Para facilitar esse processo utilize o arquivo o [100 clientes aleatórios](github.com/brunoflegler/elasticsearch-course/blob/master/003/_bulk.txt).

```json
POST _bulk
{"index":{"_index":"customers","_id":1}}
{"id":1,"name":"Mrs. Leonard Roob","email":"Arnoldo22@hotmail.com","age":97,"createdAt":"2021-08-21T03:16:33.099Z"}
{"index":{"_index":"customers","_id":2}}
```

*...five minutes later*

Imagine que gostaríamos de filtrar os clientes por idade. Sendo assim podemos usar `term query`. Retornará todos documentos exatamente igual ao valor informado.


```sh
GET customers/_search
{
  "query": {
    "term": {
      "age": 18
    }
  }
}
```

Encontramos 4 clientes com idade igual a 18 anos.

```json
{
  "took" : 2,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "customers",
        "_type" : "_doc",
        "_id" : "55",
        "_score" : 1.0,
        "_source" : {
          "id" : 55,
          "name" : "Mrs. Mable Ritchie",
          "email" : "Antonia.Will@gmail.com",
          "age" : 18,
          "createdAt" : "2021-08-21T03:16:33.116Z"
        }
      },
      {
        "_index" : "customers",
        "_type" : "_doc",
        "_id" : "78",
        "_score" : 1.0,
        "_source" : {
          "id" : 78,
          "name" : "Lauren Howell",
          "email" : "Mathilde.Waters@hotmail.com",
          "age" : 18,
          "createdAt" : "2021-08-21T03:16:33.123Z"
        }
      },
      {
        "_index" : "customers",
        "_type" : "_doc",
        "_id" : "82",
        "_score" : 1.0,
        "_source" : {
          "id" : 82,
          "name" : "Phillip Schulist",
          "email" : "Annamae99@yahoo.com",
          "age" : 18,
          "createdAt" : "2021-08-21T03:16:33.124Z"
        }
      },
      {
        "_index" : "customers",
        "_type" : "_doc",
        "_id" : "89",
        "_score" : 1.0,
        "_source" : {
          "id" : 89,
          "name" : "Warren Crooks",
          "email" : "Madilyn22@gmail.com",
          "age" : 18,
          "createdAt" : "2021-08-21T03:16:33.126Z"
        }
      }
    ]
  }
}
```

> ⚠️ **Atenção**:
> 
> Por padrão o Elasticsearch utiliza `standard analyzer` para campos do tipo `text`. Ex: O nome `Mrs. Mable Ritchie`é indexado as palavras separadamente sem pontuação como um array [mrs, mable, ritchie]
>
>Usando o `term` não será possível filtrar o nome completo. Nos próximos artigos iremos falar sobre esses casos.
