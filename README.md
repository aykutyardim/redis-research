# redis-research

## 1. What is Redis?
---

Redis is an open-source, in-memory data structure store that can be used as a database, cache, and message broker. It is known for its speed, simplicity, and flexibility. Redis is also referred to as a "data structure server" because it provides support for a variety of data structures such as strings, hashes, sets, lists, and sorted sets.


### What are the key features of Redis?

- **In-memory storage**: Redis stores data in memory, which makes it extremely fast. However, it also means that Redis can be limited by the amount of available memory.

- **Persistence**: Redis can persist data to disk, which means that it can survive a system restart or failure.

- **Replication**: Redis supports master-slave replication, which allows you to create replicas of your data for increased availability and performance.

- **Pub/sub messaging**: Redis supports publish/subscribe messaging, which allows applications to broadcast messages to multiple subscribers.

- **Lua scripting**: Redis supports Lua scripting, which allows you to execute complex operations on the server side.

- **Transactions**: Redis supports transactions, which allows you to execute multiple commands as a single atomic operation.

- **Geo commands**: Redis supports a set of commands for geospatial indexing and querying.

### What are the use cases of Redis?

- Redis can be used in a variety of use cases such as session management, real-time analytics, real-time messaging, caching, and leaderboards. It supports a variety of programming languages, including Java, Python, Node.js, Ruby, and more.

### What are the popular alternatives of Redis?

- **Memcached**: Memcached is an open-source, in-memory caching system that is similar to Redis in that it provides high-performance caching. However, unlike Redis, it is focused solely on caching and does not provide support for data structures or persistence.

- **Apache Cassandra**: Apache Cassandra is a highly scalable, distributed NoSQL database that is designed to handle large amounts of data across multiple servers. Unlike Redis, Cassandra supports horizontal scaling, fault tolerance, and high availability.

- **MongoDB**: MongoDB is a document-oriented NoSQL database that is designed to be flexible and scalable. It provides support for complex data structures and can be used for a wide range of use cases, from web applications to big data analytics.

- **Apache Kafka**: Apache Kafka is a distributed streaming platform that is used for building real-time data pipelines and streaming applications. It provides support for pub/sub messaging, fault tolerance, and horizontal scaling.

- **Hazelcast**: Hazelcast is an open-source, in-memory data grid that provides support for distributed caching, distributed data structures, and distributed processing. It is designed to provide high availability and scalability for enterprise applications.

- **Aerospike**: Aerospike is a high-performance, distributed NoSQL database that is designed for real-time, mission-critical applications. It provides support for strong consistency, high availability, and high throughput.


### Memcached vs Redis
- **Data Structure Support**: Redis supports a variety of data structures such as strings, hashes, sets, lists, and sorted sets, while Memcached only supports a simple key-value store.

- **Persistence**: Redis provides multiple options for data persistence, including snapshots and AOF (Append-Only File) logs, while Memcached does not provide built-in support for persistence.

- **Multi-Threading**: Redis supports multiple threads and concurrent requests, while Memcached is single-threaded.

- **Memory Management**: Redis has more advanced memory management capabilities, such as the ability to use virtual memory and automatic memory reclamation, while Memcached has a simpler approach to memory management.

- **Performance**: Both Redis and Memcached are known for their high performance, but Redis may be slightly faster in some cases due to its support for multi-threading and advanced memory management.

- **Replication**: Both Redis and Memcached support replication, but Redis has more advanced options for replication and can be used in a master-slave or master-master configuration.

- **In summary**: Redis is more feature-rich than Memcached, with support for a variety of data structures, persistence options, multi-threading, and more advanced memory management capabilities. However, Memcached is simpler and may be more appropriate for use cases that require only basic key-value storage and retrieval.

### Redis vs Hazelcast
- **Data Structure Support**: Redis supports a variety of data structures such as strings, hashes, sets, lists, and sorted sets, while Hazelcast supports distributed data structures such as maps, queues, and topics.

- **Persistence**: Redis provides multiple options for data persistence, including snapshots and AOF (Append-Only File) logs, while Hazelcast supports persistence to disk.

- **Multi-Threading**: Redis supports multiple threads and concurrent requests, while Hazelcast is designed to be fully multi-threaded and supports parallel processing.

- **Memory Management**: Redis has more advanced memory management capabilities, such as the ability to use virtual memory and automatic memory reclamation, while Hazelcast provides distributed garbage collection.
- **Programming Language Support**: Redis supports a wide range of programming languages, while Hazelcast provides support for Java and .NET.
- **Performance**: Both Redis and Hazelcast are known for their high performance, but the performance characteristics of each can depend on the specific use case and deployment configuration.

- **Replication**: Both Redis and Hazelcast support replication, but Hazelcast has more advanced options for replication and can be used in a master-slave or master-master configuration with data partitioning.

- **In summary**: Redis is more feature-rich and supports a wider range of programming languages, while Hazelcast is designed to be fully multi-threaded and provides advanced support for distributed data structures, garbage collection, and replication. The choice between Redis and Hazelcast will depend on the specific requirements of the application and the deployment environment.

## 2. Get Started
---
### How can be run with Docker?

```docker run --name my-redis-container -d redis```

### What are the basic configuration options of Redis image?

- **redis.conf**: You can specify a custom Redis configuration file by mounting it as a volume inside the container.

```docker run --name my-redis-container -v /path/to/redis.conf:/usr/local/etc/redis/redis.conf -d redis redis-server /usr/local/etc/redis/redis.conf```

- **REDIS_PASSWORD**: 

```docker run --name my-redis-container -e REDIS_PASSWORD=mysecretpassword -d redis```

- **maxmemory**:

```docker run --name my-redis-container -d redis redis-server --maxmemory 1gb --maxmemory-policy allkeys-lru```

- **appendonly**:  You can enable the AOF (Append-Only File) persistence mechanism by setting the appendonly configuration option to yes.

```docker run --name my-redis-container -d redis redis-server --appendonly yes```


- **bind**: You can specify the network interface that Redis listens on by setting the bind configuration option. By default, Redis listens on all network interfaces. 

```docker run --name my-redis-container -d redis redis-server --bind 127.0.0.1```

## 3. Redis with Spring Boot
---

### Dependencies

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```

### Application configuration 

```
spring.redis.host=localhost
spring.redis.port=6379
spring.redis.password=
```

### Caching items

```
@Service
public class MyService {

    @Cacheable("myCache")
    public String getFoo(int id) {
        // ...
    }
}
```

- In this example, the getFoo() method is annotated with @Cacheable("myCache"), which means that the results of this method will be cached using the cache name "myCache".

### Fetching items

```
@RestController
public class MyController {

    @Autowired
    private MyService myService;

    @GetMapping("/foo/{id}")
    public String getFoo(@PathVariable int id) {
        return myService.getFoo(id);
    }
}
```

- The first time the method is called, it will execute the method and cache the result. Subsequent calls to the method with the same arguments will return the cached result without executing the method again.

## 4. Advanced Usages

---

### What are the advanced use-cases of Redis?

- **Pub/Sub messaging**: Redis supports publish/subscribe (pub/sub) messaging, which allows messages to be broadcast to multiple subscribers. This feature can be used for real-time messaging, chat applications, and event-driven architectures.

- **Distributed locks**: Redis supports distributed locks, which can be used to ensure that only one process or thread can access a resource at a time. This feature is useful for avoiding race conditions and maintaining consistency in distributed systems.

- **Data structures**: Redis supports a variety of data structures, including sets, lists, hashes, and sorted sets. These data structures can be used for tasks such as managing user sessions, tracking real-time analytics data, and implementing leaderboards.

- **Full-text search**: Redis includes a module called Redisearch that provides full-text search capabilities. This feature is useful for applications that need to perform complex search queries on large datasets.

- **Geospatial indexing**: Redis supports geospatial indexing, which allows you to store and query location-based data. This feature is useful for applications that need to perform spatial queries, such as finding nearby stores or restaurants.

- **Stream processing**: Redis includes a module called Redis Streams that provides support for stream processing. This feature can be used for tasks such as real-time data processing, log aggregation, and event-driven architectures.

### What are the use-cases of @CachePut and @CacheEvict?

- **@CachePut** is used to update or create an item in the cache. When a method annotated with `@CachePut` is called, the result is cached with the specified key, even if it already exists. This annotation is useful when you need to update an item in the cache as a result of a method call.

- In the following example, the `updateUser()` method updates the user in the database and returns the updated user object. The updated user object is then cached using the specified key.

```
@CachePut(value = "users", key = "#user.id")
public User updateUser(User user) {
    // update the user in the database
    return user;
}
```

- **@CacheEvict** is used to remove an item from the cache. When a method annotated with `@CacheEvict` is called, the item with the specified key is removed from the cache. This annotation is useful when you need to remove an item from the cache as a result of a method call.


```
@CacheEvict(value = "users", key = "#id")
public void deleteUser(int id) {
    // delete the user from the database
}
```

### How can be separated two types objects in Redis?

```
// Cache TypeA objects using the key "typea:{id}"
@Cacheable(value = "myCache", key = "'typea:' + #id")
public TypeA getTypeAById(String id) {
    // retrieve TypeA object from the database
    return typeAObject;
}

// Cache TypeB objects using the key "typeb:{id}"
@Cacheable(value = "myCache", key = "'typeb:' + #id")
public TypeB getTypeBById(String id) {
    // retrieve TypeB object from the database
    return typeBObject;
}

```
- In this example, the `getTypeAById()` method uses the cache key `typea:{id}`, where `{id}` is the ID of the TypeA object. Similarly, the `getTypeBById()` method uses the cache key `typeb:{id}`, where `{id}` is the ID of the TypeB object.


```
// Retrieve TypeA object by ID
TypeA typeAObject = getTypeAById("123");

// Retrieve TypeB object by ID
TypeB typeBObject = getTypeBById("456");
```
---
> Note that when using multiple cache keys, you may want to consider using different Redis databases or namespaces to avoid potential key collisions. You can configure the Redis database or namespace in the Spring Boot application properties.
---


### Fetching as a block and filtering them in application level or fetching data with unique id?

- Fetching data from Redis cache depends on your application's requirements and the type of data you are caching.

- If you are caching large sets of data and you need to filter them based on some criteria, it may be more efficient to fetch the data as a block and filter them in the application level. This is because fetching a block of data from Redis involves fewer network round-trips than fetching individual items, and filtering the data in the application level can be more flexible and customizable.

- On the other hand, if you are caching individual items and you need to retrieve them by unique ID, it may be more efficient to fetch the data by ID. This is because Redis provides fast lookups by key, and fetching data by unique ID can be faster than fetching a block of data and filtering them in the application level.

- In general, if you have large sets of data that can be filtered in the application level, or if you need flexible and customizable filtering criteria, fetching data as a block and filtering them in the application level may be a better option. However, if you are caching individual items and you need to retrieve them by unique ID, fetching data by ID may be a more efficient option. Ultimately, the best approach depends on your specific use case and performance requirements.

### How can serialize, cache, fetch, and deserialize data using Redis Hashes in a Spring Boot Application?

- **Define a Redis Hash to store data**: In Redis, a Hash is a collection of key-value pairs, where the key is a unique identifier and the value is a serialized object or data. 

- **Serialize data**: Before storing in Redis, you need to serialize it into a format that can be stored as a string value.

- **Cache data**: To cache in Redis, you can use the RedisTemplate class provided by Spring Data Redis. This class provides methods to access Redis Hashes, such as `opsForHash()`. With the `opsForHash()` method, you can get a HashOperations object, which provides methods to read and write data to Redis Hashes.

- **Fetch data**: To fetch data from Redis, you can use the `get()` method of HashOperations, where you pass the name of the Redis Hash and the key (user ID). This method returns the serialized data as a string value.

- **Deserialize data**: After fetching data from Redis, you need to deserialize it back into a Java object.

```
@Autowired
private RedisTemplate<String, String> redisTemplate;

// Cache user data
public void cacheUser(User user) {
  HashOperations<String, String, String> hashOps = redisTemplate.opsForHash();
  String userData = new ObjectMapper().writeValueAsString(user);
  hashOps.put("users", user.getId(), userData);
}
```

```
// Fetch user data
public User getUser(String id) {
  HashOperations<String, String, String> hashOps = redisTemplate.opsForHash();
  String userData = hashOps.get("users", id);
  return new ObjectMapper().readValue(userData, User.class);
}
```

## 5. Redis as a Distributed System
---

Redis can be used as a distributed system for interprocess communication between two separated microservices by using Redis Pub/Sub mechanism. Pub/Sub is a messaging pattern where publishers send messages to a channel, and subscribers receive messages from that channel.

- **Setup Redis**: You need to have Redis installed and running as a standalone server or a cluster. You can use a Redis client library for your preferred programming language to connect to Redis from your microservices.

- **Define channels**: You can define one or more channels to use for communication between your microservices. For example, you can define a channel named "orders" to send order-related messages between the order service and the payment service.

- **Publish messages**: In your microservice that publishes messages, you can use the Redis client library to connect to Redis and publish messages to the relevant channels. For example, in the order service, you can publish an order-related message to the "orders" channel when a new order is created.

- **Subscribe to messages**: In your microservice that receives messages, you can use the Redis client library to connect to Redis and subscribe to the relevant channels. For example, in the payment service, you can subscribe to the "orders" channel to receive order-related messages.

- **Process messages**: When a message is received on a subscribed channel, your microservice can process the message as needed. For example, in the payment service, you can process the order-related message received on the "orders" channel to initiate a payment transaction.

> **Order Service**

```
@Autowired
private RedisTemplate<String, Object> redisTemplate;

// Publish an order-related message to the "orders" channel
public void publishOrderMessage(Order order) {
  redisTemplate.convertAndSend("orders", order);
}
```

> **Payment Service**

```
@Autowired
private RedisTemplate<String, Object> redisTemplate;

// Subscribe to the "orders" channel to receive order-related messages
public void subscribeToOrderMessages() {
  redisTemplate.execute(new RedisCallback<Void>() {
    @Override
    public Void doInRedis(RedisConnection connection) throws DataAccessException {
      connection.subscribe(new MessageListener() {
        @Override
        public void onMessage(Message message, byte[] pattern) {
          // Process the order-related message received on the "orders" channel
          Order order = (Order) redisTemplate.getValueSerializer().deserialize(message.getBody());
          initiatePayment(order);
        }
      }, "orders".getBytes());
      return null;
    }
  });
}
```

- In this example, the RedisTemplate class is used to connect to Redis and publish/subscribe to channels. The publishOrderMessage() method in the order service publishes an order-related message to the "orders" channel using the convertAndSend() method of RedisTemplate. The subscribeToOrderMessages() method in the payment service subscribes to the "orders" channel using the execute() method of RedisTemplate, and processes the order-related messages received on that channel in the onMessage() method of the MessageListener interface.