# content would be added until monday 11pm 


# CST8915 – Lab 5 Submission (Full-stack Cloud-native Development)

## Student
- **Name:** Ilyas Zazai
- **Course:** CST8915 – Full-stack Cloud-native Development
- **Lab:** Lab 5 – Containerizing the Algonquin Pet Store with Docker

---

## Demo Video
- **YouTube Link:** _Add your video link here later_

<!-- Leave this section for your future demo video -->


---

## Submission Repository
This repository contains the final Docker Compose configuration for running the Algonquin Pet Store application with Docker Hub images.

### Files Included
- `README.md`
- `docker-compose.yaml`

---

## Source Repositories
These are the forked application repositories used to build and push the Docker images:

- **Order Service:** _Add your GitHub repo link here_
- **Product Service:** _Add your GitHub repo link here_
- **Store Front:** _Add your GitHub repo link here_

---

## Docker Hub Images
Replace the placeholders below with your real Docker Hub image links if you want:

- `YOUR_DOCKERHUB_USERNAME/order-service:latest`
- `YOUR_DOCKERHUB_USERNAME/product-service:latest`
- `YOUR_DOCKERHUB_USERNAME/store-front:latest`

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
version: '3'

services:
  rabbitmq:
    image: rabbitmq:3-management
    ports:
      - "5672:5672"
      - "15672:15672"
    environment:
      - RABBITMQ_DEFAULT_USER=myuser
      - RABBITMQ_DEFAULT_PASS=mypassword

  order-service:
    image: YOUR_DOCKERHUB_USERNAME/order-service:latest
    ports:
      - "3000:3000"
    environment:
      - RABBITMQ_CONNECTION_STRING=amqp://myuser:mypassword@rabbitmq:5672/
      - PORT=3000
    depends_on:
      - rabbitmq

  product-service:
    image: YOUR_DOCKERHUB_USERNAME/product-service:latest
    ports:
      - "3030:3030"
    environment:
      - PORT=3030

  store-front:
    image: YOUR_DOCKERHUB_USERNAME/store-front:latest
    ports:
      - "80:80"
    environment:
      - VUE_APP_ORDER_SERVICE_URL=http://order-service:3000
      - VUE_APP_PRODUCT_SERVICE_URL=http://product-service:3030
    depends_on:
      - order-service
      - product-service
```

---

## How to Run the Application
Run the following command in the repository root:

```bash
docker compose up -d
```

To rebuild after updating images:

```bash
docker compose pull
docker compose up -d
```

Open these URLs after the containers start:

- **Store Front:** `http://localhost`
- **RabbitMQ Management UI:** `http://localhost:15672`

RabbitMQ default login from this file:

- **Username:** `myuser`
- **Password:** `mypassword`

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
