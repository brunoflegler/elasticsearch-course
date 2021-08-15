**Iniciando no Elasticsearch**

1 - Instale e inicie [Docker Desktop](https://www.docker.com/products/docker-desktop).

2 - Crie o arquivo docker-compose.yml e adicione:

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
  ```
3 - Inicie o cluster:

```sh
$ docker-compose up --build -d
```

4 - Use o `curl` para acessar o status do cluster:

```sh
$ curl -X GET http://localhost:9202/_cluster/health?pretty
```

O status do cluster é definido pelo funcionamento da shards primárias e réplica. Os status são:

*green*: Todos as shards foram iniciadas.

*yellow*: Todas shards primárias foram iniciadas, mas uma ou mais réplicas não foi iniciada. Se um nó falhar, alguns dados podem ficar indisponíveis até o nó seja reparado.

*red*: Uma ou mais shards primárias não foram iniciadas, portanto os dados estão indisponíveis. 

```
{
  "cluster_name" : "docker-cluster",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 1,
  "active_shards" : 1,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}

```

O intuito desse artigo não é detalhar todos as propriedades, durante os próximos artigos veremos como mais detalhes. Te vejo até lá.