# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-12-27

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp and tornado are exceptions on the
moment).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2021-12-27)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22baize%22%2C%22emmett%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22sanic%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B472935%2C420570%2C395925%2C323595%2C321450%2C299400%2C245640%2C184965%2C110085%2C98550%2C61860%2C34710%5D%7D%5D%7D%7D' />

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

2. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.

3. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.


## The Results (2021-12-27)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.2` | 17085 | 3.07 | 4.89 | 3.71
| [muffin](https://pypi.org/project/muffin/) `0.86.3` | 14801 | 3.56 | 5.58 | 4.30
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 14133 | 3.63 | 6.01 | 4.49
| [baize](https://pypi.org/project/baize/) `0.14.1` | 13154 | 3.98 | 6.37 | 4.83
| [starlette](https://pypi.org/project/starlette/) `0.17.1` | 12233 | 4.32 | 6.84 | 5.19
| [emmett](https://pypi.org/project/emmett/) `2.3.2` | 12200 | 4.20 | 6.93 | 5.21
| [fastapi](https://pypi.org/project/fastapi/) `0.70.1` | 8834 | 5.78 | 9.64 | 7.21
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.1` | 6419 | 9.74 | 9.97 | 9.97
| [quart](https://pypi.org/project/quart/) `0.16.2` | 3073 | 21.34 | 22.30 | 20.82
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2969 | 21.59 | 21.86 | 21.56
| [sanic](https://pypi.org/project/sanic/) `21.12.0` | 1511 | 31.88 | 59.87 | 42.29
| [django](https://pypi.org/project/django/) `4.0` | 900 | 65.57 | 79.01 | 71.02


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.2` | 9393 | 5.37 | 9.13 | 6.78
| [muffin](https://pypi.org/project/muffin/) `0.86.3` | 9323 | 5.47 | 9.16 | 6.83
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 9101 | 5.54 | 9.53 | 6.99
| [starlette](https://pypi.org/project/starlette/) `0.17.1` | 7198 | 7.06 | 11.91 | 8.86
| [emmett](https://pypi.org/project/emmett/) `2.3.2` | 6490 | 7.68 | 13.06 | 9.93
| [baize](https://pypi.org/project/baize/) `0.14.1` | 5874 | 11.08 | 11.44 | 10.87
| [fastapi](https://pypi.org/project/fastapi/) `0.70.1` | 5632 | 8.81 | 15.61 | 11.33
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.1` | 3986 | 15.84 | 16.08 | 16.08
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2457 | 26.01 | 26.34 | 26.05
| [quart](https://pypi.org/project/quart/) `0.16.2` | 1925 | 32.96 | 33.77 | 33.22
| [sanic](https://pypi.org/project/sanic/) `21.12.0` | 1427 | 34.55 | 63.80 | 44.79
| [django](https://pypi.org/project/django/) `4.0` | 800 | 74.32 | 88.01 | 79.89

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.2` | 5051 | 9.98 | 17.19 | 12.64
| [muffin](https://pypi.org/project/muffin/) `0.86.3` | 3914 | 13.24 | 21.83 | 16.36
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 3161 | 16.65 | 26.87 | 20.30
| [baize](https://pypi.org/project/baize/) `0.14.1` | 2402 | 26.02 | 29.43 | 26.64
| [starlette](https://pypi.org/project/starlette/) `0.17.1` | 2142 | 34.13 | 38.92 | 29.82
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.1` | 1926 | 32.94 | 33.21 | 33.22
| [tornado](https://pypi.org/project/tornado/) `6.1` | 1913 | 33.49 | 33.82 | 33.44
| [fastapi](https://pypi.org/project/fastapi/) `0.70.1` | 1910 | 38.28 | 43.16 | 33.44
| [quart](https://pypi.org/project/quart/) `0.16.2` | 1572 | 40.84 | 41.99 | 40.67
| [emmett](https://pypi.org/project/emmett/) `2.3.2` | 1270 | 47.26 | 55.91 | 50.31
| [sanic](https://pypi.org/project/sanic/) `21.12.0` | 1186 | 42.27 | 75.83 | 53.86
| [django](https://pypi.org/project/django/) `4.0` | 614 | 97.99 | 104.67 | 104.02


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.2` | 472935 | 6.14 | 10.4 | 7.71
| [muffin](https://pypi.org/project/muffin/) `0.86.3` | 420570 | 7.42 | 12.19 | 9.16
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 395925 | 8.61 | 14.14 | 10.59
| [starlette](https://pypi.org/project/starlette/) `0.17.1` | 323595 | 15.17 | 19.22 | 14.62
| [baize](https://pypi.org/project/baize/) `0.14.1` | 321450 | 13.69 | 15.75 | 14.11
| [emmett](https://pypi.org/project/emmett/) `2.3.2` | 299400 | 19.71 | 25.3 | 21.82
| [fastapi](https://pypi.org/project/fastapi/) `0.70.1` | 245640 | 17.62 | 22.8 | 17.33
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.1` | 184965 | 19.51 | 19.75 | 19.76
| [tornado](https://pypi.org/project/tornado/) `6.1` | 110085 | 27.03 | 27.34 | 27.02
| [quart](https://pypi.org/project/quart/) `0.16.2` | 98550 | 31.71 | 32.69 | 31.57
| [sanic](https://pypi.org/project/sanic/) `21.12.0` | 61860 | 36.23 | 66.5 | 46.98
| [django](https://pypi.org/project/django/) `4.0` | 34710 | 79.29 | 90.56 | 84.98

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)