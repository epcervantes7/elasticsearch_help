# elasticsearch_help

## Create Index

```json
PUT shop_brasil_products/_mapping
{
  "properties": {
    "id": {
      "type": "keyword"
    },
    "description": {
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
    path => "/home/dataset.csv"
    start_position => "beginning"
   sincedb_path => "/dev/null"
  }
}
filter {
  csv {
      separator => ";"
     columns => ["id", "description"]
     quote_char => "'"
  }
}
output {
   elasticsearch {
     hosts => "http://localhost:9200"
     index => "shop_brasil_products"
  }
stdout {}
}
```


```console
home:~$ /opt/logstash/bin/logstash -f csv.conf

```
## number of replicas
```
curl -XPUT 'http://localhost:9200/_settings' -d '
{
    "index" : {
        "number_of_replicas" : 0
    }
}'
```
