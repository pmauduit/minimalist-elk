# Purpose

A minimalist ELK docker composition.

# howto

1. Hack around the `logstash.conf` configuration (see below)
2. `docker-compose up`
3. feed the glouton, using e.g.:

```
oc logs -f pod-identifier | nc localhost 9000
```

# Hacking the logstash.conf

Several examples are provided in this repository.

* `logstash.conf`: the default one, aimed at receiving plain JSON documents on a network socket (TCP 9000)
* `logstash-combined-apache.conf`: aimed at piping apache-style log files (also works with traefik logs, normally)

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

# If you are short on disk space

your elastic will likely to refuse getting in read/write mode

```
curl -X PUT "localhost:9200/_cluster/settings" -H 'Content-Type: application/json' -d'
{
  "transient": {
    "cluster.routing.allocation.disk.watermark.low": "30mb",
    "cluster.routing.allocation.disk.watermark.high": "20mb",
    "cluster.routing.allocation.disk.watermark.flood_stage": "10mb",
    "cluster.info.update.interval": "1m"
  }
}
'

# Note: it does not seem to work on elasticsearch 8
curl -XPUT -H "Content-Type: application/json" http://localhost:9200/_all/_settings -d '{"index.blocks.read_only_allow_delete": null}'

```
