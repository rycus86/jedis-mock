[![GitHub release](https://img.shields.io/github/release/fppt/jedis-mock.svg)](https://github.com/fppt/jedis-mock/releases/latest)
[![Build Status](https://travis-ci.org/fppt/jedis-mock.svg?branch=master)](https://travis-ci.org/fppt/jedis-mock)

# Redis-Mock

Redis Mock is a simple in-memory mock of redis for java testing. 
It allows you to test any behaviour dependent on redis without having to deploy an instance of redis

## Quickstart 

Add it as a dependency in Maven as:

```xml
<dependency>
  <groupId>com.github.fppt</groupId>
  <artifactId>jedis-mock</artifactId>
  <version>1.0.0</version>
</dependency>
```

Create a redis server and bind it to jedis:

```
private static RedisServer server = null;

@Before
public void before() throws IOException {
  server = RedisServer.newRedisServer();  // bind to a random port
  server.start();
}

@Test
public void test() {
  ...
  Jedis jedis = new Jedis(server.getHost(), server.getBindPort());
  ...
}

@After
public void after() {
  server.stop();
  server = null;
}
```

From here test as needed

## Master and Slave

```
RedisServer master = newRedisServer();
RedisServer slave = newRedisServer();
master.setSlave(slave);
```

## Fault tolerance testing

We can make a RedisServer close connection after every several commands. This will cause a connection exception for clients.

```
RedisServer server = RedisServer.newRedisServer();
ServiceOptions options = new ServiceOptions();
options.setCloseSocketAfterSeveralCommands(3);
server.setOptions(options);
server.start();
```

