# statsd
A docker image for statsd [https://github.com/etsy/statsd](https://github.com/etsy/statsd).


## Run statsd Container

    docker run -e GRAPHITE_HOST=localhost -e GRAPHITE_PORT=2003 -p 8125:8125/udp --name statsd salimane/statsd:v0.8.0

Test statsd:

    echo "foo:1|c" | nc -u -w0 127.0.0.1 8125

For debugging purposes you might want to drop the `-d` switch to show output.


## Build Image Yourself

### Customizing

ENVironment variables:

- GRAPHITE_HOST : the host of graphite (default: "localhost")
- GRAPHITE_PORT : the port of graphite (default: 2003)
- FLUSH_INTERVAL : the interval (in ms) to flush metrics to the backend (default: 1000)
- PERCENT_TRESHOLD : for time information, calculate the Nth percentile(s) (default [ '90' ])
- DEBUG : execute statsd with debug enabled (default: "false")
- LOG_LEVEL: the log level used by statsd (default:  "LOG_INFO")

### Build & Verify
    make
    docker images


## Source

https://github.com/etsy/statsd/