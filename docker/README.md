# docker files

This is a collection of Docker image build files for various purposes.

## Building an image

Builds need to run from the top-level directory since the sources will be
copied to the builder image during the course of the build.

Build the image by referencing the respective docker file from the repository
root:

```shell
$ docker build [-t <tag>] -f docker/<dockerfile> .
```
e.g.
```shell
$ docker build -t cache-stresser -f docker/Dockerfile.cache-stresser .
```

## Cache-stresser
A tiny Docker image based on Alpine for stressing CDN / cache servers.

We maintain an up-to-date container image at
[quay.io](http://quay.io/kinvolk/cache-stresser).


### Using the container to run stress tests

Please **NOTE** the it-looks-like-a-http-url-but-actually-isn't-quite URL format:
 `http[s]://<hostname>/<caching-server-ip>/...`

```shell
$ docker run -ti cache-stresser \
            [-c <overall-num-of-concurrent-connections>] \
            [-r <overall-num-of-requests-per-second>] \
            [-d <duration>] \
            http[s]://<hostname>/<cacheserver-ip>/path \
            [ http[s]://<hostname>/<cacheserver-ip>/path ] \
            ...

```

By default, the stresser uses 10 concurrent connections overall(i.e. 5 per
cache server in the above example), with an overall of 10 requests per second -
that's 1 request per second, per connection. The stress test duration is 60 seconds by default.

While the test is running it will display throughput per interface via the `nload` utility. After the test is done,
throughput, RPS, and latency statistics will be displayed.
