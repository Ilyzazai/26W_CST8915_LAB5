# CST8915 – Lab 5 Submission (Full-stack Cloud-native Development)

## Student
- **Name:** Ilyas Zazai
- **Course:** CST8915 – Full-stack Cloud-native Development
- **Lab:** Lab 5 – Containerizing the Algonquin Pet Store with Docker

---

## Demo Video
- **YouTube Link:** https://www.youtube.com/watch?v=MfM2ftv0H-0

---

## Submission Repository
This repository contains the final Docker Compose configuration for running the Algonquin Pet Store application with Docker Hub images.

---

## Source Repositories
These are the clone application repositories used to build and push the Docker images:

- **Order Service:** https://github.com/Ilyzazai/26W_CST8915_LAB5/tree/main/order-service-L4
- **Product Service:** https://github.com/Ilyzazai/26W_CST8915_LAB5/tree/main/product-service-python-L4
- **Store Front:** https://github.com/Ilyzazai/26W_CST8915_LAB5/tree/main/store-front-L4

---

## Docker Hub Images
- ilyaszazai/order-service:latest
- ilyaszazai/product-service:latest
- ilyaszazai/store-front:latest

---

## Lab Overview
In this lab, I containerized the Algonquin Pet Store microservices using Docker and used Docker Compose to run them together as a multi-container application. The application includes:

- **order-service** (Node.js)
- **product-service** (Python or Rust)
- **store-front** (Vue.js)
- **rabbitmq** for messaging

The final setup uses Docker Hub images instead of local builds, which makes the application easier to run on any machine or virtual machine.

---

## Architecture Overview
The Docker Compose application contains the following services:

- **rabbitmq** provides the messaging broker for order processing.
- **order-service** connects to RabbitMQ and handles order requests.
- **product-service** provides product data through an API.
- **store-front** is the frontend that communicates with the backend services.

Docker Compose creates a shared network automatically, so the containers can communicate with each other using service names such as `rabbitmq`, `order-service`, and `product-service`.

---

## Final `docker-compose.yaml`

```yaml
services:
  rabbitmq:
    image: rabbitmq:3-management
    ports:
      - "5672:5672"
      - "15672:15672"
    environment:
      RABBITMQ_DEFAULT_USER: myuser
      RABBITMQ_DEFAULT_PASS: mypassword

  order-service:
    image: ilyaszazai/order-service:latest
    ports:
      - "3000:3000"
    environment:
      PORT: 3000
      RABBITMQ_CONNECTION_STRING: amqp://myuser:mypassword@rabbitmq:5672/
    depends_on:
      - rabbitmq

  product-service:
    image: ilyaszazai/product-service:latest
    ports:
      - "3030:3030"
    environment:
      PORT: 3030

  store-front:
    image: ilyaszazai/store-front:latest
    ports:
       - "8080:80"
    depends_on:
      - order-service
      - product-service
```
---

## Notes / Lessons Learned
- Docker makes each microservice portable and consistent across environments.
- Docker Compose simplifies running multiple containers together.
- Using Docker Hub images removes the need to build each service locally every time.
- Container networking allows services to communicate by service name instead of hard-coded IP addresses.
- RabbitMQ can be accessed by the `order-service` using the internal Docker network.

> Note: For a Vue frontend served by Nginx, service URLs are often baked into the frontend image during the build stage. If the frontend does not call the backend correctly, the `store-front` image may need to be rebuilt with the correct API URLs before pushing it to Docker Hub.

---

## Optional Reflection
This lab helped me understand how microservices can be packaged into containers and managed together in one environment. I learned how to use Dockerfiles for different technologies, how to orchestrate services with Docker Compose, and how to store images in Docker Hub for reuse. This lab also showed how containers support cloud-ready and portable application deployment.

---

## AI Usage
AI was used to help format documentation and improve the structure of the submission README.
