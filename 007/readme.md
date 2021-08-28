**Paginando resultados na pesquisa do Elasticsearch**

Como mencionado no artigo [#003: Buscando todos os documentos no Elasticsearch](https://github.com/brunoflegler/elasticsearch-course/blob/master/003/readme.md), por padrão ele retorna 10 documentos. Hoje iremos aprender a paginar os resultados.

Vamos usar os parâmetros `size` que é o total de registros que será retornado e o `from` que é a quantidade de documentos que serão ignorados, começando como 0. Juntos, as duas propriedades formam a paginação. Dessa forma temos na primeira requisição:

```json
GET customers/_search
{
  "query": {
    "match_all": {}
  },
  "from": 0,
  "size": 10
}
```

Na segunda requisição será `from` + `size`, igual à 0 + 10 = 10:

```json
GET customers/_search
{
  "query": {
    "match_all": {}
  },
  "from": 10,
  "size": 10
}
```

Na terceira requisição será `from` + `size`, igual à 10 + 10 = 20:

```json
GET customers/_search
{
  "query": {
    "match_all": {}
  },
  "from": 20,
  "size": 10
}
```

E assim por diante, porém por padrão a soma de `from` + `size` não pode ultrapassar 10000. É aconselhável usar `search_after` para essa situação, veremos nos próximos artigos.

> ⚠️ **Atenção**:
> 
> Evite usar o paginação com muita profundidade, ou seja, o `from` com um valor muito alto ou solicitar muitos resultados de uma vez. Os registros das páginas anteriores são mantidos em mémoria e podem aumentar o consumo de mémoria e CPU, resultando em degradação do desempenho ou falhas de nó.

Sua vez de praticar, te vejo em breve.