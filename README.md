# ElasticCommands

GET /_cluster/health

GET /_cat/nodes?v

GET /_cat/indices?v

PUT /pages

GET /_cat/shards?v

DELETE /pages

PUT /products
{
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 2
  }
}

POST /products/_doc
{
  "name": "Coffee Maker",
  "price": 64,
  "in_stock": 10
  
}

POST /products/_doc/100
{
  "name": "Toster",
  "price": 49,
  "in_stock": 4
  
}

GET /products/_doc/100

POST  /products/_update/100
{
  "doc":{
    "tags":["elec"]
  }
}

POST  /products/_update/100
{
  "script": {
    "source":"ctx._source.in_stock--"
  }
}

POST /products/_update/100
{
  "script": {
    "source":"ctx._source.in_stock = 10"
  }
}

POST  /products/_update/100
{
  "script": {
    "source":"ctx._source.in_stock -= params.quantity",
    "params": {
      "quantity": 4
    }
  }
}

POST  /products/_update/101
{
  "script": {
    "source":"ctx._source.in_stock++"
  },
  "upsert": {
    "name": "Blender",
    "prace": 399,
    "in_stock":5
  }
}

GET /products/_doc/101

PUT /products/_doc/100
{
  "name":"Toaster",
  "price": 79,
  "in_stock": 4
}

GET /products/_doc/100

DELETE /products/_doc/101

POST /products/_update/100?if_primary_term=1&if_seq_no=10
{
  "doc": {
    "in_stock": 123
  }
}

POST /products/_update_by_query
{
  "conflicts": "proceed", 
  "script": {
    "source": "ctx._source.in_stock--"
  },
  "query": {
    "match_all": {
      
    }
  }
}

POST /products/_delete_by_query
{
  "query":{
    "match_all":{}
  }
}

POST /_bulk
{ "index": { "_index": "products", "_id": 200 } }
{ "name": "Espresso Machine", "price":199, "in_stock":5 }
{ "create": { "_index": "products", "_id": 201 } }
{ "name": "Milk Frother", "price":149, "in_stock":14 }

POST /products/_bulk
{ "update": { "_id": 201 } }
{ "doc": { "price": 129 } }
{ "delete": { "_id": 200 } }

GET /products/_search
{
    "query": {
    "match_all": {
      
    }
  }
}

POST /_analyze
{
  "text": "2 guys walk",
  "analyzer": "keyword"
}

POST /_analyze
{
  "text": "2 guys walk.... DUCKS! []",
  "char_filter": [],
  "tokenizer": "standard",
  "filter": ["lowercase"]
}

PUT /reviews
{
  "mappings":{
    "properties":{
      "rating":{"type":"float"},
      "content":{"type":"text"},
      "product_id":{"type":"integer"},
      "auther":{
        "properties":{
          "first_name":{"type":"text"},
          "last_name":{"type":"text"},
          "email":{"type":"keyword"}
        }
      }
    }
  }
}

PUT /reviews/_doc/1
{
  "rating": 5.0,
  "content":"Outstanding ......",
  "product_id": 123,
  "auther":{
    "first_name":"John",
    "last_name":"Doe",
    "email": "johndoe123@exam.com"
  }
}

PUT /reviews/_doc/1
{
  "rating": 5.0,
  "content":"Outstanding ......",
  "product_id": 123,
  "auther":{
    "first_name":"John",
    "last_name":"Doe",
    "email": {}
  }
}

GET /reviews/_mapping

GET /reviews/_mapping/field/content

PUT /reviews
{
  "mappings":{
    "properties":{
      "rating":{"type":"float"},
      "content":{"type":"text"},
      "product_id":{"type":"integer"},
      "auther":{
        "properties":{
          "first_name":{"type":"text"},
          "last_name":{"type":"text"},
          "email":{"type":"keyword"}
        }
      }
    }
  }
}

PUT /reviews_dot_notation
{
  "mappings":{
    "properties":{
      "rating":{"type":"float"},
      "content":{"type":"text"},
      "product_id":{"type":"integer"},
      "auther.first_name":{"type":"text"},
      "auther.last_name":{"type":"text"},
      "auther.email":{"type":"keyword"}
    }
  }
}

GET /reviews_dot_notation/_mapping





