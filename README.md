# Best Buy Cloud-Native Application - Staff-Service Microservice

## 🔍 Application and Architecture Overview

The **Best Buy Cloud-Native App** is a microservices-based demo application designed for internal use and customer-facing operations. It enables order management, product listing, and AI-enhanced product presentation through services deployed on Azure Kubernetes Service (AKS). The staff-service microservice is responsible for managing employee information in the system.

> The architecture follows the principles of the Algonquin Pet Store (On Steroids) with a key modification:
> **RabbitMQ** has been replaced with **Azure Service Bus**, a managed queueing service.

## 🏠 Updated Application Architecture

> **![Image](https://github.com/user-attachments/assets/8068cf1f-a8c5-4b26-877a-096d50ab4df5)**

### Components:
- **Store-Front**: Frontend app for customers - IP Address to visit that http://130.107.224.231/ 
- **Store-Admin**: Admin dashboard for internal staff - IP Address to visit that http://130.107.225.174/orders
- **Order-Service**: Sends orders to Azure Service Bus
- **Product-Service**: Handles product CRUD operations
- **Makeline-Service**: Listens to Service Bus for order processing
- **AI-Service**: Generates AI product descriptions/images
- **Staff-Service**: Manages staff info (CRUD via REST APIs)
- **MongoDB**: Persists product and order data

---

## ⚖️ Microservices Repositories

| **Service**                   | **Repository Link**                                               |
|-------------------------------|-------------------------------------------------------------------|
| Store-Front                   | https://github.com/Mishravaibhav0032/bestbuy-store-front-L8       |
| Store-Admin                   | https://github.com/Mishravaibhav0032/bestbuy-store-admin-L8       |
| Order-Service                 | https://github.com/Mishravaibhav0032/bestbuy-order-service-L8     |
| Product-Service               | https://github.com/Mishravaibhav0032/bestbuy-product-service-L8   |
| Makeline-Service              | https://github.com/Mishravaibhav0032/bestbuy-makeline-service-L8  |
| AI-Service                    | https://github.com/Mishravaibhav0032/bestbuy-ai-service-L8        |
| Cloud-Native-App-Service      | https://github.com/Mishravaibhav0032/best-buy-cloud-native-app    |

---

## 🚀 Docker Images

| **Service**         | **Docker Image Link**                                                           |
|---------------------|---------------------------------------------------------------------------------|
| Store-Front         | https://hub.docker.com/repository/docker/vaibhav2792/store-front/general        |
| Order-Service       | https://hub.docker.com/repository/docker/vaibhav2792/order-service/general      |
| Product-Service     | https://hub.docker.com/repository/docker/vaibhav2792/product-service/general    |
| Makeline-Service    | https://hub.docker.com/repository/docker/vaibhav2792/makeline-service/general   |
| AI-Service          | https://hub.docker.com/repository/docker/vaibhav2792/ai-service/general         |
| Store-Admin         |https://hub.docker.com/repository/docker/vaibhav2792/store-admin/general |


---

## 📆 Deployment Instructions

1. Clone all repositories listed above.
2. Build Docker images locally or pull them from Docker Hub.
3. Apply Kubernetes YAML files using:
   ```bash
   kubectl apply -f Deployment-Files/
   ```
4. Ensure ConfigMaps, Secrets, and ServiceBus connection strings are correctly configured.
5. Expose services via LoadBalancer for external access.

> MongoDB uses StatefulSet; ensure persistent volumes are created.

---

## 📍 Deployment Files
All Kubernetes deployment files are in the `/Deployment-Files` folder.

All in One Services Link :-<br></br>
https://github.com/Mishravaibhav0032/best-buy-cloud-native-app/blob/main/Deployment%20Files/aps-all-in-one.yaml

Secret Yaml File :- <br></br>
https://github.com/Mishravaibhav0032/best-buy-cloud-native-app/blob/main/Deployment%20Files/secrets.yaml

Admin Tasks File :- <br></br>
https://github.com/Mishravaibhav0032/best-buy-cloud-native-app/blob/main/Deployment%20Files/admin-tasks.yaml

Config Maps :- <br></br>
https://github.com/Mishravaibhav0032/best-buy-cloud-native-app/blob/main/Deployment%20Files/config-maps.yaml

---

## 🎥 Demo Video

Watch the full deployment and live demo of the app, including AI-generated content:
https://drive.google.com/file/d/1YLa5-ZpDlv_viy3tFim_8UsW_zWDCB-0/view?usp=drive_link

---

## ❌ Issues or Limitations

- MongoDB pods may restart on insufficient resource limits and service bus taking time in showing spikes.
- Initial latency in generating AI images through DALL-E but the quota's limit reached the size.
- AKS external IP allocation can take a few minutes.

---

## 💼 Assignment Deliverables
- [x] Microservices developed and dockerized
- [x] Managed queue (Azure Service Bus) integration
- [x] Kubernetes deployment for all services
- [x] CI/CD setup for staff-service (bonus pending others)
- [x] Architecture diagram and documentation
- [x] Demo video showcasing functionality

---

## ✅ Bonus: CI/CD Pipeline
- GitHub Actions used to automate build and push of Docker images.
- Secrets configured: `DOCKER_USERNAME`, `DOCKER_PASSWORD`

```yaml
# .github/workflows/ci_cd.yaml
on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm install
      - run: docker build -t ${{ secrets.DOCKER_USERNAME }}/staff-service .
      - uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
      - run: docker push ${{ secrets.DOCKER_USERNAME }}/staff-service
```

---

## 🌟 Author
**Vaibhav Mishra(041165850) & Meet Prajapati(041178387)**  
Full-Stack Cloud-Native <br></br>
Algonquin College, 2025
