# Distributed Web Crawler with Kafka & ElasticSearch
A scalable Java-based web crawler that extracts and indexes website content in real-time. 
Designed to perform concurrent, depth-limited crawls, it tracks progress via Redis and stores indexed content in Elasticsearch for full-text search.

---

## Tech Stack
- **Java**
- **Spring Boot**
- **Spring Web**
- **Spring WebFlux**
- **Redis**
- **Apache Kafka**
- **ElasticSearch**
- **Jsoup**
- **Jackson**
- **OkHttp**
- **Maven**
  
---

## Features
- Submit a crawl request with configurable depth, max pages, and time limit
- Async Kafka-based processing of URLs
- Automatic URL deduplication using Redis
- Status tracking for each crawl (visited pages, duration, stop reason)
- Extracts links from pages and recursively queues them
- Indexes results in Elasticsearch for later searching
- Graceful handling of timeouts, distance limits, and max URL thresholds

---

## Project Structure
```text
src/
├── main/
│   ├── java/
│   │   └── com/handson/searchengine/
│   │       ├── config/            → Kafka and Redis configurations
│   │       │   ├── KafkaTopicConfig.java
│   │       │   └── RedisConfig.java
│   │       ├── controller/        → REST endpoints
│   │       │   └── AppController.java
│   │       ├── crawler/           → Core crawling logic
│   │       │   └── Crawler.java
│   │       ├── kafka/             → Kafka producer and consumer
│   │       │   ├── Producer.java
│   │       │   └── Consumer.java
│   │       ├── model/             → Data Transfer Objects (DTOs)
│   │       └── util/              → ElasticSearch integration
│   │           └── ElasticSearch.java
│   └── resources/
│       └── application.properties
├── docker-compose.yml
└── pom.xml
```
---
### Example request
```http
POST /api/crawl
Content-Type: application/json

{
  "url": "https://example.com",
  "maxUrls": 50,
  "maxDistance": 2,
  "maxTime": 10000
}
```
---

## API Endpoints

| Method | Endpoint               | Description                                  |
| ------ | ---------------------- | -------------------------------------------- |
| POST   | `/api/crawl`           | Starts a new crawl with given URL and config |
| GET    | `/api/crawl/{crawlId}` | Returns progress of a specific crawl         |
| POST   | `/api/sendKafka`       | Sends a crawl task directly to Kafka         |


---

## Running the App
### Prerequisites
```text
Java
Maven
Kafka + Zookeeper
Redis 
ElasticSearch
```
### Build & Run
```bash
mvn clean install
mvn spring-boot:run
```
### Configuration
Update application.properties:

```properties
spring.kafka.bootstrap-servers=localhost:9092
spring.redis.host=localhost
elasticsearch.base.url=https://your-es-url
elasticsearch.index=your-index-name
elasticsearch.key=your-es-api-key
```
