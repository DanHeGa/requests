# Requests

[![Version](https://img.shields.io/pypi/v/requests.svg?maxAge=86400)](https://pypi.org/project/requests/)
[![Supported Versions](https://img.shields.io/pypi/pyversions/requests.svg)](https://pypi.org/project/requests)
[![Downloads](https://static.pepy.tech/badge/requests/month)](https://pepy.tech/project/requests)
[![Contributors](https://img.shields.io/github/contributors/psf/requests.svg)](https://github.com/psf/requests/graphs/contributors)
[![Documentation](https://readthedocs.org/projects/requests/badge/?version=latest)](https://requests.readthedocs.io)

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

Requests is one of the most downloaded Python packages today, pulling in around `300M downloads / week` — according to GitHub, Requests is currently [depended upon](https://github.com/psf/requests/network/dependents?package_id=UGFja2FnZS01NzA4OTExNg%3D%3D) by `4,000,000+` repositories.

## Installing Requests and Supported Versions

Requests is available on PyPI:

```console
$ python -m pip install requests
```

Requests officially supports Python 3.10+.

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

## AI Coding Interview Practice Ticket

### Feature: Expose the `Retry-After` delay on responses

**Timebox:** 45 minutes
**Difficulty:** Intermediate
**Suggested issue title:** `Add a parsed Retry-After value to Response`

#### Context

APIs sometimes ask clients to wait before sending another request by returning a
`Retry-After` response header. Requests exposes response headers, but callers
currently need to implement their own parsing logic. Add a small, read-only
convenience property so callers can safely inspect the server's suggested delay.

#### Requirements

1. Add a `Response.retry_after` property in `src/requests/models.py`.
2. Return the delay as a non-negative `float` number of seconds, or `None` when:
   - the response has no `Retry-After` header;
   - the header value is malformed; or
   - the header uses an unsupported format.
3. Support both formats allowed by HTTP:
   - **delta-seconds:** a non-negative integer such as `120`;
   - **HTTP-date:** a valid HTTP date such as `Wed, 21 Oct 2015 07:28:00 GMT`.
4. For an HTTP-date in the past, return `0.0`. For a future date, return the
   number of seconds from the current UTC time.
5. Header lookup must remain case-insensitive, consistent with Requests'
   existing response header behavior.
6. The property must not sleep, retry, mutate the response, or make a network
   request. Existing behavior must remain unchanged for all other APIs.

#### Acceptance criteria

- `Response().headers["Retry-After"] = "10"` produces `10.0`.
- A valid future HTTP-date produces a positive delay, allowing a small tolerance
  for clock and test execution time.
- A past HTTP-date produces `0.0`.
- Missing, empty, negative, decimal, and malformed values produce `None`.
- Header-name casing does not affect the result.
- Focused unit tests cover each supported format and the invalid-input cases.
- The existing test suite passes.

#### Suggested implementation surface

- `src/requests/models.py`: add the public property and parsing logic.
- `tests/test_requests.py` or an appropriate existing test module: add focused
  unit tests using constructed `Response` objects; do not rely on an external
  service or real network timing.

#### Out of scope

- Automatically retrying requests.
- Adding a retry policy or backoff strategy.
- Changing adapter behavior or introducing a new dependency.
- Guaranteeing sub-second precision for HTTP-date values.

#### Interview deliverable

Submit the implementation and tests, then briefly explain the parsing decisions,
how the property behaves with invalid input, and how the tests avoid flaky timing
assumptions.

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
