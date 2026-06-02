# HTTP Server Implementation — Network Programming Task 4

> Pemrograman Jaringan (Network Programming) · ITS Surabaya · 2025

Implementation of an HTTP server from scratch in Python, comparing **7 different concurrency models** for handling concurrent client connections. Includes SSL/TLS support, a socket proxy, and performance benchmarking.

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![HTTP](https://img.shields.io/badge/HTTP%2F1.1-005C97?style=flat&logo=http&logoColor=white)
![Socket](https://img.shields.io/badge/Socket_Programming-3DDC84?style=flat)

---

## Overview

This project explores the core concepts of network programming by building an HTTP/1.1-compliant server using Python's raw socket API — without relying on high-level web frameworks. The same server logic is implemented across seven concurrency architectures, allowing direct performance comparison of each approach.

The server handles GET requests, serves static files (HTML, images, text), and responds with proper HTTP status codes as defined in [RFC 2616](https://www.rfc-editor.org/rfc/rfc2616).

---

## Server Implementations

| File | Concurrency Model | Description |
|---|---|---|
| `server_thread_http.py` | Thread per client | Spawns a new thread for each incoming connection |
| `server_thread_pool_http.py` | Thread pool | Uses a fixed pool of reusable threads |
| `server_thread_http_secure.py` | Thread + SSL/TLS | Threaded server with HTTPS support via SSL certificates |
| `server_process_http.py` | Process per client | Spawns a new process for each incoming connection |
| `server_process_pool_http.py` | Process pool | Uses a fixed pool of reusable processes |
| `server_async_http.py` | Async (callback) | Event-loop based, non-blocking I/O |
| `server_asyncio_stream_http.py` | Asyncio streams | Using Python's `asyncio` stream API |

All servers share the same HTTP parsing logic via `http.py` and listen on port **8889** by default.

---

## Project Structure

```
progjar-task-04/
│
├── http.py                          ← HTTP/1.1 request parser & response builder
├── socket_proxy.py                  ← Socket proxy implementation
│
├── server_thread_http.py            ← Thread-per-client server
├── server_thread_pool_http.py       ← Thread pool server
├── server_thread_http_secure.py     ← Threaded HTTPS (SSL/TLS) server
├── server_process_http.py           ← Process-per-client server
├── server_process_pool_http.py      ← Process pool server
├── server_async_http.py             ← Async server (callbacks)
├── server_asyncio_stream_http.py    ← Asyncio streams server
│
├── certs/                           ← SSL certificates for HTTPS server
├── client/                          ← Client scripts for testing
│
├── perftest.sh                      ← Performance benchmarking script
│
├── page.html                        ← Static HTML file (served by server)
├── sending.html                     ← Static HTML file
├── donalbebek.jpg                   ← Static image file
├── pokijan.jpg                      ← Static image file
├── research_center.jpg              ← Static image file
├── test1.txt / test2.txt / testing.txt ← Static text files
└── rfc2616.pdf                      ← HTTP/1.1 specification reference
```

---

## How It Works

### HTTP Request Handling
```
Client sends HTTP request (e.g. GET /page.html HTTP/1.1\r\n)
        ↓
Server receives raw bytes via TCP socket
        ↓
http.py parses the request (method, path, headers)
        ↓
Server locates the requested file on disk
        ↓
Server responds with:
  - 200 OK + file content (if found)
  - 404 Not Found (if missing)
        ↓
Connection closed
```

### Concurrency Comparison

| Model | Strength | Weakness |
|---|---|---|
| Thread per client | Simple, isolated per request | High memory at scale |
| Thread pool | Bounded resource usage | Pool size limits concurrency |
| Process per client | True parallelism (bypasses GIL) | High overhead per request |
| Process pool | Bounded, parallel | Pool size limits concurrency |
| Async | Low memory, high concurrency | Complexity; blocks on CPU tasks |
| Asyncio stream | Clean async API | Same as async |
| Thread + SSL | Secure HTTPS connections | SSL handshake overhead |

---

## Getting Started

### Prerequisites
- Python 3.8+
- OpenSSL (for the secure server)

### Run a server
```bash
# Thread-based server (port 8889)
python server_thread_http.py

# Thread pool server
python server_thread_pool_http.py

# Async server
python server_async_http.py

# HTTPS server (requires certs/)
python server_thread_http_secure.py
```

### Test with a browser or curl
```bash
# After starting any server
curl http://localhost:8889/page.html
curl http://localhost:8889/donalbebek.jpg
curl http://localhost:8889/test1.txt

# For HTTPS server
curl -k https://localhost:8889/page.html
```

### Run performance test
```bash
chmod +x perftest.sh
./perftest.sh
```

---

## Key Concepts Covered

- **TCP Socket Programming** — raw socket creation, binding, listening, accepting connections
- **HTTP/1.1 Protocol** — manual implementation of request parsing and response formatting (RFC 2616)
- **Concurrency Models** — threads, processes, async I/O, and pool patterns
- **SSL/TLS** — securing HTTP connections with certificates
- **Socket Proxy** — forwarding connections between endpoints
- **Performance Testing** — benchmarking server throughput and response time

---

## Course Context

This is **Task 4** of the *Pemrograman Jaringan* (Network Programming) course at Institut Teknologi Sepuluh Nopember (ITS) Surabaya. The task focuses on understanding low-level HTTP and socket concepts by implementing them from scratch, rather than using pre-built frameworks.

---

## Author

**Agnes Priscilla Sekartaji Hadikusuma**  
S1 Teknik Informatika · Institut Teknologi Sepuluh Nopember (ITS) Surabaya

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com/in/agnespriscilla)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/agnespriscilla)
[![Email](https://img.shields.io/badge/Email-D14836?style=flat&logo=gmail&logoColor=white)](mailto:agnes.priscilla33@gmail.com)
