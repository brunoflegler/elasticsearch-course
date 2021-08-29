**Como definir o mapeamento no Elasticsearch**

O mapeamento defini como cada campo do documento será indexado. Uma vez que não é definido o tipo do campo o Elasticsearch irá definir dinamicamente. Podemos ver o `mapping` criado no nosso índice `customers`.

```sh
GET /customers/_mapping
```

```json
{
  "customers" : {
    "mappings" : {
      "properties" : {
        "age" : {
          "type" : "long"
        },
        "createdAt" : {
          "type" : "date"
        },
        "email" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "id" : {
          "type" : "long"
        },
        "name" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        }
      }
    }
  }
}
```
&nbsp;

Os tipos sugeridos pelo Elasticsearch:

| Campo     | Tipo  | Valor                       | 
| -----     | ----  | -----                       | 
| age       | long  |  18                         |   
| createdAt | date  | "2021-08-21T03:16:33.116Z"  | 
| email     | text  | "Antonia.Will@gmail.com"    | 
| id        | long  | 55                          | 
| name      | text  | "Mrs. Mable Ritchie"        | 

&nbsp;
> ⚠️ **Atenção**:
> 
> Uma vez que o índice é definido não podemos mais alterar o tipo do campo, apenas adicionar um campo novo. Da mesma forma não será mais possível indexar um documento com outro formato. Vamos tentar indexar um documento com o `id` a `abcdef`. 

&nbsp;
``` json
POST /customers/_doc
{
  "id": "abcdef",
  "name": "Brenda H. Barajas",
  "email": "bredahbarajas@hotmail.com",
  "age": 97,
  "createdAt": "2021-08-29T19:16:33.099Z"
}
```

O Elasticsearch indica que não é possível transformar `abcedf` em um valor numérico.

```json
{
  "error" : {
    "root_cause" : [
      {
        "type" : "mapper_parsing_exception",
        "reason" : "failed to parse field [id] of type [long] in document with id 'q64NlHsBLpS_vORe78sd'. Preview of field's value: 'abcdef'"
      }
    ],
    "type" : "mapper_parsing_exception",
    "reason" : "failed to parse field [id] of type [long] in document with id 'q64NlHsBLpS_vORe78sd'. Preview of field's value: 'abcdef'",
    "caused_by" : {
      "type" : "illegal_argument_exception",
      "reason" : "For input string: \"abcdef\""
    }
  },
  "status" : 400
}
```

Vamos deletar o índice `customers` e criar ele novamente definindo o mapping antes.

```
DELETE customers
```

Agora podemos criar o mapping antes de indexar o primeiro documento. 

```json
PUT /customers
{
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

Se olharmos o mapping agora o `id` será do tipo `text` e conseguiríamos indexar com o valor `abcdef`.

O mapeamento é um tema complexo, envolve os tipos do campos e depende de um conhecimento prévio do documento que será indexado para conseguir otimizar as buscas. Com o tempo vamos entender quais tipos são sugeridos para cada tipo de situação.