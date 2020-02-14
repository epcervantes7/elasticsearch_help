# elasticsearch_help

## Insert data from csv to index
'''
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
'''
