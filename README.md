# Requests

**Requests** is a simple, yet elegant, HTTP library.

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

Requests allows you to send HTTP/1.1 requests extremely easily. There’s no need to manually add query strings to your URLs, or to form-encode your `PUT` & `POST` data — but nowadays, just use the `json` method!

Requests is one of the most downloaded Python packages today, pulling in around `30M downloads / week`— according to GitHub, Requests is currently [depended upon](https://github.com/psf/requests/network/dependents?package_id=UGFja2FnZS01NzA4OTExNg%3D%3D) by `1,000,000+` repositories. You may certainly put your trust in this code.

[![Downloads](https://static.pepy.tech/badge/requests/month)](https://pepy.tech/project/requests)
[![Supported Versions](https://img.shields.io/pypi/pyversions/requests.svg)](https://pypi.org/project/requests)
[![Contributors](https://img.shields.io/github/contributors/psf/requests.svg)](https://github.com/psf/requests/graphs/contributors)

## Project Overview

Requests was created to make HTTP calls feel like first-class Python, hiding the complexity of sockets, decoders, and connection management behind a friendly interface. Over the years, it has evolved alongside the Python ecosystem and web standards, all while keeping a human-readable API at its core. Whether you are building command-line utilities, data pipelines, or production web services, the library provides the same dependable experience.

The project is maintained by a team of volunteers working within the Python Software Foundation. Each release goes through community review, automated testing across supported platforms, and rigorous compatibility checks with popular dependencies. If you are curious about how decisions are made, the [project roadmap](https://github.com/psf/requests/projects) and the [issue tracker](https://github.com/psf/requests/issues) are great entry points.

### Philosophy & Design Goals

- **Simplicity first**: Core APIs map cleanly to HTTP verbs and concepts so that your code reads the way you describe it out loud.
- **Robust defaults**: Sensible timeout behavior, TLS verification, and persistent sessions are baked in to reduce the chance of surprises in production.
- **Extensibility**: Hooks, transport adapters, and streaming interfaces let you integrate Requests into everything from microservices to desktop applications.
- **Community driven**: Governance happens in the open, and we rely on a vast contributor base to keep documentation, tests, and features in sync.

### Getting Started Quickly

1. Install via `pip`, `pipx`, or add `requests` to your project’s `pyproject.toml` dependencies.
2. Explore the `requests.get`, `requests.post`, and `requests.Session` APIs, which cover the majority of use cases.
3. Review the [advanced usage guide](https://requests.readthedocs.io/en/latest/user/advanced/) to learn about streaming uploads/downloads, custom authentication, and transport adapters.
4. Run your integration tests with `REQUESTS_CA_BUNDLE` or `CURL_CA_BUNDLE` set when you need to point at custom certificate stores.

If you are upgrading from an earlier release, take a moment to read the changelog in `HISTORY.md`. Each entry highlights new features, deprecations, and any behavioral changes that might require code updates, so you can plan your rollout with confidence.

## Installing Requests and Supported Versions

Requests is available on PyPI:

```console
$ python -m pip install requests
```

Requests officially supports Python 3.9+.

## Supported Features & Best–Practices

Requests is ready for the demands of building robust and reliable HTTP–speaking applications, for the needs of today.

- Keep-Alive & Connection Pooling
- International Domains and URLs
- Sessions with Cookie Persistence
- Browser-style TLS/SSL Verification
- Basic & Digest Authentication
- Familiar `dict`–like Cookies
- Automatic Content Decompression and Decoding
- Multi-part File Uploads
- SOCKS Proxy Support
- Connection Timeouts
- Streaming Downloads
- Automatic honoring of `.netrc`
- Chunked HTTP Requests

## API Reference and User Guide available on [Read the Docs](https://requests.readthedocs.io)

[![Read the Docs](https://raw.githubusercontent.com/psf/requests/main/ext/ss.png)](https://requests.readthedocs.io)

## Cloning the repository

When cloning the Requests repository, you may need to add the `-c
fetch.fsck.badTimezone=ignore` flag to avoid an error about a bad commit timestamp (see
[this issue](https://github.com/psf/requests/issues/2690) for more background):

```shell
git clone -c fetch.fsck.badTimezone=ignore https://github.com/psf/requests.git
```

You can also apply this setting to your global Git config:

```shell
git config --global fetch.fsck.badTimezone ignore
```

---

[![Kenneth Reitz](https://raw.githubusercontent.com/psf/requests/main/ext/kr.png)](https://kennethreitz.org) [![Python Software Foundation](https://raw.githubusercontent.com/psf/requests/main/ext/psf.png)](https://www.python.org/psf)
