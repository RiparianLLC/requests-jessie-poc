# Requests

**Requests** is a simple, yet elegant, HTTP library.

If you have ever wrestled with Python's built-in `http.client` or manual socket code, Requests is the ergonomic alternative. It wraps the power of `urllib3` in a clean API so you can focus on the intent of each request instead of the mechanics of authentication headers, SSL options, or payload encoding.

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

In the interactive example above, `requests.get` handles Basic Auth, connection reuse, and response decoding on your behalf—leaving you with simple attribute access for status codes, headers, and JSON payloads. That convenience carries over to POSTs, streaming downloads, and everything in between.

Requests is one of the most downloaded Python packages today, pulling in around `30M downloads / week`— according to GitHub, Requests is currently [depended upon](https://github.com/psf/requests/network/dependents?package_id=UGFja2FnZS01NzA4OTExNg%3D%3D) by `1,000,000+` repositories. You may certainly put your trust in this code. Those numbers translate into a vibrant community, fast issue triage, and wide adoption in tools like `pip`, `pipenv`, and numerous SDKs that rely on Requests for their HTTP layer.

[![Downloads](https://static.pepy.tech/badge/requests/month)](https://pepy.tech/project/requests)
[![Supported Versions](https://img.shields.io/pypi/pyversions/requests.svg)](https://pypi.org/project/requests)
[![Contributors](https://img.shields.io/github/contributors/psf/requests.svg)](https://github.com/psf/requests/graphs/contributors)

## Installing Requests and Supported Versions

Requests is available on PyPI, and installing it takes just a few steps:

1. Confirm your Python interpreter:

   ```console
   $ python --version
   ```

   Requests officially supports Python 3.9+ across CPython, PyPy, and the system Python that ships with most Linux and macOS distributions.

2. (Recommended) Create a virtual environment so your dependencies stay isolated:

   ```console
   $ python -m venv .venv
   $ source .venv/bin/activate
   ```

3. Install Requests with pip:

   ```console
   $ python -m pip install requests
   ```

If you need exact version constraints or wheels for alternative Python versions, refer to the [Supported Versions table](https://requests.readthedocs.io/en/latest/community/faq/#what-versions-of-python-are-supported).

## Supported Features & Best–Practices

Requests is ready for the demands of building robust and reliable HTTP–speaking applications, for the needs of today.

- **Keep-Alive & Connection Pooling** – reuse TCP connections automatically for faster follow-up calls (powered by `urllib3` pooling).
- **International Domains and URLs** – send requests to IDNA domains and percent-encoded paths without manual encoding.
- **Sessions with Cookie Persistence** – maintain stateful conversations; see [Session Objects](https://requests.readthedocs.io/en/latest/user/advanced/#session-objects).
- **Browser-style TLS/SSL Verification** – negotiate secure connections with certificates verified via certifi defaults.
- **Basic & Digest Authentication** – attach credentials easily or plug in custom auth flows.
- **Familiar `dict`-like Cookies** – read and mutate cookies with an interface that mirrors Python dictionaries.
- **Automatic Content Decompression and Decoding** – transparently handle gzip/deflate encodings and character sets.
- **Multi-part File Uploads** – send files and form data with a single call; see [Advanced POST requests](https://requests.readthedocs.io/en/latest/user/advanced/#post-multiple-multipart-encoded-files).
- **SOCKS Proxy Support** – route traffic through SOCKS proxies via `requests[socks]`.
- **Connection Timeouts** – guard against hanging requests with per-call timeouts.
- **Streaming Downloads** – stream large responses incrementally with `iter_content`.
- **Automatic honoring of `.netrc`** – load credentials configured in your local `.netrc`.
- **Chunked HTTP Requests** – upload streaming bodies efficiently with chunked transfer encoding.

## API Reference and User Guide available on [Read the Docs](https://requests.readthedocs.io)

The hosted documentation includes a Quickstart tutorial, in-depth guides on authentication, sessions, proxies, and streaming, plus a full API reference. Start with the Quickstart if you are new, then explore the Advanced topics when you need recipes for retries, hooks, or custom transports.

[![Read the Docs](https://raw.githubusercontent.com/psf/requests/main/ext/ss.png)](https://requests.readthedocs.io)

## Cloning the repository

When cloning the Requests repository, you may need to add the `-c
fetch.fsck.badTimezone=ignore` flag to avoid an error about a bad commit timestamp (see
[this issue](https://github.com/psf/requests/issues/2690) for more background):

Older Git versions can flag historical commits with timestamps recorded in unusual time zones. If you hit that warning, pick one of the approaches below to continue safely.

**One-off clone**

```shell
git clone -c fetch.fsck.badTimezone=ignore https://github.com/psf/requests.git
```

**Global setting**

You can also apply this setting to your global Git config:

```shell
git config --global fetch.fsck.badTimezone ignore
```

## Next Steps

Ready to dig deeper or contribute fixes? Browse `CONTRIBUTING.md` for coding standards and runbooks, file issues with reproducible examples, or join the discussion on the Requests GitHub repository.

---

[![Kenneth Reitz](https://raw.githubusercontent.com/psf/requests/main/ext/kr.png)](https://kennethreitz.org) [![Python Software Foundation](https://raw.githubusercontent.com/psf/requests/main/ext/psf.png)](https://www.python.org/psf)
