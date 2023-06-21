# diy-redis

## How to run it

1. Make sure you build the env
   ```
   mamba env create -f environment.yml
   ```
1. Start the server:
   ```
   make run-server
   ```
1. Start the client:
   ```
   make run-client
   ```

Note: I will suggest you run the client as a debugger. You will be able to see where it crash.

### Current state of the project

The server is working as expected. The socket creation works as well.
The problem I currently see is that in the redis_server.py line 78:

```python
socket_file.write(buf.getvalue())
```

Doesn't seems to be working as expected. So what happen is that the `handle_request` will not be able to read from that socket_file.

## Description

Following this tutorial: https://charlesleifer.com/blog/building-a-simple-redis-server-with-python/

We will create a cache that can handle the following datatypes (Redis DT):

- string
- number
- collection
- list [multi dimensional]
  = null

2 protocols:

- Wire Query per second
- GRPC

Query performance types:

- Throughput: How long to get 100mb file.
- Latency: How long before you get the first byte.

We will build a Redis cache this way:

1. Week 1: GET and SET call for all datatypes.
2. Week 2: DELETE and FLUSH for all datatypes
3. Week 3: MGET and MSET for all datatypes
4. Week 4: Add ability to handle multiple clients async

Additionally: Discussion around how we could transform this cache to allow it to handle embeddings storage + search similarity. https://www.linkedin.com/pulse/vector-databases-demystified-part-2-building-your-own-adie-kaye/ https://github.com/adiekaye/very-simple-vector-database

Reference:

- RedisAI (cache pickle file much better)
- Redis has 10k write/s in its protocol
- Redis benchmark https://redis.io/docs/management/optimization/benchmarks/
- gRPC https://grpc.io/
- The Software Pro's Best Kept Secret. (rust redis) https://github.com/login/oauth/authorize?client_id=acc30b7ce12bf360599b&redirect_uri=https%3A%2F%2Fapp.codecrafters.io%2Fauth%2Fgithub%2Fcallback&response_type=code&scope=user%3Aemail&state=ddbb0ba228e55ba4f4ac22fba2bab8c4cf47c86a72b28c82
