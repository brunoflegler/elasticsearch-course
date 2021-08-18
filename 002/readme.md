  **Indexando o primeiro documento no Elasticsearch**

Certifique-se que o cluster foi iniciado. Como podemos ver nesse artigo [#001: Iniciando no Elasticsearch](https://github.com/brunoflegler/elasticsearch-course/blob/master/001/readme.md).

Os documentos são indexados no elasticsearch no formato como JSON. Ex:

```json
{
  "id": 1,
  "name": "Christopher M. Bass",
  "age": 45,
  "createdAt": "2021-08-17T02:36:39.104"
}
```

Use o `curl` para indexar o documento. O mesmo será indexado no índice `customers`. A palavra `_doc` é reservada e iremos entende-lá melhor quando falarmos sobre *mappings*. 

```sh
$ curl -X POST "http://localhost:9202/customers/_doc" -H "Content-Type: application/json" -d '
{
  "id": 1,
  "name": "Christopher M. Bass",
  "age": 45,
  "createdAt": "2021-08-17T02:36:39.104"
}'
```

O cluster confirma a indexação e como não atribuímos um identificador para o documento, o cluster cria automáticamente. Como podemos ver no atributo `_id`.

```json
{
  "_index": "customers",
  "_type": "_doc",
  "_id": "TKMXUnsBDnCk96a8UqV7",
  "_version": 1,
  "result": "created",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 1,
  "_primary_term": 1
}
```

Para atribuir um identificador basta informar um valor único depois do `_doc/?`. Ex: `http://localhost:9202/customers/_doc/1"`.

É importante frisar a diferença entre `id` e `_id`. O `_id` é identificador único do elasticsearch, diferentemente de `id` que pode conter ou não no documento indexado.

Para visualizar o documento indexado podemos fazer a pesquisa pelo identificador `_id`. Ex:

```sh
$ curl -X GET "http://localhost:9202/customers/_doc/1"
```

```json
{
  "_index": "customers",
  "_type": "_doc",
  "_id": "1",
  "_version": 1,
  "_seq_no": 0,
  "_primary_term": 1,
  "found": true,
  "_source": {
    "id": 1,
    "name": "Christopher M. Bass",
    "age": 45,
    "createdAt": "2021-08-17T02: 36: 39.104"
  }
}
```

No próximo artigo iremos entender como retornar todos os documentos de um indíce. Até lá pratique bem essa etapa, ela é o coração para um bom aprendizado sobre elasticsearch.