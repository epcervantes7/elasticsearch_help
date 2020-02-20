# elasticsearch_help

## Create Index
```
PUT base_grupo_big
```

```json
PUT base_grupo_big/_mapping
{
  "properties": {
    "DIVISAO": {
      "type": "text"
    },
    "SUBCATEGORIA": {
      "type": "text"
    },
    "FINELINE": {
      "type": "text"
    },
    "UPC": {
      "type": "text"
    },
    "DESCRICAO_DO_TEM": {
      "type": "text"
    },
    "TIPO_CAIXA_FORN": {
      "type": "text"
    },
    "COD_FAMILIA": {
      "type": "text"
    },
    "MARCA": {
      "type": "text"
    },
    "TAMANHO": {
      "type": "text"
    }
  }
}
```
## Insert data from csv to index

file:csv.conf

```json
input {
  file {
    path => "/home/base_grupo_big.csv"
    start_position => "beginning"
   sincedb_path => "/dev/null"
  }
}
filter {
  csv {
      separator => ";"
     columns => ["DIVISAO","SUBCATEGORIA","FINELINE","UPC","DESCRICAO_DO_ITEM","TIPO_CAIXA_FORN","COD_FAMILIA","MARCA","TAMANHO"]
     quote_char => "'"
  }
}
output {
   elasticsearch {
     hosts => "http://localhost:9200"
     index => "base_grupo_big"
  }
stdout {}
}
```


```console
home:~$ /opt/logstash/bin/logstash -f base_grupo_big.conf

```
## number of replicas
```
PUT /_settings
{
    "index" : {
        "number_of_replicas" : 0
    }
}'
```
## update settings analyzer
```
POST /base_grupo_big/_close
```

```
PUT base_grupo_big/_settings
{
    "analysis": {
      "filter": {
        "brazilian_stop": {
          "type":       "stop",
          "stopwords":  "_brazilian_" 
        },
        "brazilian_keywords": {
          "type":       "keyword_marker",
          "keywords":   ["exemplo"] 
        },
        "brazilian_stemmer": {
          "type":       "stemmer",
          "language":   "brazilian"
        }
      },
      "analyzer": {
        "rebuilt_brazilian": {
          "tokenizer":  "standard",
          "filter": [
            "lowercase",
            "brazilian_stop",
            "brazilian_keywords",
            "brazilian_stemmer"
          ]
        }
      }
    }
}
```
```
POST /base_grupo_big/_open
```

## index shopping brasil
```
PUT shopping_brasil
```

```
PUT shopping_brasil/_mapping
{
  "properties": {
    "CATEGORIA": {
      "type": "text"
    },
    "SUBCATEGORIA": {
      "type": "text"
    },
    "DESCRICAOREDUZIDA": {
      "type": "text"
    },
    "CLASSIFICACAO_COMPLETA": {
      "type": "text"
    },
    "ORDEM": {
      "type": "text"
    },
    "IDPRDT": {
      "type": "text"
    },
    "MODELO": {
      "type": "text"
    },
    "DESCRICAO": {
      "type": "text"
    },
    "IDMARCA": {
      "type": "text"
    },
    "IDSEGMENTOBASE": {
      "type": "text"
    },
    "IDGRUPOBASE": {
      "type": "text"
    },
    "GRUPOBASE": {
      "type": "text"
    },
    "GENERICO": {
      "type": "text"
    },
    "SITUACAO": {
      "type": "text"
    },
    "ADS": {
      "type": "text"
    },
    "UNIDADE": {
      "type": "text"
    },
    "IDTIPOUNIDADE": {
      "type": "text"
    },
    "APRESENTACAO": {
      "type": "text"
    },
    "STATUS": {
      "type": "text"
    }
  }
}
```


```json
input {
  file {
    path => "/home/Banco_SB_completa.csv"
    start_position => "beginning"
   sincedb_path => "/dev/null"
  }
}
filter {
  csv {
      separator => ";"     columns => ["CATEGORIA","SUBCATEGORIA","DESCRICAOREDUZIDA","CLASSIFICACAO_COMPLETA","ORDEM","IDPRDT","MODELO","DESCRICAO","IDMARCA","MARCA","IDSEGMENTOBASE","SEGMENTOBASE","IDGRUPOBASE","GRUPOBASE","GENERICO","SITUACAO","ADS","UNIDADE","IDTIPOUNIDADE","APRE
SENTACAO","STATUS"]
     quote_char => "'"
  }
}
output {
   elasticsearch {
     hosts => "http://localhost:9200"
     index => "shopping_brasil"
  }
stdout {}
}
```
