#### 记录看到的问题

ES的具体操作

```
es是基于lucene开发的一个分部式全文索引框架.往ES中存储和从ES中查询,格式为Json
索引:Index 相当于数据库中的database
类型:type 相当于数据库中的table
主键:id 相当于数据库中的主键
```

```
往ES中存储数据,其实就是往ES中的index下的type存储Json数据
Restful风格API
	通过http的形式,发送请求,对ES进行操作
	查询:GET
	删除:DETELE
	添加:PUT/POST
	修改:PUT/POST
```

```
1.PUT操作,store为database;books为table;2为主键
curl -XPUT 'http://hadoop100:9200/store/books/2' -d '{
	"title":"Elasticsearch Blueprints",
	"name":{
		"first": "Vineeth",
		"last": "Mohan"
	},
	"publish_date":"2015-06-06",
	"price":"35.99"
}'

2.GET操作
a)根据id查询:
curl -XGET 'http://hadoop100:9200/store/book/1'
b) 通过_source获取指定的字段
curl -XGET 'http://hadoop100:9200/store/book/1?_source=title'
curl -XGET 'http://hadoop100:9200/store/book/1?_source=title,price'
curl -XGET 'http://hadoop100:9200/store/book/1?_source'
c)最简单_search 结合filter查询
curl -XGET 'http://hadoop100:9200/store/books/_search' -d '{
	"query":{
		"bool": {
			"must": {
				"match_all": {}
			},
			"filter": {
				"term": {
					"price": 35.99
				}
			}
		}
	}
}'
d)bool过滤查询,可以做组合过滤查询
e) 嵌套查询
f) range范围过滤

3.UPDATE操作
a)以通过覆盖的方式更新
b)通过_update API的方式单独更新你想要更新的

4.DETELE操作
curl -XDELETE 'http://192.168.10.16:9200/store/books/1'

5.查询所有(_search)
http://hadoop100:9200/store/books/_search

```

