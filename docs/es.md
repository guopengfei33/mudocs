# 设备缓存树安装相关

#### 软件版本

* CentOS Linux release 7.3.1611 (Core)
* jdk 1.8.0_131
* elasticsearch-5.4.1
* logstash-5.4.1  

#### mapping 设置

    GET equipment/_mapping/unit/

#### 删除、查看索引    

	curl -XDELETE 'http://nari_185:9200/equipment?pretty'
	curl 'http://nari_185:9200/_cat/indices?v'
	

[223_185/unit/_search](http://nari_223_185:19200/equipment/unit/_search)

[es.xiaoleilu.com](https://es.xiaoleilu.com/)

#### corn

	0th minute of every hour every day
	schedule => "0 * * * *"	
	
	//删除索引
	DELETE equipment
	
	//创建索引
	PUT equipment 
    {
        "mappings": {
            "unit": { 
        "properties": { 
            "id": {
            "type": "keyword",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          }, 
            "name": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
            "pid": {
            "type": "keyword",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
            "orgid": {
            "type": "keyword",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          }
        }
        }
      }
    }
	
	//查看映射
	GET equipment/_mapping/unit/
	
	//查询
	POST equipment/_search?pretty
	{
  		"query": {
    		"bool" : {
		      "filter": {
        		"term" : { "pid" : "100" }
      			}
    		}
  		}
	}
	
	

## centos es

#### IP

    172.16.221.59 root nariadmin

#### 注意要

    ➜  .ssh vi config
    sudo yum localinstall jre-7u79-linux-x64.rpm

#### centos es安装问题

    sudo rpm --install elasticsearch-5.4.1.rpm

#### centos es常用命令

    sudo -i service elasticsearch start
    sudo -i service elasticsearch stop

    /var/log/elasticsearch/
    curl -XGET 'localhost:9200/?pretty'

    logstash

#### centos es 配置文件

    /etc/elasticsearch/elasticsearch.yml  
    #
    http.cors.enabled: true
    http.cors.allow-origin: /.*/
    #
    /etc/logstash/logstash.yml
    
#### centos es同步表到索引

	./logstash -f ../config/jdbc_oracle.conf    

#### centos basic 

    service httpd start



# Window 上的测试数据
	

### 使用CURL执行命令(put-增加/delete-删除/post-修改/get-获取)
	
	1. 插入一个文档 : 要在/music 索引下创建一个类型,可插入一个文档
		1. curl -XPUT "http://127.0.0.1:9200/music/songs/1" -d '{"name" : "Deck the Halls", "year" : 2017,"Lyrics" : "Fa la la la"}'
		2. 前面的命令使用 PUT 动词将一个文档添加到/songs 文档类型.,并为该文档分配ID为1
	2. 查看文档 : 要查看文档使用简单的 GET 命令
		1. curl -XGET "HTTP://127.0.0.1:9200/music/songs/1" 
		2. ES 将之间 put 的数据以 JSON 的格式进行响应
	3. 更新文档 : 由于使用与插入一样的命令ID一样 所以会更新
		    curl -XPUT "HTTP://127.0.0.1:9200/music/songs/1" -d {
			  "name" : "Deck the Halls",
			  "year" : 2001,
			  "Lyrics" : "Fa la la la"
		    }
	4. 删除文档 :
		1.  curl -XDELETE "http://127.0.0.1:9200/music/songs/1"
	5. 插入文件(必须在xxx.json 所在文件夹下) :
		1.  curl -XPUT "http://127.0.0.1:9200/music/songs/3" -d @xxx.json
	
### 搜索 REST API
	## 查询

		- 文档URL有一个内置的 _search 端点用于此用途,在songs中查找含有 the 的歌曲
		- curl -XGET "http://127.0.0.1:9200/music/songs/_search?q=name:'the'"
		- q 表示一个查询
	

	
	## 使用其他比较符
		- curl -XGET "http://127.0.0.1:9200/music/songs/_search?q=year:<2018"
		- 此查询将返回年份小于2018的数据
	

	## 限制字段(报错 the parameter [fields] is no longer supported)
		- curl -XGET "http://127.0.0.1:9200/music/songs/_search?q=name:'the'&fiels=name" 



### ES 与 java 的集成


### ES 常用的命令

##请求参数
		
	- 请求参数(curl)
		- curl -XPOST 'http://127.0.0.1:9200/bank/_search?q=*&pretty'
		- 其中bank 是查询索引,q后边跟搜索条件:q=*表示查询所有的内容
		- curl -XPOST 'http://127.0.0.1:9200/bank/_search?pretty' -d '
		- {
			- "query" : {
				- "match_all" : {}
			- }
		- }
	- 请求参数(kibana)
		- POST bank/_search?q=*&pretty
		- POST bank/_Search?pretty	
		- {
			- "query" : {
				- "match_all" : {}
			- }
		- }


## 返回内容
	took：是查询花费的时间，毫秒单位
	time_out：标识查询是否超时
	_shards：描述了查询分片的信息，查询了多少个分片、成功的分片数量、失败的分片数量等
	hits：搜索的结果，total是全部的满足的文档数目，hits是返回的实际数目（默认是10）
	_score是文档的分数信息，与排名相关度有关，参考各大搜索引擎的搜索结果，就容易理解。 

## 查询语言DSL(es支持一种JSON的格式的查询) [引用](https://www.cnblogs.com/xing901022/p/4967796.html "引用")
		
	1. query 定义了查询,match_all声明了查询的内容
		1. size 表示查询的数量,from表示从第几个开始
			POST bank/_search?pretty
			{
				"query" : {"match_all" : {} },
				"size" : 10,
				"from" : 10
			} 

		2. sort : 排序
			POST  bank/_search?pretty
			{
			  "query": {
			    "match_all": {}
			  },
			  "sort": [
			    {
			      "balance": {
			        "order": "asc"
			      }
			    },
			    {
			      "age": {
			        "order": "desc"
			      }
			    }
			  ]
			} 

		3. _source : [fields] -- 指定返回的列
			POST bank/_search?pretty
			{
			  "query": {
			    "match_all": {}
			  },
			  "_source": ["age","firstname","address"]
			}
	
		4. match方式查询特定字段的特定内容
			POST bank/_search?pretty
			{
			  "query" : {
			    "match": {
			      "account_number": "2"
			    }
			  }
			}
			--address 包含 mill 或者 lane
			POST bank/_search?pretty
			{
			  "query" : {
			    "match": {
			      "address": "mill lane"
			    }
			  }
			}
			--match_phrase 同时包含
			POST bank/_search?pretty
			{
			  
			  "query": {
			    "match_phrase": {
			      "address": "mill lane"
			    }
			  }
			}
	2. ES bool查询 : 可以将很多小的查询组合起来
			同时包含 mill 和 lane
		1.  POST bank/_search?pretty
			{
			  "query": {
			    "bool": {
			      "must": [
			        {"match": {
			          "address": "mill"
			        }},
			        {
			          "match": {
			            "address": "lane"
			          }
			        }
			      ]
			    }
			  }
			}
		2.  包含 mill 或者 lane
			POST bank/_search?pretty
			{
			  "query": {
			    "bool": {
			      "should": [
			        {"match": {
			          "address": "mill"
			        }},
			        {
			          "match": {
			            "address": "lane"
			          }
			        }
			      ]
			    }
			  }
			}

		3.  must_not 排除包含
		    POST bank/_search?pretty
			{
			  "query": {
			    "bool": {
			      "must_not": [
			        {"match": {
			          "address": "mill"
			        }},
			        {"match": {
			          "address": "line"
			        }}
			      ]
			    }
			  }
			}
		4.  bool 组合
		    POST bank/_search?pretty
			{
			  "query": {
			    "bool": {
			      "must": [
			        {"match": {
			          "age": "40"
			        }}
			      ],
			      "must_not": [
			        {"match": {
			          "state": "ID"
			        }}
			      ]
			    }
			  }
			}

		5. filter过滤可以嵌套在bool查询内部使用，比如想要查询在2000-3000范围内的所有文档，可以执行下面的命令(range-范围)：
			POST bank/_search?pretty
			{
			  
			  "query": {
			    "bool": {
			      "must": [
			        {"match_all": {}}
			      ],
			      "filter": {
			        "range": {
			          "balance": {
			            "gte": 20,
			            "lte": 60
			          }
			        }
			      }
			    }
			  }
			}
	
	3. 聚合:聚合提供了用户进行分组和数理统计的能力，可以把聚合理解成SQL中的GROUP BY和分组函数(例子报错)--可能版本的问题(后续分析)
		1. POST bank/_search?pretty
			{
			  "size": 0, 
			  "aggs": {
			    "average_balance": {
			      "avg": {
			        "field": "balance"
			      }
			    }
			  }
			}


### 数据交换工具(从oracle到Elasticsearch)[网页引用](http://www.cnblogs.com/xing901022/p/5901095.html "网页引用")