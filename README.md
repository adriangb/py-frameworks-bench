# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-04-29

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp is an exception).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2021-04-29)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Composite stats ](#composite)

## The Methodic

The benchmark runs as a [Github Action](https://github.com/features/actions).
According to the [github
documentation](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
the hardware specification for the runs is:

* 2-core vCPU (Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1GHz (Skylake))
* 7 GB of RAM memory
* 14 GB of SSD disk space
* OS Ubuntu 20.04

[ASGI](https://asgi.readthedocs.io/en/latest/) apps are running from docker using the gunicorn/uvicorn command:

    gunicorn -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8080 app:app

Applications' source code can be found
[here](https://github.com/klen/py-frameworks-bench/tree/develop/frameworks).

Results received with WRK utility using the params:

    wrk -d15s -t4 -c64 [URL]

The benchmark has a three kind of tests:

1. "Simple" test: accept a request and return HTML response with custom dynamic
   header. The test simulates just a single HTML response.

2. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.

3. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.


## The Results (2021-04-29)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 16541 | 3.55 | 4.87 | 3.85
| [muffin](https://pypi.org/project/muffin/) `0.69.5` | 14926 | 4.26 | 5.41 | 4.25
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 14121 | 4.60 | 5.72 | 4.49
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 12965 | 4.61 | 6.21 | 4.91
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 12508 | 4.40 | 6.61 | 5.08
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 9358 | 5.79 | 8.89 | 6.81
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 7658 | 7.97 | 10.62 | 8.37
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 7165 | 8.68 | 9.06 | 8.93
| [quart](https://pypi.org/project/quart/) `0.14.1` | 3276 | 19.82 | 20.88 | 19.52
| [django](https://pypi.org/project/django/) `3.2` | 1463 | 42.79 | 48.51 | 43.69


</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 5249 | 10.34 | 15.96 | 12.19
| [muffin](https://pypi.org/project/muffin/) `0.69.5` | 4045 | 12.82 | 21.00 | 15.80
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 3602 | 14.99 | 23.23 | 17.78
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 3256 | 16.65 | 25.84 | 19.73
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 2231 | 23.43 | 37.91 | 28.64
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2091 | 30.26 | 30.64 | 30.62
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 1972 | 31.85 | 42.08 | 32.40
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 1250 | 48.30 | 56.46 | 51.19
| [quart](https://pypi.org/project/quart/) `0.14.1` | 1206 | 53.28 | 55.25 | 53.00
| [django](https://pypi.org/project/django/) `3.2` | 815 | 76.44 | 88.56 | 78.30


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 9109 | 5.95 | 9.13 | 7.00
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 8841 | 6.20 | 9.41 | 7.21
| [muffin](https://pypi.org/project/muffin/) `0.69.5` | 8594 | 6.16 | 9.76 | 7.43
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 7722 | 7.26 | 10.86 | 8.25
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 6641 | 7.68 | 12.66 | 9.63
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 6372 | 7.89 | 13.16 | 10.11
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 5906 | 8.70 | 14.44 | 10.80
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 4474 | 14.05 | 14.37 | 14.31
| [quart](https://pypi.org/project/quart/) `0.14.1` | 2122 | 29.94 | 30.84 | 30.15
| [django](https://pypi.org/project/django/) `3.2` | 1252 | 50.83 | 56.86 | 51.05

</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 463485 | 6.61 | 9.99 | 7.68
| [muffin](https://pypi.org/project/muffin/) `0.69.5` | 413475 | 7.75 | 12.06 | 9.16
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 375930 | 9.15 | 13.82 | 10.62
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 361110 | 11.76 | 18.16 | 13.79
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 301950 | 20.2 | 25.41 | 22.13
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 268515 | 10.21 | 15.5 | 11.93
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 258540 | 15.45 | 21.8 | 16.67
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 205950 | 17.66 | 18.02 | 17.95
| [quart](https://pypi.org/project/quart/) `0.14.1` | 99060 | 34.35 | 35.66 | 34.22
| [django](https://pypi.org/project/django/) `3.2` | 52950 | 56.69 | 64.64 | 57.68

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)