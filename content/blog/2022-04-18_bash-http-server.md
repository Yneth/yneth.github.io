+++
title = "bash http server with fixed response"

[taxonomies]
tags = ["bash", "nc"]
+++

```bash
while true; do { 
  echo -e "HTTP/1.1 400 Bad Request\nContent-Type: application/json\n\n{\"foo\": \"bar\"}"
} | nc -l 8082; done
```