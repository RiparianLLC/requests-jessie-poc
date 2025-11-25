# Requests

[![Downloads](https://static.pepy.tech/badge/requests/month)](https://pepy.tech/project/requests)
[![Supported Versions](https://img.shields.io/pypi/pyversions/requests.svg)](https://pypi.org/project/requests)
[![Contributors](https://img.shields.io/github/contributors/psf/requests.svg)](https://github.com/psf/requests/graphs/contributors)

**Requests** is a simple, yet incredibly powerful HTTP library for Python that
prioritizes developer ergonomics without sacrificing correctness. The project
has been continuously refined for over a decade, serves billions of API calls
across organizations of every size, and is depended on by over **1,000,000**
public repositories. With roughly **30 million downloads per week**, Requests is
the de-facto standard for speaking HTTP in Python.

Requests lets you interact with HTTP/1.1 services with a clean, intuitive API.
Instead of manually constructing query strings, juggling transient sockets,
wrangling manual redirects, or decoding gzip-compressed payloads, you can focus
entirely on the business logic of your application. Under the hood, Requests
handles redirects, connection pooling, TLS verification, cookies, proxies,
timeouts, streaming bodies, and a multitude of other HTTP intricacies so you
don’t have to.

```python
import requests

def fetch_secure_resource():
    response = requests.get(
        "https://httpbin.org/basic-auth/user/pass",
        auth=("user", "pass"),
        timeout=5,
    )
    response.raise_for_status()  # immediately surface unexpected status codes
    assert response.headers["content-type"] == "application/json; charset=utf8"
    data = response.json()
    if not data.get("authenticated"):
        raise RuntimeError("Unexpected authentication response")
    return data
```

## Table of Contents

- [Why Requests?](#why-requests)
- [Installation](#installation)
- [Quickstart](#quickstart)
- [Core Capabilities](#core-capabilities)
- [Working with Sessions](#working-with-sessions)
- [Handling Data Payloads](#handling-data-payloads)
- [Streaming, Timeouts & Retries](#streaming-timeouts--retries)
- [Authentication & Security](#authentication--security)
- [Environment & Proxy Configuration](#environment--proxy-configuration)
- [Debugging & Observability](#debugging--observability)
- [Compatibility Notes](#compatibility-notes)
- [Documentation & Resources](#documentation--resources)
- [Contributing](#contributing)
- [License](#license)

## Why Requests?

- **Human-friendly design:** Method names match HTTP verbs, keyword arguments
  read like configuration files, and responses expose ergonomic helpers such as
  `.json()` for parsing JSON payloads.
- **Battle tested in production:** Requests is embedded inside web frameworks,
  data pipelines, CLI tooling, network automation stacks, and even small IoT
  devices. The library ships with a battery of interoperability tests that run
  against real HTTP services.
- **Extensible without boilerplate:** Sessions, hooks, adapters, and transport
  level configuration allow you to customize behavior to match advanced
  requirements while keeping your code compact.
- **Standards compliant:** Requests adheres to RFC-defined behavior wherever
  possible. TLS verification is on by default, cookies honor public suffix
  rules, and the project tracks new RFCs as they emerge.
- **Community supported:** Maintained by the Python Software Foundation,
  Requests benefits from robust governance, accessible documentation, and an
  active contributor base ready to help.

## Installation

Requests is published on [PyPI](https://pypi.org/project/requests/). Install it
with `pip` (shown with explicit interpreter invocation to avoid path surprises):

```console
$ python -m pip install requests
```

We recommend working inside a [virtual environment](https://docs.python.org/3/library/venv.html)
to isolate dependencies per project:

```console
$ python -m venv .venv
$ source .venv/bin/activate
(.venv) $ python -m pip install --upgrade pip
(.venv) $ python -m pip install requests
```

### Verifying your installation

```python
import requests

r = requests.get("https://httpbin.org/get", timeout=5)
r.raise_for_status()
print(r.json()["headers"]["User-Agent"])
```

If you see the default `python-requests/X.Y.Z` user agent printed, your
installation is complete and outbound HTTPS traffic functions correctly from
your environment.

## Quickstart

Requests exposes convenience methods for the most common HTTP verbs.

```python
import requests

payload = {"search": "python", "limit": 5}
headers = {"Accept": "application/json"}
r = requests.get("https://httpbin.org/get", params=payload, headers=headers, timeout=10)
r.raise_for_status()

print("Status:", r.status_code)
print("Sent parameters:", r.json()["args"])
```

- `params` automatically encodes query string parameters.
- `headers` merges with sensible defaults so you can override the pieces you
  care about.
- `timeout` protects your application from hanging indefinitely by bounding both
  the connect and read phases unless you pass a tuple for fine-grained control.
- `raise_for_status()` surfaces unexpected responses early, making debugging and
  logging far easier.

## Core Capabilities

Requests packs features that line up with real-world HTTP client needs:

- Keep-Alive connection pooling for efficient reuse of TCP sockets.
- International domain & URL support so punycode and Unicode hosts just work.
- Automatic TLS/SSL verification with certificate bundle management.
- Basic, Digest, and modern authentication flows backed by pluggable auth
  handlers.
- Familiar dictionary-like cookie APIs with persistence across redirects.
- Automatic decompression (gzip, deflate, brotli via optional dependency) and
  decoding based on response headers.
- Multipart file uploads, streaming request bodies, and chunked transfer
  support.
- SOCKS proxy handling via `requests[socks]`.
- Timeout management, including per-request or per-Session defaults.
- Transparent honor of `.netrc` credential helpers when enabled.

## Working with Sessions

Use `requests.Session` to share settings (headers, cookies, proxies) and reap the
benefits of connection pooling:

```python
from requests import Session

with Session() as session:
    session.headers.update({"User-Agent": "myapp/1.0"})
    session.auth = ("api-key", "secret")
    session.params = {"locale": "en_US"}

    for endpoint in ("/profile", "/settings", "/notifications"):
        resp = session.get(f"https://api.example.com{endpoint}", timeout=(3.05, 27))
        resp.raise_for_status()
        print(endpoint, "→", resp.json())
```

- Sessions automatically persist cookies between requests, making stateful
  interactions trivial.
- Adapter mounting lets you direct certain hosts through bespoke transports, for
  example to enable retries or to talk to services over Unix sockets.

## Handling Data Payloads

Requests supports multiple strategies for sending data:

- `data=` for HTML form-encoded content.
- `json=` for automatic JSON serialization (sets the appropriate `Content-Type`
  header for you).
- `files=` for multipart uploads, including large streaming file objects.
- `stream=True` when retrieving large responses you do not want fully resident
  in memory; iterate over `iter_content()` chunks instead.

```python
files = {"avatar": ("profile.png", open("profile.png", "rb"), "image/png")}
r = requests.post("https://httpbin.org/post", files=files, timeout=10)
r.raise_for_status()
```

## Streaming, Timeouts & Retries

- Use `stream=True` to read response bodies lazily:

  ```python
  with requests.get("https://httpbin.org/stream/20", stream=True, timeout=10) as r:
      for line in r.iter_lines():
          print(line)
  ```

- Provide a tuple timeout `(connect_timeout, read_timeout)` for granular control.
- Combine Sessions with [`urllib3.util.retry.Retry`](https://urllib3.readthedocs.io/en/stable/reference/urllib3.util.html#module-urllib3.util.retry)
  via an adapter for automatic retry policies on transient errors.

## Authentication & Security

- Basic authentication is built-in. For OAuth, API keys, or signed requests you
  can supply your own `AuthBase` subclass.
- TLS verification is **enabled by default**. Requests bundles certificates via
  `certifi`. If you need to talk to an internal CA, pass `verify="/path/to/ca.pem"`.
- Client certificates (including key passphrases) are supported with the
  `cert=` argument.
- Redirection limits prevent infinite loops; override `allow_redirects` or
  `max_redirects` in rare circumstances.

## Environment & Proxy Configuration

- Honor standard environment variables such as `HTTP_PROXY`, `HTTPS_PROXY`, and
  `NO_PROXY`.
- Explicitly configure proxies per request or via Sessions:

  ```python
  proxies = {
      "http": "http://10.10.1.10:3128",
      "https": "http://10.10.1.10:1080",
  }
  requests.get("https://httpbin.org/ip", proxies=proxies, timeout=5)
  ```

- If you require SOCKS proxies, install `pip install requests[socks]` to activate
  the optional dependency and pass `socks5://` URLs.

## Debugging & Observability

- Enable logging to inspect requests and responses:

  ```python
  import logging
  import requests

  logging.basicConfig(level=logging.DEBUG)
  http_logger = logging.getLogger("urllib3")
  http_logger.setLevel(logging.DEBUG)
  http_logger.propagate = True

  requests.get("https://httpbin.org/get", timeout=3)
  ```

- Use `response.history` to inspect redirect chains.
- Capture `response.elapsed` to analyze latency.
- Combine with Python’s `traceback` module or your preferred profiler to
  understand performance hotspots.

## Compatibility Notes

- **Supported Python versions:** Officially 3.9 and newer. Older interpreters may
  still function but no longer receive active support or patch releases.
- **Operating systems:** Requests runs anywhere CPython runs—Linux, macOS,
  Windows, FreeBSD, Alpine-based containers, and more.
- **Dependency policy:** Requests aims for minimal runtime dependencies. Optional
  extras are available for SOCKS proxies, security tooling, and certificate
  management.
- **API stability:** We follow semantic versioning. Backwards-incompatible
  changes are introduced only in major version bumps and are accompanied by
  migration notes in the changelog.

## Documentation & Resources

- Complete user guide, advanced recipes, and API reference are published on
  [Read the Docs](https://requests.readthedocs.io).
- Historical release notes live in `HISTORY.md` and highlight bug fixes,
  security advisories, and new features.
- The Requests community maintains a curated list of third-party extensions and
  related tooling on the [wiki](https://github.com/psf/requests/wiki).

![Read the Docs screenshot](https://raw.githubusercontent.com/psf/requests/main/ext/ss.png)

## Authors

- Kenneth Reitz
- Chrissy Steinmeier

## Contributing

1. Fork the repository and clone it locally. If you encounter Git complaints
   about timestamp anomalies, add the `-c fetch.fsck.badTimezone=ignore` flag:

   ```console
   $ git clone -c fetch.fsck.badTimezone=ignore https://github.com/psf/requests.git
   ```

2. Install development dependencies:

   ```console
   $ python -m pip install -r requirements-dev.txt
   $ python -m pip install -e .
   ```

3. Run the test suite before submitting a pull request:

   ```console
   $ python -m pytest
   ```

4. Review our [contributing guidelines](https://github.com/psf/requests/blob/main/CONTRIBUTING.md)
   for coding standards, documentation expectations, and triage workflow.

We welcome bug reports, feature proposals, documentation enhancements, and
performance investigations. Join the discussion in GitHub issues or the Python
Software Foundation community spaces to collaborate on improvements.

## License

Requests is available under the [Apache 2.0 License](LICENSE). By contributing
to the project you agree that your contributions fall under the same license and
acknowledge the [NOTICE](NOTICE) file.

---

[![Kenneth Reitz](https://raw.githubusercontent.com/psf/requests/main/ext/kr.png)](https://kennethreitz.org)
[![Python Software Foundation](https://raw.githubusercontent.com/psf/requests/main/ext/psf.png)](https://www.python.org/psf)
