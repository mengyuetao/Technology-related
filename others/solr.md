curl http://localhost:8983/solr/uuch/replication?command=backup
curl http://localhost:8983/solr/uuch/replication?command=details

curl http://localhost:8983/solr/uuch/replication?command=restore
curl http://localhost:8983/solr/uuch/replication?command=restorestatus


cd solr-5.4.0/
  153  bin/solr start


{
 "delete": {"query":"productId_s:(2367)"},
 "commit": {}
}
