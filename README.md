# Requests

Requests is the human-friendly HTTP library for Python that lets you reason about HTTP the same way you reason about Python objects. Instead of wrestling with low-level sockets or verbose stdlib modules, you compose readable code that mirrors how people talk about the web.

Teams use Requests to ship microservices, automate operational runbooks, sync SaaS data, and power CLI tools. Backend engineers rely on its robust connection management, operations teams appreciate the rich debugging hooks, and data scientists value the intuitive JSON helpers when iterating in notebooks.

The library wraps HTTP concepts in clear abstractions - sessions, responses, headers, cookies - so you can focus on business logic. That philosophy has made Requests one of the most-downloaded Python packages in the ecosystem, with millions of weekly installs and thousands of open-source projects depending on it.

[![Downloads](https://static.pepy.tech/badge/requests/month)](https://pepy.tech/project/requests)
[![Supported Versions](https://img.shields.io/pypi/pyversions/requests.svg)](https://pypi.org/project/requests)
[![Contributors](https://img.shields.io/github/contributors/psf/requests.svg)](https://github.com/psf/requests/graphs/contributors)

## Annotated Quickstart

Follow this guided example to issue an authenticated request, inspect the response, and decode JSON safely.

1. **Prepare an authenticated session.** Sessions let you persist headers, authentication, and connection pooling across requests.

   ```python
   import requests

   session = requests.Session()
   session.auth = ("user", "pass")
   ```

2. **Send the request with sensible timeouts.** Timeouts keep your application responsive even when an upstream service stalls.

   ```python
   response = session.get(
       "https://httpbin.org/basic-auth/user/pass",
       timeout=5,
   )
   response.raise_for_status()  # raises HTTPError on 4xx/5xx
   ```

3. **Inspect HTTP metadata and decode the payload.** Every `Response` keeps the original request, headers, encoding, and helper methods for various content types.

   ```python
   print(response.status_code)           # 200
   print(response.headers["content-type"])  # application/json; charset=utf-8
   data = response.json()
   ```

   For APIs that return non-JSON payloads, use `response.text` or `response.iter_content()` to stream data without loading everything into memory. When you need alternative authentication (bearer tokens, client certificates, or custom headers), update the session once and reuse it across your codebase.

## Environment Setup and Installation

Production deployments benefit from isolating dependencies and being explicit about networking constraints.

- **Create an isolated environment.**

  ```console
  $ python -m venv .venv
  $ source .venv/bin/activate  # Windows: .venv\Scripts\activate
  $ python -m pip install --upgrade pip
  ```

- **Install Requests for your workflow.**

  ```console
  # Direct install for applications or notebooks
  $ python -m pip install requests

  # Pin a version for reproducible builds
  $ python -m pip install "requests==2.32.*"

  # From a requirements file used in CI/CD
  $ python -m pip install -r requirements.txt
  ```

  Prefer tools such as `pipenv`, Poetry, or `pipx` when you want to manage isolated environments automatically or install command-line helpers that depend on Requests.

- **Work with corporate proxies and certificates.** Export `HTTP_PROXY`/`HTTPS_PROXY`, point `REQUESTS_CA_BUNDLE` at your internal CA bundle, or configure per-request proxies as described in the [proxy guide](https://requests.readthedocs.io/en/latest/user/advanced/#proxies).

- **Verify the installation.**

  ```console
  $ python -m requests --version
  ```

Requests officially supports Python 3.9 and newer.

## Capabilities for Production Use

### Connection Management

Persistent sessions reuse TCP connections, reducing latency across microservices and reducing load on upstream APIs. Features include connection pooling, streaming downloads, chunked uploads, configurable retries via adapters, and per-request timeouts. See the [advanced usage guide](https://requests.readthedocs.io/en/latest/user/advanced/) for details.

### Authentication and Security

Requests supports browser-style TLS verification, `.netrc` credentials, and helpers for Basic, Digest, token-based, and client-certificate authentication. Combine sessions with environment variables to rotate credentials safely. The [authentication documentation](https://requests.readthedocs.io/en/latest/user/authentication/) outlines each pattern.

### Content Handling

Automatic content decompression, encoding detection, and helper methods (`json()`, `iter_content()`, multipart form uploads) make it easy to consume APIs and file streams. The [Request and Response guide](https://requests.readthedocs.io/en/latest/user/quickstart/#response-content) covers each helper with examples.

### Developer Ergonomics

APIs mirror familiar Python containers - headers and cookies behave like dictionaries, sessions expose a clean context-manager interface, and adapters let you plug in custom transports such as SOCKS proxies. That consistency keeps CLI scripts, web backends, and data notebooks approachable for mixed-experience teams.

## Releases and Deployment Readiness

- Track official release notes in [`HISTORY.md`](HISTORY.md) or on the [Requests release feed](https://requests.readthedocs.io/en/latest/community/updates/). Stable releases follow semantic versioning, and patch releases focus on bug fixes and security updates.
- Pin the package in `requirements.txt`, Poetry `pyproject.toml`, or Conda environment files to control upgrade cadence. For example: `requests==2.32.3` or `requests>=2.32,<3`.
- Roll out upgrades with a lightweight checklist: refresh a staging environment, rerun smoke tests that cover authentication and streaming downloads, confirm TLS defaults with your observability tooling, and monitor timeouts during the first production deploy.

## Troubleshooting Playbook

- **Timeouts or hanging connections:** Add explicit `timeout=` values and inspect upstream latency. Enable debug logging with:

  ```python
  import logging
  logging.basicConfig(level=logging.DEBUG)
  logging.getLogger("urllib3").setLevel(logging.DEBUG)
  ```

- **Proxy or firewall issues:** Double-check environment variables, confirm DNS resolution from the host, and review the [proxy section](https://requests.readthedocs.io/en/latest/user/advanced/#proxies) for authenticated proxies.
- **SSL certificate warnings:** Point `REQUESTS_CA_BUNDLE` to your certificate bundle or install trust stores via your operating system. The [TLS documentation](https://requests.readthedocs.io/en/latest/user/advanced/#ssl-cert-verification) lists options for client certificates and custom verification callbacks.
- **Unexpected response payloads:** Inspect `response.headers` and `response.text` to ensure the server advertises the correct encoding, then fall back to `response.content` with manual decoding when necessary.
- **Need a full diagnostic snapshot?** Run `python -m requests.help` to capture environment details when filing a bug report.

## Documentation Map and Contribution Path

- **Quickstart and tutorials:** Begin with the [user guide](https://requests.readthedocs.io/en/latest/user/quickstart/) for end-to-end scenarios.
- **Advanced topics for operators:** Explore [advanced usage](https://requests.readthedocs.io/en/latest/user/advanced/) for adapters, streaming, and custom transports, and review the [release process](https://requests.readthedocs.io/en/latest/community/release-process/) when planning production upgrades.
- **API surface area:** The [API reference](https://requests.readthedocs.io/en/latest/api/) provides every function, class, and exception signature with links into the source.
- **Contributing fixes or features:** Follow the steps in [`docs/dev/contributing.rst`](docs/dev/contributing.rst) to set up a development environment, run tests, and submit pull requests.

## Cloning the Repository

When cloning the Requests repository, you may need to add the `-c fetch.fsck.badTimezone=ignore` flag to avoid an error about a bad commit timestamp (see [this issue](https://github.com/psf/requests/issues/2690) for more background):

```console
$ git clone -c fetch.fsck.badTimezone=ignore https://github.com/psf/requests.git
```

You can also apply this setting to your global Git config:

```console
$ git config --global fetch.fsck.badTimezone ignore
```

---

[![Kenneth Reitz](https://raw.githubusercontent.com/psf/requests/main/ext/kr.png)](https://kennethreitz.org) [![Python Software Foundation](https://raw.githubusercontent.com/psf/requests/main/ext/psf.png)](https://www.python.org/psf)
