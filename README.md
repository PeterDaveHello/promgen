# What is Promgen?

Promgen is a configuration file generator for [Prometheus](http://prometheus.io). Promgen is a web application written with [Django](https://docs.djangoproject.com/en/1.10/) and can help you do the following jobs.

* Create and manage Prometheus configuration files
* Configure alert rules and notification options

See the [Promgen introduction slides](http://www.slideshare.net/tokuhirom/promgen-prometheus-managemnet-tool-simpleclientjava-hacks-prometheus-casual) for more details.

## Promgen screenshots

![screenshot](docs/images/screenshot.png)


## Getting started

Below are the steps to get started with Promgen.

### 1. Initialize Promgen

Initialize Promgen using Docker.

```bash
# Initialize required settings with Docker container
# This will prompt you for connection settings for your database and Redis broker
# using the standard DSN syntax.
# Database example: mysql://password:username@hostname/databasename
# Broker example: redis://localhost:6379/0
docker run --rm --network host -v ~/.config/promgen:/etc/promgen/ promgen:latest bootstrap
# Apply database updates
docker run --rm --network host -v ~/.config/promgen:/etc/promgen/ promgen:latest migrate
```

You can then use your favorite configuration management system to deploy to each worker.

> Note: Promgen aims to use the [XDG](https://specifications.freedesktop.org/basedir-spec/latest/ar01s03.html) specs and follows suggestions made by the [12-Factor App](https://12factor.net/).

### 2. Configure Prometheus

Configure Prometheus to load the target file from Prometheus and configure AlertManager to send notifications back to Promgen.

See the example settings files for proper configuration of Prometheus and AlertManager.  

* [Example settings file](promgen/tests/examples/promgen.yml)
* [Example Prometheus file](docker/prometheus.yml)
* [Example AlertManager file](docker/alertmanager.yml)

### 3. Run Promgen

Run Promgen using the following command.

```bash
# Run Promgen web worker. This is typically balanced behind an NGINX instance
docker run --rm --network host -p 8000:8000 -v ~/.config/promgen:/etc/promgen/ promgen:latest web

# Run Promgen celery worker. Make sure to run it on the same machine as your Prometheus server to manage the config settings
docker run --rm --network host -v ~/.config/promgen:/etc/promgen/ -v /etc/prometheus:/etc/prometheus promgen:latest worker
```

## How to contribute to Promgen

First of all, thank you so much for taking your time to contribute! We always welcome your ideas and feedback. Please feel free to make any pull requests.

* File an issue in [the issue tracker](https://github.com/line/promgen/issues) to report bugs and propose new features and improvements.
* Ask a question using [the issue tracker](https://github.com/line/promgen/issues).
* Contribute your work by sending [a pull request](https://github.com/line/promgen/pulls).

### Setting up Promgen for development

You can install Promgen for a development environment as follows.

```bash
virtualenv --python=/path/to/python3 /path/to/virtualenv
source /path/to/virtualenv/activate
pip install -e .[dev]
pip install mysqlclient # psycopg or another database driver
# Setup database and update tables
promgen migrate
# Run tests
promgen test
# Run development server
promgen runserver
```

> Note: Promgen strives to be a standard Django application. Make sure to apply standard Django development patterns.

### Contributor license agreement

If you are sending a pull request and it's a non-trivial change beyond fixing typos, please make sure to sign [the ICLA(individual contributor license agreement)](add link). Please contact us if you need the CCLA (corporate contributor license agreement).

## The MIT License

Copyright (c) 2017 LINE Corporation

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.