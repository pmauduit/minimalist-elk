# Purpose

A minimalist ELK docker composition.

# howto

1. Hack around the `logstash.conf` configuration
2. `docker-compose up`
3. feed the glouton, using e.g.:

```
oc logs -f pod-identifier | nc localhost 9000
```

