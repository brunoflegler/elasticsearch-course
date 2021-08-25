**Ordenando resultados na pesquisa do Elasticsearch**

Artigo super rápido e fácil. Vamos aprender a ordenar os resultados do Elasticsearch. 

```sh
GET customers/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "age": "asc"
    }
  ]
}
```

A ordenação pode ser `asc` ou `desc`. Por padrão o Elasticsearch utiliza a propriedade `_score: desc`, ou seja do maior para o menor. Quando adicionamos qualquer outra propriedade a ordenação é ascendente, do menor para o maior. 

> ⚠️ **Atenção**:
> 
> Uma vez adicionado a ordenação o `_score` é null, uma vez que não será utilizado a relevância do documento.

O `_score` é o resultado do cálculo de relevância sobre a sua pesquisa, quanto maior `_score` mais relevante é o documento, iremos entender melhor nos próximos artigos.

Como podemos observar a ordenação é um array e podemos adicionar mais propriedade.

```json
"sort": [
  {
    "field1": "asc"
  },
  {
    "field2": "desc"
  }
]
```

Artigo super rápido, mas super importante no dia a dia.