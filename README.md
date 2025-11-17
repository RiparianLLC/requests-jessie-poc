# Requests

**Requests** brings the original "HTTP for Humans" mission to modern Python by wrapping HTTP/1.1 in a friendly, battle-tested API. Instead of juggling headers, query strings, and encodings by hand, you call intuitive methods that already know how to handle the transport details.

## Quick Start

Below is a minimal request against the GitHub API. It checks for transport errors, inspects the JSON body, and surfaces a useful field—all without extra plumbing.

```python
import requests

response = requests.get("https://api.github.com/repos/psf/requests", timeout=10)
response.raise_for_status()

data = response.json()
print(f"{data['full_name']} has {data['stargazers_count']} stars.")
```

Requests keeps connection management, TLS verification, and content decoding out of your way so you can concentrate on the response payload.

Requests remains one of the most widely used Python packages, powering millions of installs each week and hundreds of thousands of open-source projects.

[![PyPI Version](https://img.shields.io/pypi/v/requests.svg)](https://pypi.org/project/requests)
[![Downloads](https://static.pepy.tech/badge/requests/month)](https://pepy.tech/project/requests)
[![Supported Versions](https://img.shields.io/pypi/pyversions/requests.svg)](https://pypi.org/project/requests)
[![License](https://img.shields.io/pypi/l/requests.svg)](https://github.com/psf/requests/blob/main/LICENSE)
[![Contributors](https://img.shields.io/github/contributors/psf/requests.svg)](https://github.com/psf/requests/graphs/contributors)

## Install Requests

Requests is published on PyPI and officially supports Python 3.9+:

```console
$ python -m pip install requests
```

### Getting Started Checklist

1. Create or activate a virtual environment for your project (`python -m venv .venv` and `source .venv/bin/activate` on Unix-likes).
2. Install Requests with `python -m pip install requests`.
3. Verify the install by running the quick-start example or simply checking the version:

   ```python
   >>> import requests
   >>> requests.__version__
   '2.x.y'
   ```

## Core Capabilities

### Resilient networking

- Keep-alive and connection pooling automatically reuse sockets for you.
- Per-request timeouts help you fail fast when services misbehave.
- Streaming and chunked downloads let you process large payloads incrementally.

### Security and authentication

- Browser-style TLS/SSL verification is on by default to keep connections safe.
- Built-in helpers cover Basic and Digest authentication and honor `.netrc`.
- Session objects persist cookies and headers across requests with minimal setup.

### Data ergonomics

- Automatic content decompression and decoding respect response headers and character sets.
- International domains and URLs are handled transparently.
- Familiar dict-like cookies make state management straightforward.

### Integrations and advanced usage

- Multi-part file uploads send binary data and form fields together.
- SOCKS and HTTPS proxy support route traffic through your preferred intermediaries.
- Streaming uploads and chunked requests let you send data without loading entire payloads into memory.

## Documentation Pathways

- **User Guide** – Step-by-step tutorials and patterns for everyday use. Start at https://requests.readthedocs.io/en/latest/user/index.html
- **API Reference** – Detailed documentation for every function, class, and method. Browse https://requests.readthedocs.io/en/latest/api/
- **Community Guide** – Ecosystem highlights, FAQ, release process, and support channels. See https://requests.readthedocs.io/en/latest/community/index.html
- **Contributor Guide** – Development workflow, testing, and authorship details. Read https://requests.readthedocs.io/en/latest/dev/contributing/

[![Read the Docs](https://raw.githubusercontent.com/psf/requests/main/ext/ss.png)](https://requests.readthedocs.io)

## Community & Support

- **Python compatibility** – Requests targets CPython 3.9+ and is continuously tested on PyPy.
- **Release notes** – Track changes in `HISTORY.md` or the updates page at https://requests.readthedocs.io/en/latest/community/updates.html
- **Security** – Report vulnerabilities responsibly via https://requests.readthedocs.io/en/latest/community/vulnerabilities.html
- **Stay informed** – Follow the release process and community guidance in https://requests.readthedocs.io/en/latest/community/release-process.html

## Contributing

Thinking about contributing? We recommend these steps to get started:

- Review open issues and discussions on https://github.com/psf/requests/issues
- Read the Contributor Guide at https://requests.readthedocs.io/en/latest/dev/contributing/
- Clone the repository with the timezone workaround if your Git install needs it:

  ```shell
  git clone -c fetch.fsck.badTimezone=ignore https://github.com/psf/requests.git
  ```

  Apply the same setting globally if you prefer:

  ```shell
  git config --global fetch.fsck.badTimezone ignore
  ```

---

[![Kenneth Reitz](https://raw.githubusercontent.com/psf/requests/main/ext/kr.png)](https://kennethreitz.org) [![Python Software Foundation](https://raw.githubusercontent.com/psf/requests/main/ext/psf.png)](https://www.python.org/psf)
