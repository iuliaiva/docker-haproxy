# docker-haproxy
Basic Docker image to run haproxy as client to connect the server with mutual auth

You need edit (add) this env:
- **LISTENPORT**: 80 or other port value
- **MODE**: mode tcp or http
- **CERTIFICATE**: server certificate file name placed as volume in path /usr/local/etc/haproxy/certs/ (optional)
- **DEFAULT**: define default backend
- **RETRIES**: define number of retries to perform on a server after a connection failure (optional - default 3)
- **LOGFORMAT**: define custom log format, ex `"%Ci [%t] %b %hr %r %ST %B %Tr"` (optional)
- **START_MODE**: define start mode, ex `simple` or `haproxy` or `multiconfig`

- **CA**: server trusted ca file name placed as volume in path /usr/local/etc/haproxy/certs/ (optional)
- **VERIFY**: if you enabled CA you should provide verify option ```optional``` or ```required```

- **BACKEND_1_ADDRESS**: backend address
- **BACKEND_1_PORT**: backend port
- **BACKEND_1_ACL**: acr rule like "hdr_dom(server) -i BACKEND_1"
- **BACKEND_1_CERTIFICATE**: backend client certificate file name placed as volume in path /usr/local/etc/haproxy/certs/ (optional)
- **BACKEND_1_REQIREP**: reqirep (optional)

- **RESOLVER_1_VALUE**: dns domain (optional)

- **PRINT_CONFIG**: print config files on startup (optional)

**There is important change - we moved to supervisord.**
HAProxy can be started with different modes:
- **simple**: configuration based on single file
- **haproxy**: configuration based on environments
- **multiconfig**: configuration based on multiple files

You also need to mount folder with client certificates to /usr/local/etc/haproxy/certs/

Container will kill itself after any of the process will enter FATAL state. (This prevents bug with haproxy is dead, but rsyslog still works).
Usage: 
```
docker run --name haproxy -d -v /path/to/certs/:/usr/local/etc/haproxy/certs/ -e LISTENPORT=80 -e BACKEND_1_ACL="hdr_dom(server) -i BACKEND_1" -e BACKEND_1_ADDRESS=my-server.com -e BACKEND_1_PORT=443 -e BACKEND_1_CERTIFICATE=my-server.crt oberthur/docker-haproxy
```
