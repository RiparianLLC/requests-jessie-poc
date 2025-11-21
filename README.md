# Requests

Requests is the human-friendly HTTP client for Python, letting you send HTTP/1.1 requests with minimal code.

Copy this snippet to make your first request:

```python
import requests

response = requests.get("https://httpbin.org/get", timeout=5)
response.raise_for_status()
print(response.json())
```

## Install

- Python 3.9+ supported.
- Install with `python -m pip install requests`.
- Optional extras: see the install guide in the documentation.

## Dig deeper

- Documentation: https://requests.readthedocs.io
- Release notes: `HISTORY.md`
- Source: https://github.com/psf/requests
- Contributing guide: `CONTRIBUTING.md`
