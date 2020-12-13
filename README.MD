In java project:
1) Added logstash-logback-encoder in maven
2) In logback.xml added STASH appender
3) Added one test for checking that log file is created

ELK:
1) Install ES
2) Install kibana and modify file /usr/share/kibana/config/kibana.yml
   (see example in folder config)
3) Install logstash and change files:
   /usr/share/logstash/pipeline/logstash.conf
   /usr/share/logstash/config/logstash.yml
   (see example in folder config)
   
Tests:
1) add document into ES:
   curl -X PUT "localhost:9200/customer/_doc/1?pretty" -H 'Content-Type: application/json' -d'
   {
   "name": "John Doe"
   }
   '


2) check indices
   curl -X GET "localhost:9200/_cat/indices/"
   
3) Kibana: go to stack managements/index patterns/create index pattern
.Set pattern name customer*, next, next... Go to Kibana discover/set needed pattern/refresh
4) You should see your log documents    
