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

# Loading JSON datas as a single shot

Loading Chambéry boundaries as a GeoJSON document could be done this way:

```
% curl 'http://polygons.openstreetmap.fr/get_geojson.py?id=74386&params=0' | curl -XPUT -d @- -H 'Content-Type: application/json' http://localhost:9200/boundaries/_doc/1
```
