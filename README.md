# elasticsearch_help

```
docker run -p 5601:5601 -p 9200:9200  -p 5044:5044 -v /home/evelyn/Documentos/shopbrasil/shopbrasil_ocr/data/database/:/var/lib/elasticsearch --name elk2 sebp/elk
```

## index shopping brasil
```
PUT shopping_brasil
```

```
PUT shopping_brasil/_mapping
{
  "properties": {
    "IDPRDT": {
      "type": "text"
    },
    "MARCA": {
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
    "SEGMENTOBASE": {
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
    }
  }
}
```
## update settings analyzer
```
POST /shopping_brasil/_close
```

```
PUT shopping_brasil/_settings
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
POST /shopping_brasil/_open
```

## number of replicas
```
PUT /_settings
{
    "index" : {
        "number_of_replicas" : 0
    }
}
```
## Insert data from csv to index


```json
input {
  file {
    path => "/var/lib/elasticsearch/CLEAN_BANCO_ DE_DADOS_SHOPPING_BRASIL.csv"
    start_position => "beginning"
   sincedb_path => "/dev/null"
  }
}
filter {
  csv {
      separator => ";"     
      columns => ["IDPRDT","MARCA","MODELO","DESCRICAO","IDMARCA","IDSEGMENTOBASE","SEGMENTOBASE","IDGRUPOBASE","GRUPOBASE","GENERICO","SITUACAO","ADS","UNIDADE","IDTIPOUNIDADE","APRESENTACAO"]
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
## stop and restart logstash

```
/etc/init.d/logstash stop

/opt/logstash/bin/logstash -f insert_data.conf
```
