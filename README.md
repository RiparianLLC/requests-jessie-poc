# Requests

Requests is the simple, human-friendly HTTP library for Python. It wraps the standard library modules so you can send HTTP requests and work with responses in a few readable lines.

## Quick start

```python
import requests

response = requests.get("https://httpbin.org/get", timeout=5)
response.raise_for_status()
print(response.json())
```

## Install

Requests supports Python 3.9+. Install the latest release with:

```console
python -m pip install requests
```

Optional extras are available, for example `python -m pip install "requests[socks]"`.

## Learn more

- Documentation: https://requests.readthedocs.io
- Release notes: `HISTORY.md`
- Source: https://github.com/psf/requests

## Contribute

Open issues, discussions, and pull requests on GitHub. See `CONTRIBUTING.md` for guidelines and tests.
