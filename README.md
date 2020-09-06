# Purpose

A minimalist ELK docker composition.

# howto

1. Hack around the `logstash.conf` configuration
2. `docker-compose up`
3. feed the glouton, using e.g.:

```
oc logs -f pod-identifier | nc localhost 9000
```

# Then

Configure Kibana by browsing it: http://localhost:5601/

# Querying ES directly

```
curl -XPOST "http://localhost:9200/_search" -d '{ "query": { "match_all" :{} } }' -H'Content-Type: application/json'
```

