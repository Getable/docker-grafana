## Graphite + Carbon + Statsd + Grafana + Elasticsearch

An all-in-one image running graphite and carbon-cache and grafana.

This image contains a sensible default configuration of graphite and
carbon-cache. Starting this container will, by default, bind the the following
host ports:

- `8080`: the graphite web interface
- `80`: the grafana web interface
- `2003`: the carbon-cache line receiver (the standard graphite protocol)
- `2004`: the carbon-cache pickle receiver
- `7002`: the carbon-cache query port (used by the web interface)

## Install
On an Ubuntu server:

```bash
cd /srv
git clone https://github.com/Getable/docker-grafana
cd grafana-docker-dev-env
bash build
bash start
```

## Usage
Visit: `http://<your ip>`

### Send data

```js
var metricClient = require('dgram').createSocket('udp4')

function sendToStatsd(key, value, type){
  // key is a dot separated string
  // value is a number
  // type is one of ms, c, or g
  // https://github.com/etsy/statsd/blob/master/docs/metric_types.md
  var message = new Buffer(process.env.NODE_ENV + '.' + key + ':' + value + '|' + type)
  metricClient.send(message, 0, message.length, 8125, '<your ip>', function (err, bytes){
    if (err) console.error(err || bytes)
  })
}

```
