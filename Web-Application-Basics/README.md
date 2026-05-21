# Web Application Basics

![Status](https://img.shields.io/badge/Status-Completed-brightgreen) ![Difficulty](https://img.shields.io/badge/Difficulty-Easy-green) ![Path](https://img.shields.io/badge/Path-Cyber%20Security%20101-blue)

| Field | Value |
|---|---|
| **Path** | Cyber Security 101 → Web Hacking |
| **Difficulty** | Easy |
| **Duration** | 120 minutes (official) |
| **Date Completed** | 22 May 2026 |
| **Room Link** | [TryHackMe — Web Application Basics](https://tryhackme.com/room/webapplicationbasics) |

---

## 📖 Overview

This room covers the foundations of how web applications work — from URL anatomy through HTTP requests, responses, status codes, headers, and basic web security headers. Essential knowledge for any pentester, since virtually every modern attack surface involves HTTP.

Topics covered:

- Web application architecture (front end vs back end)
- URL structure and components
- HTTP message format (start line, headers, body)
- HTTP request methods (GET, POST, PUT, DELETE, PATCH, etc.)
- HTTP request headers and body formats
- HTTP response status codes (1xx–5xx)
- HTTP response headers
- Security headers (CSP, HSTS, X-Content-Type-Options, Referrer-Policy)
- Practical: crafting HTTP requests against a live emulator

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| TryHackMe HTTP Emulator | In-browser request builder for practical task |
| Browser DevTools | Inspecting real-world HTTP requests/responses |
| `curl` | Crafting custom HTTP requests from terminal |
| [securityheaders.com](https://securityheaders.com) | Auditing security header configuration |

---

## 📝 Task Walkthrough

### Task 1 — Introduction

Read-through task outlining the learning objectives:
- Understand web application architecture
- Break down URL components
- Learn how HTTP requests/responses work
- Recognise HTTP methods and status codes
- Understand security headers

✅ **Answer:** *I am ready to learn about Web Applications!*

---

### Task 2 — Web Application Overview

**Key concept:** Web apps are split into the **Front End** (what the user sees and interacts with in the browser) and the **Back End** (the infrastructure that powers it).

**Front End components:**

| Component | Purpose |
|---|---|
| **HTML** | Structure of the page — the "DNA" |
| **CSS** | Visual styling — colours, layout, typography |
| **JavaScript** | Logic and interactivity in the browser |

**Back End components:**

| Component | Purpose |
|---|---|
| **Web Server** | Hosts and delivers content |
| **Database** | Stores, modifies, and retrieves data |
| **Infrastructure** | Networking, app servers, storage |
| **WAF (Web Application Firewall)** | Filters malicious traffic (optional but recommended) |

**💡 Why this matters for pentesting:** Each layer has its own attack surface:
- Front End → XSS, DOM-based attacks, client-side validation bypass
- Back End → SQL injection, command injection, SSRF, authentication flaws
- WAF → bypass techniques, evasion payloads

**Questions answered:**

| # | Question | Answer |
|---|---|---|
| 1 | Which component is responsible for hosting and delivering content for web applications? | `web server` |
| 2 | Which tool is used to access and interact with web applications? | `web browser` |
| 3 | Which component acts as a protective layer, filtering incoming traffic to block malicious attacks? | `web application firewall` |

---

### Task 3 — Uniform Resource Locator (URL)

**Key concept:** A URL is structured into distinct components — understanding each is essential for both web dev and exploitation.

**URL anatomy:**

```
https://user:pass@example.com:443/path/to/resource?query=value#fragment
└─┬─┘   └───┬───┘ └────┬────┘ └┬┘ └──────┬──────┘ └─────┬─────┘ └───┬───┘
Scheme    User      Host      Port     Path           Query     Fragment
```

| Component | Example | Purpose |
|---|---|---|
| **Scheme** | `https://` | Protocol (HTTP/HTTPS) |
| **User** | `user:pass@` | Embedded credentials (rare, insecure) |
| **Host/Domain** | `example.com` | Server identifier |
| **Port** | `:443` | Service port (80=HTTP, 443=HTTPS) |
| **Path** | `/path/to/resource` | Specific resource on the server |
| **Query String** | `?query=value` | Parameters (after `?`) |
| **Fragment** | `#fragment` | Section anchor (after `#`) |

**💡 Security relevance:**

- **Typosquatting** — fake domains like `goggle.com` or `paypa1.com` for phishing
- **Embedded credentials** in URLs leak in logs and browser history
- **Query string manipulation** is a primary attack vector (SQLi, XSS, IDOR)
- **Path traversal** (`../../../etc/passwd`) abuses the path component

**Questions answered:**

| # | Question | Answer |
|---|---|---|
| 1 | Which protocol provides encrypted communication between browser and server? | `HTTPS` |
| 2 | What term describes registering misspelt variations of popular domains? | `Typosquatting` |
| 3 | What part of a URL passes additional info like search terms or form inputs? | `Query String` |

---

### Task 4 — HTTP Messages

**Key concept:** HTTP messages are the packets exchanged between client (browser) and server. There are two types: **Requests** (client → server) and **Responses** (server → client).

**Message structure (both types):**

```
Start Line               ← Identifies the message type
Headers                  ← Key-value metadata
[empty line]             ← Separator (CRITICAL — missing this breaks parsing)
Body                     ← Actual data payload (optional)
```

**Example request:**
```
POST /api/login HTTP/1.1          ← Start Line
Host: example.com                 ← Header
Content-Type: application/json    ← Header
Content-Length: 38                ← Header
                                  ← Empty line (essential)
{"username":"admin","password":"x"}   ← Body
```

**💡 Why this matters:** When manually crafting requests (in Burp Suite, custom scripts, or pentest tools), forgetting the empty line between headers and body breaks the request entirely. The server can't parse where headers end and body begins. *(I learned this the hard way during the Task 10 practical!)*

**Questions answered:**

| # | Question | Answer |
|---|---|---|
| 1 | Which HTTP message is returned by the web server after processing a client's request? | `HTTP Response` |
| 2 | What follows the headers in an HTTP message? | `Body` |

---

### Task 5 — HTTP Request: Request Line and Methods

**Key concept:** The **Request Line** is the first line of every HTTP request. Format:

```
METHOD /path HTTP/version
```

Example: `GET /api/users HTTP/1.1`

**HTTP Methods cheatsheet:**

| Method | Purpose | Pentest Relevance |
|---|---|---|
| `GET` | Fetch data | Sensitive data in URL leaks to logs |
| `POST` | Create/submit data | Primary target for SQLi, XSS, CSRF |
| `PUT` | Replace/update | Check auth before allowing changes |
| `DELETE` | Remove resource | Should require strong authorisation |
| `PATCH` | Partial update | Easy to miss validation gaps |
| `HEAD` | Headers only (no body) | Great for recon — fast metadata checks |
| `OPTIONS` | Lists allowed methods | Identifies exposed methods (often forgotten) |
| `TRACE` | Echoes request (debug) | Should be **disabled** — XST attacks |
| `CONNECT` | Tunnel (used for HTTPS proxies) | Sometimes abused for SSRF |

**HTTP versions:**

| Version | Year | Notable Features |
|---|---|---|
| HTTP/0.9 | 1991 | GET only |
| HTTP/1.0 | 1996 | Headers, multiple content types |
| **HTTP/1.1** | 1997 | **Persistent connections, chunked encoding (still dominant)** |
| HTTP/2 | 2015 | Multiplexing, header compression |
| HTTP/3 | 2022 | Built on QUIC, faster, more secure |

**💡 Recon tip:** Always test `OPTIONS` on every endpoint during a pentest:
```bash
curl -X OPTIONS https://target.com/api/users -i
```
The `Allow:` header in the response reveals every method the server accepts — including ones developers forgot were enabled (like `PUT` or `DELETE` on a "read-only" API).

**Questions answered:**

| # | Question | Answer |
|---|---|---|
| 1 | Which HTTP protocol version remains the most commonly used? | `HTTP/1.1` |
| 2 | Which HTTP method describes the communication options for the target resource? | `OPTIONS` |
| 3 | Which component specifies the specific resource or endpoint? | `URL Path` |

---

### Task 6 — HTTP Request: Headers and Body

**Key concept:** Headers carry metadata about the request; the body carries the actual payload (for POST/PUT/PATCH).

**Common request headers:**

| Header | Example | Purpose |
|---|---|---|
| `Host` | `tryhackme.com` | Target server (required in HTTP/1.1) |
| `User-Agent` | `Mozilla/5.0` | Identifies the browser/client |
| `Referer` | `https://google.com/` | Where the request came from |
| `Cookie` | `sessionId=abc123` | Stored session data |
| `Content-Type` | `application/json` | Format of the request body |

**Request body formats:**

**1. URL Encoded (`application/x-www-form-urlencoded`)** — default for HTML forms
```
name=Aleksandra&age=27&country=US
```

**2. Multipart Form Data (`multipart/form-data`)** — for file uploads
```
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="profile_pic"; filename="pic.jpg"
Content-Type: image/jpeg

[binary data]
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

**3. JSON (`application/json`)** — most common for APIs
```json
{
  "name": "Aleksandra",
  "age": 27,
  "country": "US"
}
```

**4. XML (`application/xml`)** — legacy and SOAP APIs
```xml
<user>
  <name>Aleksandra</name>
  <age>27</age>
  <country>US</country>
</user>
```

**💡 Pentest gold:** Headers are prime injection points:
- **Host header injection** → cache poisoning, password reset poisoning
- **User-Agent SQLi** → if logged into a DB
- **Referer-based auth bypass** → some apps trust the referer
- **Cookie tampering** → privilege escalation
- **XML body** → XXE (XML External Entity) attacks
- **JSON body** → NoSQL injection, prototype pollution

**Questions answered:**

| # | Question | Answer |
|---|---|---|
| 1 | Which HTTP request header specifies the domain name of the web server? | `Host` |
| 2 | Default content type for form submissions (key=value pairs)? | `application/x-www-form-urlencoded` |
| 3 | Which part of an HTTP request contains additional info like host, user agent, content type? | `Request Headers` |

---

### Task 7 — HTTP Response: Status Line and Status Codes

**Key concept:** The **Status Line** is the first line of an HTTP response. Format:

```
HTTP/version StatusCode ReasonPhrase
```

Example: `HTTP/1.1 200 OK`

**Status code categories:**

| Range | Category | Meaning |
|---|---|---|
| **1xx** | Informational | Server received part of request, waiting for more |
| **2xx** | Success | Request worked — server returned data |
| **3xx** | Redirection | Resource moved — follow the new URL |
| **4xx** | Client Error | Your request is broken (wrong URL, missing auth, etc.) |
| **5xx** | Server Error | Server crashed trying to handle the request |

**Common codes to memorise:**

| Code | Reason | When You'll See It |
|---|---|---|
| `100` | Continue | Server got headers, send the rest |
| `200` | OK | Standard success |
| `201` | Created | POST succeeded, resource created |
| `301` | Moved Permanently | Redirect to new URL forever |
| `302` | Found | Temporary redirect |
| `400` | Bad Request | Malformed request |
| `401` | Unauthorized | Missing/bad authentication |
| `403` | Forbidden | Authenticated but not allowed |
| `404` | Not Found | Resource doesn't exist |
| `405` | Method Not Allowed | Right URL, wrong HTTP method |
| `500` | Internal Server Error | Server crashed |
| `502` | Bad Gateway | Upstream server failed |
| `503` | Service Unavailable | Server temporarily down |

**💡 Pentest tip:** Status codes reveal a lot during recon. A `403` tells you a resource exists but you can't access it (worth attacking). A `401` tells you auth is required (try default creds, brute force). A `500` often leaks stack traces with sensitive info — *always* check the response body.

**Questions answered:**

| # | Question | Answer |
|---|---|---|
| 1 | What part of an HTTP response provides version, status code, and explanation? | `Status Line` |
| 2 | Which category indicates the web server encountered an internal issue? | `Server Error Responses` |
| 3 | Which HTTP status code indicates the requested resource could not be found? | `404` |

---

### Task 8 — HTTP Response: Headers and Body

**Key concept:** Response headers are metadata sent back by the server; the body is the actual content (HTML, JSON, image, etc.).

**Required response headers:**

| Header | Example | Purpose |
|---|---|---|
| `Date` | `Fri, 23 Aug 2024 10:43:21 GMT` | When the response was generated |
| `Content-Type` | `text/html; charset=utf-8` | Format of the response body |
| `Server` | `nginx` | ⚠️ Server software (often leaked info) |

**Other common response headers:**

| Header | Example | Purpose |
|---|---|---|
| `Set-Cookie` | `sessionId=38af1337es7a8; HttpOnly; Secure` | Tells client to store a cookie |
| `Cache-Control` | `max-age=600` | How long client can cache the response |
| `Location` | `/index.html` | Where to redirect (used with 3xx codes) |

**🔒 Cookie security flags:**

| Flag | Effect |
|---|---|
| `HttpOnly` | Cookie can't be read by JavaScript → mitigates XSS theft |
| `Secure` | Cookie only sent over HTTPS → prevents MITM theft |
| `SameSite` | Restricts cross-site sending → mitigates CSRF |

**💡 Real-world tie-in:** I ran [securityheaders.com](https://securityheaders.com) against my own production WordPress site (`metalrecyclingms.com.au`) during this room and discovered:
- `Server: BunnyCDN-IE1-1085` (CDN identified)
- `x-turbo-charged-by: LiteSpeed` (origin server leaked)
- Missing: HSTS, CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy

The site scored an **F**. Then I fixed it using the HTTP Headers WordPress plugin to score an **A**. This room directly translated into real-world security improvements on a live business website.

**Questions answered:**

| # | Question | Answer |
|---|---|---|
| 1 | Which HTTP response header can reveal info about the web server's software and version? | `Server` |
| 2 | Which flag ensures cookies are only transmitted over HTTPS? | `Secure` |
| 3 | Which flag prevents cookies from being accessed via JavaScript? | `HttpOnly` |

---

### Task 9 — Security Headers

**Key concept:** Security headers are HTTP response headers that instruct the browser to enforce extra protections — defending against XSS, clickjacking, MITM, MIME sniffing, and more.

**Key security headers:**

#### 🛡️ Content-Security-Policy (CSP)
Whitelist of allowed content sources. Mitigates XSS.

```
Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.tryhackme.com; style-src 'self'
```

| Directive | Purpose |
|---|---|
| `default-src` | Fallback policy for all resource types |
| `script-src` | Allowed sources for JavaScript |
| `style-src` | Allowed sources for CSS |
| `img-src` | Allowed sources for images |
| `'self'` | Same origin as the page |
| `'unsafe-inline'` | Allow inline scripts (⚠️ weakens protection) |

#### 🔒 Strict-Transport-Security (HSTS)
Forces all future connections over HTTPS.

```
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

| Directive | Purpose |
|---|---|
| `max-age` | Duration in seconds |
| `includeSubDomains` | Apply to all subdomains |
| `preload` | Browser preload list (⚠️ hard to undo) |

#### 🚫 X-Content-Type-Options
Stops MIME sniffing.

```
X-Content-Type-Options: nosniff
```

#### 🔗 Referrer-Policy
Controls what info is leaked in the `Referer` header.

| Value | Behaviour |
|---|---|
| `no-referrer` | Nothing sent |
| `same-origin` | Only sent within the same site |
| `strict-origin` | Origin only, and only HTTPS→HTTPS |
| `strict-origin-when-cross-origin` | Full URL same-site, origin only cross-site (recommended) |

**💡 Real-world application:** I implemented all of these on `metalrecyclingms.com.au` using the WordPress "HTTP Headers" plugin during this room. The site went from **F → A** on securityheaders.com.

**Questions answered:**

| # | Question | Answer |
|---|---|---|
| 1 | In CSP, which property defines where scripts can be loaded from? | `script-src` |
| 2 | Which HSTS directive applies the policy to subdomains? | `includeSubDomains` |
| 3 | Which directive prevents browsers from interpreting files as a different MIME type? | `nosniff` |

---

### Task 10 — Practical: Making HTTP Requests 🎯

**Key concept:** Hands-on task using the in-browser HTTP request emulator. Crafted real GET, POST, and DELETE requests against a mock API.

**Method 1 — Using the THM emulator:**

| Request | Method | Path | Parameters |
|---|---|---|---|
| Fetch all users | `GET` | `/api/users` | *(none)* |
| Update Bob's country | `POST` | `/api/user/2` | `country=US` |
| Delete user 1 | `DELETE` | `/api/user/1` | *(none)* |

**Method 2 — Equivalent curl commands:**

```bash
# GET — fetch all users
curl -X GET http://target/api/users

# POST — update Bob's country from UK to US
curl -X POST http://target/api/user/2 \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "country=US"

# DELETE — remove user 1
curl -X DELETE http://target/api/user/1
```

**⚠️ Gotcha I hit during this task:**
My initial requests were returning `404 Not Found`. The reasons:
1. Missing leading `/` in the path (`api/user/2` instead of `/api/user/2`)
2. `Content-Length: 0` meant the parameter wasn't actually attached to the request body — I had to make sure I clicked **+ Add** in the parameters section to commit the key-value pair before pressing Go.

**Questions answered:**

| # | Question | Answer |
|---|---|---|
| 1 | Make a GET request to `/api/users`. What is the flag? | `[FILL_IN_FLAG_HERE]` |
| 2 | Make a POST request to `/api/user/2` and update Bob's country to US. What is the flag? | `[FILL_IN_FLAG_HERE]` |
| 3 | Make a DELETE request to `/api/user/1`. What is the flag? | `[FILL_IN_FLAG_HERE]` |

---

### Task 11 — Conclusion

Read-through summary. Now confident with:
- Web application architecture and components
- URL anatomy and security implications
- HTTP message structure (request/response)
- HTTP methods and when to use each
- Status codes and what they reveal
- Headers (both request and response)
- Security headers and how they defend against common attacks
- Practical: crafting GET, POST, and DELETE requests by hand

✅ **Answer:** *Completed the room*

---

## 🎯 Key Takeaways

- **HTTP is the foundation of web pentesting.** Almost every web exploit is an unexpected HTTP request the server didn't anticipate.
- **The empty line between headers and body is mandatory.** Forgetting it breaks the entire request.
- **Headers are injection points too.** Host header injection, User-Agent SQLi, Referer-based auth bypass — all real attack vectors.
- **Cookie security flags matter.** `HttpOnly + Secure + SameSite` should be the default on every session cookie.
- **Security headers are free wins.** Adding HSTS, CSP, X-Content-Type-Options, and Referrer-Policy takes minutes and dramatically improves posture.
- **The `Server` header leaks info.** Always strip or obfuscate it in production.
- **`OPTIONS` requests during recon** reveal forgotten endpoints and methods (like `PUT`/`DELETE` on read-only APIs).
- **Status codes are intelligence.** `403` = exists but blocked (attack it), `401` = needs auth (brute force it), `500` = often leaks errors.

---

## ⚠️ Gotchas / Lessons Learned

- **Leading `/` in paths is required.** `POST api/user/2` → 404. `POST /api/user/2` → works.
- **`Content-Length: 0` means your body didn't attach.** Common mistake when using request builders that need an explicit "+ Add" click on parameters.
- **`application/x-www-form-urlencoded` ≠ `application/json`.** Some APIs accept only one. Always check `Content-Type`.
- **Different request builders format bodies differently.** THM emulator uses form-encoded; Burp Repeater lets you choose; `curl` defaults to form-encoded with `-d`.

---

## 🌍 Real-World Application

I applied the knowledge from Task 8 and Task 9 directly to my own production website (`metalrecyclingms.com.au`):

**Before this room:**
- Site grade on [securityheaders.com](https://securityheaders.com): **F**
- Missing every modern security header
- Server software (`LiteSpeed`) exposed via `x-turbo-charged-by` header

**Actions taken:**
- Installed the "HTTP Headers" WordPress plugin
- Configured `Strict-Transport-Security`, `X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy`, and `Permissions-Policy`
- Removed `x-turbo-charged-by` server fingerprint header
- Fixed the admin user display name to prevent username enumeration

**After:**
- Site grade: **A**
- Server fingerprinting reduced
- User enumeration vector closed

---

## 🛡️ MITRE ATT&CK Mapping

| Technique | ID | Relevance |
|---|---|---|
| Gather Victim Network Info: DNS | T1590.002 | URL/domain reconnaissance |
| Application Layer Protocol: Web Protocols | T1071.001 | HTTP is the foundation |
| Active Scanning: Vulnerability Scanning | T1595.002 | Status codes guide vuln discovery |
| Exploit Public-Facing Application | T1190 | All HTTP-based web exploits |

---

## 🔗 References

- [TryHackMe — Web Application Basics](https://tryhackme.com/room/webapplicationbasics)
- [MDN — HTTP Documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP)
- [OWASP — HTTP Security Headers](https://owasp.org/www-project-secure-headers/)
- [securityheaders.com — Header Scanner](https://securityheaders.com)
- [RFC 7230 — HTTP/1.1 Message Syntax](https://www.rfc-editor.org/rfc/rfc7230)
- [HTTP Status Code Reference (httpstatuses.com)](https://httpstatuses.com)

---

**Completed:** 22 May 2026  
**Previous room:** Metasploit: Exploitation  
**Next room:** *(continuing Cyber Security 101 path)*
