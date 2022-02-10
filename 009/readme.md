**Como definir um template para um índice**

No artigo anterior [#008: Como definir o mapeamento no Elasticsearch](https://dev.to/brunoflegler/008-como-definir-o-mapeamento-no-elasticsearch-13c0) definimos um mapping diretamente no índice, porém podemos criar um template e criar o índice a partir dele. Essa estratégia é útil quando criamos índices temporais.


```json
PUT /_template/customers
{
  "index_patterns": [
    "customers_*"
  ],
  "aliases": {
    "customers": {}
  },
  "mappings": {
    "properties": {
      "id": {
        "type": "text"
      },
      "age": {
        "type": "integer"
      },
      "email": {
        "type": "text"
      },
      "name": {
        "type": "text"
      },
      "createdAt": {
        "type": "date"
      }
    }
  }
}
```

O campo `index_patterns` será usado para corresponder aos nomes dos índices durante a criação. Exemplo: `customers_2022_02`, `customers_2022_03`, uma vez que o nome do índice corresponde ao pattern `customer_*` o índice é criado com o mapping definido no template.

```json
PUT /customers_2022_02
```

```json
GET /customers_2022_02/_mapping
{
  "customers_2022_02" : {
    "mappings" : {
      "properties" : {
        "age" : {
          "type" : "integer"
        },
        "createdAt" : {
          "type" : "date"
        },
        "email" : {
          "type" : "text"
        },
        "id" : {
          "type" : "text"
        },
        "name" : {
          "type" : "text"
        }
      }
    }
  }
}

```

O campo `aliases` também é bem útil quando queremos buscar em todos índices criados a partir do template. Exemplo: `customers_2022_02`, `customers_2022_03`, podemos pesquisar em ambos índices utilizando apenas o nome `customers`.

```json
GET /customers/_search
{
  "query": {
    "match_all": {}
  }
}
```

Sua vez de praticar, te vejo em breve.