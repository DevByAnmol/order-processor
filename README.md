# 🛒 Order Processor – Distributed System Demo

A **mini distributed system** built with **Java, Spring Boot, PostgreSQL, Kafka, Redis, and Docker**.  
This project simulates a real-world **order processing workflow** with idempotency, asynchronous event handling, caching, retries, and observability.

---

## 📌 Features
- **Order API Service (`order-api`)**
    - Create and fetch orders
    - Idempotency key support (safe retries)
    - Publishes order events to Kafka

- **Order Worker Service (`order-worker`)**
    - Consumes order events from Kafka
    - Calls Payment Simulation service
    - Updates order status in database
    - Implements retries and resilience

- **Payment Simulation Service (`payment-sim`)**
    - Mock payment processor (success/failure responses)

- **Infrastructure (`infra`)**
    - PostgreSQL for persistence
    - Kafka for async messaging
    - Redis for caching
    - Docker Compose for easy setup

---

## 🏗️ Architecture
```text
+-----------+       +-------------+       +-------------+
|  Client   | --->  |  order-api  | --->  |   Kafka     |
+-----------+       +-------------+       +-------------+
                          |                        |
                          v                        v
                   +-------------+       +-------------+
                   | order-worker| <---  | payment-sim |
                   +-------------+       +-------------+
                          |
                          v
                     PostgreSQL
                          |
                          v
                        Redis
```

---

## 🚀 Getting Started

### 1. Clone the repo
```bash
git clone https://github.com/DevByAnmol/order-processor.git
cd order-processor
```

### 2. Start infrastructure
```bash
cd infra
docker-compose up -d
```

Services:
- PostgreSQL → `localhost:5432` (db: `orderdb`, user: `orderuser`, pass: `secret`)
- Kafka → `localhost:9092`
- Redis → `localhost:6379`

### 3. Run services
```bash
cd order-api
./mvnw spring-boot:run
```

```bash
cd order-worker
./mvnw spring-boot:run
```

```bash
cd payment-sim
./mvnw spring-boot:run
```

---

## 📬 Example API Usage

### Create Order
```bash
curl -X POST http://localhost:8080/orders   -H "Content-Type: application/json"   -H "Idempotency-Key: abc123"   -d '{"customerId": "CUST001", "amount": 200, "currency": "USD"}'
```

### Get Order
```bash
curl http://localhost:8080/orders/1
```

---

## 🧰 Tech Stack
- **Java 17**
- **Spring Boot 3**
- **PostgreSQL + Flyway**
- **Kafka (Confluent/Bitnami image)**
- **Redis**
- **Docker & Docker Compose**

---

## ✅ Roadmap
- [x] Project scaffolding & infra setup
- [ ] Order creation API
- [ ] Idempotency support
- [ ] Kafka event publishing
- [ ] Worker processing + payment simulation
- [ ] Redis caching
- [ ] Observability (Actuator, Prometheus, Grafana)
- [ ] Integration tests with Testcontainers

---

## 📜 License
MIT License
