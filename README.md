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
    path => "/home/example.csv"
    start_position => "beginning"
   sincedb_path => "/dev/null"
  }
}
filter {
  csv {
      separator => ";"
     columns => ["id", "name"]
  }
}
output {
   elasticsearch {
     hosts => "http://localhost:9200"
     index => "user"
  }
stdout {}
}
```


```console
home:~$ /opt/logstash/bin/logstash -f csv.conf

```
