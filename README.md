# Requests

Requests is the trusted HTTP library for Python when you need readable code and sane defaults. It smooths over Python's lower-level networking modules so you can focus on the exchange you want to make instead of plumbing. Whether you are checking the health of an internal service, synchronizing data between APIs, or scripting quick automations, Requests keeps the experience predictable and well-documented.

Here is the five-line experience most people start with when they reach for Requests:

```python
>>> import requests
>>> r = requests.get('https://httpbin.org/basic-auth/user/pass', auth=('user', 'pass'))
>>> r.status_code
200
>>> r.headers['content-type']
'application/json; charset=utf8'
>>> r.encoding
'utf-8'
>>> r.text
'{"authenticated": true, ...'
>>> r.json()
{'authenticated': True, ...}
```

The snippet above authenticates against a test service, inspects response metadata, and converts JSON into Python data types. You get ergonomic helpers without losing access to underlying HTTP concepts.

## Community signals

Requests is one of the most downloaded Python packages today, pulling in around 30 million downloads every week. More than a million public repositories depend on it, and long-term contributors across the Python ecosystem help keep the project stable.

[![Downloads](https://static.pepy.tech/badge/requests/month)](https://pepy.tech/project/requests)
[![Supported Versions](https://img.shields.io/pypi/pyversions/requests.svg)](https://pypi.org/project/requests)
[![Contributors](https://img.shields.io/github/contributors/psf/requests.svg)](https://github.com/psf/requests/graphs/contributors)

## Installing Requests and supported versions

Requests officially supports Python 3.9 and newer. The simplest install uses the Python packaging tools that ship with the language:

```console
$ python -m pip install requests
```

If you are new to Python packaging, create an isolated environment first so that dependencies stay tidy:

- `python -m venv .venv && source .venv/bin/activate` for a virtual environment inside your project.
- `pipx install requests` to keep the library in a managed, per-tool environment.
- `conda install requests` if you work in a Conda-managed data science stack.

Requests exposes extras for optional integrations, for example `python -m pip install "requests[socks]"` when you need SOCKS proxy support. The project follows [semantic versioning](https://semver.org), and each release is documented in `HISTORY.md` so you can confirm compatibility changes.

## Core workflows

Requests ships with conveniences for the tasks people hit first when working with HTTP.

### Working with sessions

`requests.Session` objects persist cookies, headers, and connection pools across multiple calls. Use a session when you need to paginate through an API or make a series of related requests without re-authenticating. The [advanced session guide](https://requests.readthedocs.io/en/latest/user/advanced/#session-objects) walks through cookie handling, keep-alive behavior, and context management.

### Authentication strategies

From simple Basic auth through OAuth and custom schemes, Requests can attach credentials in a few lines. Out of the box it supports Basic, Digest, and TLS client certificates, and it is straightforward to plug in custom hooks. See the [authentication documentation](https://requests.readthedocs.io/en/latest/user/authentication/) for recipes and third-party helpers.

### Handling responses

Responses expose convenience helpers (`json()`, `iter_content()`, `raise_for_status()`) while giving you raw access to headers, status codes, and the underlying socket when you need it. The [quickstart guide](https://requests.readthedocs.io/en/latest/user/quickstart/#response-content) explains decoding, streaming downloads, and processing different content types.

### Performance and reliability

Connection pooling, timeouts, automatic retry support through adapter configuration, and proxy helpers keep production workloads predictable. Combine `Session` adapters with timeouts to avoid hanging requests, and lean on features like certificate verification and `.netrc` support to inherit secure defaults. The [advanced usage documentation](https://requests.readthedocs.io/en/latest/user/advanced/) covers these patterns in depth.

## Error handling and debugging

Start with explicit timeouts (`requests.get(url, timeout=5)`) so failures surface quickly, and call `Response.raise_for_status()` to convert HTTP error codes into Python exceptions. Environment variables such as `PYTHONWARNINGS="default::requests.packages.urllib3.exceptions.InsecureRequestWarning"` and the `HTTPDEBUG` tooling in the docs help surface TLS issues. For request and response tracing, enable HTTP client logging as described in the [troubleshooting section](https://requests.readthedocs.io/en/latest/user/advanced/#logging).

## Documentation map

The README covers the most common workflows, while the full API reference and user guides live on [Read the Docs](https://requests.readthedocs.io). Start with the quickstart to explore everyday patterns, then visit the advanced topics for authentication, transport adapters, streaming, and migration notes.

[![Read the Docs](https://raw.githubusercontent.com/psf/requests/main/ext/ss.png)](https://requests.readthedocs.io)

## Contributing and community

Requests is maintained under the Python Software Foundation umbrella. If you want to report an issue or propose a feature, open a discussion or issue on [GitHub](https://github.com/psf/requests/issues). Contributions of all sizes are welcome—read through `CONTRIBUTING.md` for coding standards, testing instructions, and governance details. Community updates and release announcements are published alongside the [PSF blog](https://www.python.org/psf/).

## Cloning the repository

If you see a warning about commit timestamps while cloning the repository, configure Git to ignore the specific check. This usually appears on platforms with strict timezone validation.

```shell
git clone -c fetch.fsck.badTimezone=ignore https://github.com/psf/requests.git
```

You can also apply this setting to your global Git config:

```shell
git config --global fetch.fsck.badTimezone ignore
```

---

[![Kenneth Reitz](https://raw.githubusercontent.com/psf/requests/main/ext/kr.png)](https://kennethreitz.org) [![Python Software Foundation](https://raw.githubusercontent.com/psf/requests/main/ext/psf.png)](https://www.python.org/psf)
