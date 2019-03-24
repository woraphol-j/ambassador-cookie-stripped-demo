### How to reproduce the issue

1. Run the services in the cluster

```
kubectl apply -f .
```

2. Use the following curl command to test:

```
curl -X GET http://example.com/echo/test
```

### Explanation

The request will go through the auth service (Nginx that returns 200 and 2 `set-cookie`) and reach the upstream server(echo) that will return a response as is.
The retured result is as follows:

```json
{
  "path": "/test",
  "headers": {
    "host": "example.com",
    "content-type": "application/json",
    "cache-control": "no-cache",
    "postman-token": "e631e861-ea07-415f-9d3b-5699e9495bcc",
    "user-agent": "PostmanRuntime/7.6.1",
    "accept": "*/*",
    "accept-encoding": "gzip, deflate",
    "x-forwarded-for": "10.1.0.193",
    "x-forwarded-proto": "http",
    "x-envoy-internal": "true",
    "x-request-id": "8624f609-3c04-41b1-8ef2-54856b911e7f",
    "set-cookie": ["cookie2=1033;Domain=.whatever.com;Path=/;Max-Age=31536000"],
    "x-envoy-expected-rq-timeout-ms": "3000",
    "x-envoy-original-path": "/echo/test",
    "content-length": "0"
  },
  "method": "GET",
  "body": "",
  "fresh": false,
  "hostname": "example.com",
  "ip": "::ffff:10.1.0.193",
  "ips": [],
  "protocol": "http",
  "query": {},
  "subdomains": [],
  "xhr": false,
  "os": {
    "hostname": "echo-server-6fbc7d99c4-6mx5f"
  }
}
```

Notice that there is only one `set-cookie` left in the response, meaning only the last `set-cookie` set by Nginx will remain.
