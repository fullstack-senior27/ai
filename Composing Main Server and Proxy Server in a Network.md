🧠 Programming Contest QA: Composing Main Server and Proxy Server in a Network
✅ General Understanding
Q1: What is the role of a proxy server in a client-server architecture?
A: A proxy server acts as an intermediary between the client and the main server. It forwards requests from clients to the main server and may cache or modify the request/response to optimize performance or enforce security policies.

Q2: Why might you use a proxy server in a programming contest system?
A: To add load balancing, logging, request filtering, rate limiting, or to isolate the main judging server for security purposes.

⚙️ Implementation Questions
Q3: What networking protocol is typically used between the proxy server and the main server?
A: HTTP or HTTPS (via TCP). For real-time use cases, WebSocket or gRPC over HTTP/2 might be used.

Q4: How can you forward a request from the proxy to the main server in code?
A: In a simple Node.js proxy:

const httpProxy = require('http-proxy');
const proxy = httpProxy.createProxyServer({});
require('http').createServer((req, res) => {
  proxy.web(req, res, { target: 'http://main-server:8080' });
}).listen(3000);

Q5: How would you log requests on the proxy server?
A: You can use middleware (e.g., morgan in Express.js) to log method, path, headers, and response time.

🔒 Security & Isolation
Q6: How can a proxy help isolate the main server from public access?
A: By allowing only the proxy to be exposed to the public internet, and making the main server accessible only from internal networks (e.g., using a firewall or private subnet).

Q7: What security headers should a proxy add to responses?
A: Common ones include X-Content-Type-Options, Strict-Transport-Security, X-Frame-Options, and Content-Security-Policy.

🧪 Failure Handling
Q8: If the main server crashes, how should the proxy respond?
A: Return a 502 Bad Gateway or custom error page. Implement retry logic or fallback options if needed.

Q9: How do you monitor the health of the main server from the proxy?
A: By sending regular health check requests (e.g., /healthz) and using tools like NGINX’s proxy_next_upstream, or custom logic in a reverse proxy.

🏁 Example Scenario
Q10: Contestants submit code to a public API. How would you structure the proxy and main judging server?
A:

Proxy Server: Exposes /submit, authenticates, rate-limits, and logs.

Main Server: Validates submission, runs code in a sandbox, stores result.

Communication: Proxy forwards submission payload to http://main-server/judge.