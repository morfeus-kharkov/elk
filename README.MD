ELK:
1) Install ElasticSearch(ES). It uses 9200 port

You can check that ES is up: http://localhost:9200

You can add new document to ES

* curl -X PUT "localhost:9200/customer/_doc/1?pretty" -H 'Content-Type: application/json' -d'
{
"name": "John Doe"
}

and than check that it's appeared in ES

* curl -X GET "localhost:9200/_cat/indices/"
* curl -X GET "localhost:9200/customer/_doc/1"
   
2) Install kibana. It uses 5601 port
   
Modify file /usr/share/kibana/config/kibana.yml
   
(see example in this project in  folder "config/kibana". I only set url to ES)

If you added test document into ES above, you can check that kibana shows documents from ES:

* open kibana: http://localhost:5601
* go to stack managements/index patterns/create index pattern
* set pattern name customer*, next, next...
* go to Kibana discover/set needed pattern/refresh
* you should see your created document

3) Install logstash and change files:
* /usr/share/logstash/pipeline/logstash.conf   
* /usr/share/logstash/config/logstash.yml
   
   (see example in folder "config/logstash")

In logstash.yml I only set url to ES

In logstash.conf you should set input and output plugins

* As far as we writing to ES, we set output plugin "elasticsearch", and set url to our ES

I set index => "logback-%{+YYYY.MM.dd}", therefore each day new index will be created in ES. 

* In this project we fetch logs from java project, so I added "file" input plugin, and set path to logs that I want to parse

There are lot of logstash plugins. You can find documentation here: 
* https://www.elastic.co/guide/en/logstash/current/input-plugins.html
* https://www.elastic.co/guide/en/logstash/current/output-plugins.html
-------------------
In java project:
1) Added logstash-logback-encoder in maven
2) In logback.xml added STASH appender
3) While running ElkTest would be appended log message to file logback/redditApp.log in appropriate for logstash input plugin format 
4) if logstash input plugin is set up to fetch documents from logs (see input plugin "file", and path to logs), documents will be created in ES 
-------------------
Conclusion:
* You can use any appropriate input codec in logstash add documents to ES (TCP, RabbitMq, File, http, rabbitMQ e.t.c) 
