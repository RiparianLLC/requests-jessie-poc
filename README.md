# Requests

**Requests** is a simple, yet elegant HTTP library that makes human-friendly HTTP for the Python community.

| | |
| --- | --- |
| **Install** | `python -m pip install requests` |
| **Docs** | [requests.readthedocs.io](https://requests.readthedocs.io) |
| **Python** | 3.9+ (matches `python_requires` in `setup.py`) |

[![Downloads](https://static.pepy.tech/badge/requests/month)](https://pepy.tech/project/requests)
[![Supported Versions](https://img.shields.io/pypi/pyversions/requests.svg)](https://pypi.org/project/requests)
[![Contributors](https://img.shields.io/github/contributors/psf/requests.svg)](https://github.com/psf/requests/graphs/contributors)

Requests lets you send HTTP/1.1 requests with minimal boilerplate—no manual query string building, no manual form encoding, and plenty of sensible defaults for TLS, sessions, redirects, and more.

## Quickstart

Perform a basic `GET`, validate the response, and parse JSON in just a few lines:

```python
import requests

response = requests.get("https://httpbin.org/get", timeout=5)
response.raise_for_status()
payload = response.json()

print(f"Status: {response.status_code}")
print(f"Origin IP: {payload['origin']}")
```

Add a session when you need connection pooling, shared headers, or authentication:

```python
import requests

with requests.Session() as session:
    session.auth = ("user", "pass")
    session.headers.update({"User-Agent": "example-app/1.0"})

    response = session.get("https://httpbin.org/basic-auth/user/pass", timeout=5)
    response.raise_for_status()

    print(response.json())  # {'authenticated': True, 'user': 'user'}
```

Prefer `response.raise_for_status()` before inspecting content so exceptions surface immediately, and reach for `.json()` to safely decode JSON bodies.

## When You Need…

| Task | Documentation |
| --- | --- |
| A deeper tour of the API | [User Quickstart](https://requests.readthedocs.io/en/latest/user/quickstart/) |
| Handling authentication schemes | [Authentication](https://requests.readthedocs.io/en/latest/user/authentication/) |
| Uploading files and forms | [POST a Multipart-Encoded File](https://requests.readthedocs.io/en/latest/user/advanced/#post-a-multipart-encoded-file) |
| Streaming downloads | [Streaming Requests](https://requests.readthedocs.io/en/latest/user/advanced/#streaming-requests) |
| Working with timeouts and retries | [Advanced Usage](https://requests.readthedocs.io/en/latest/user/advanced/) |

## Key Capabilities

- **Keep-Alive & Connection Pooling** – Sessions reuse sockets automatically for efficient throughput.
- **International Domains & URLs** – Unicode domain names and paths are transparently encoded.
- **Cookie Persistence** – Session cookies behave like browser storage with a familiar dict-style API.
- **TLS/SSL Verification** – Certificate verification and host checking are on by default, with hooks for custom CAs.
- **Authentication Helpers** – Built-in support for Basic, Digest, and pluggable auth flows.
- **Automatic Decoding** – Content encodings such as gzip and deflate are decoded for you.
- **File Uploads & Streaming** – Send and receive large payloads without loading everything into memory.
- **Proxy & SOCKS Support** – Route requests through HTTP and SOCKS proxies with one configuration value.
- **Timeouts & Retries** – Tune networking behavior to match production reliability requirements.
- **`.netrc` Integration** – Respect existing `.netrc` credentials when present.

## Supported Versions

Requests supports Python 3.9 and newer. This matches the `python_requires=">=3.9"` metadata enforced during installation (see `setup.py`). For older Python versions, install a Requests release earlier than 2.32.0.

## Contributing

When cloning the repository, you may need the `-c fetch.fsck.badTimezone=ignore` flag to avoid a Git warning about a historical commit (see [issue #2690](https://github.com/psf/requests/issues/2690)):

```shell
git clone -c fetch.fsck.badTimezone=ignore https://github.com/psf/requests.git
```

To run the test suite, install development dependencies and invoke `pytest`:

```shell
python -m pip install -r requirements-dev.txt
python -m pytest
```

Interested in contributing? Start with the guidelines in [docs/dev/contributing.rst](docs/dev/contributing.rst) and the broader [Requests community resources](https://requests.readthedocs.io/en/latest/community/).

---

[![Read the Docs](https://raw.githubusercontent.com/psf/requests/main/ext/ss.png)](https://requests.readthedocs.io)

[![Kenneth Reitz](https://raw.githubusercontent.com/psf/requests/main/ext/kr.png)](https://kennethreitz.org) [![Python Software Foundation](https://raw.githubusercontent.com/psf/requests/main/ext/psf.png)](https://www.python.org/psf)
