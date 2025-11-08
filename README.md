# Microservices Demo with Checkout DB + Observability (SigNoz)

This project is based on Google's **Microservices Demo Application**, enhanced with:
- ✅ Order persistence into **PostgreSQL**
- ✅ Deployment on **Minikube + Azure Kubernetes Service (AKS)**
- ✅ Full observability using **SigNoz (traces, logs, metrics)**

---

## 🚀 Features

### ✅ Application
- Modified **CheckoutService** to save order data into PostgreSQL
- Application fully containerized using **Docker**
- Includes services like:
  - `frontend`
  - `checkoutservice`
  - `productcatalogservice`
  - `shippingservice`
  - `emailservice`
  - `paymentservice`
  - `cartservice`
  - `currencyservice`
  - `adservice`
  - `redis-cart`

---

## 🗄️ Database (Postgres)

Orders are persisted with the schema:

```sql
CREATE TABLE orders (
  id SERIAL PRIMARY KEY,
  order_id VARCHAR(64),
  user_id VARCHAR(64),
  total_amount VARCHAR(32),
  currency VARCHAR(8),
  transaction_id VARCHAR(128),
  shipping_id VARCHAR(128),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Environment variables for checkout service:
```
DB_HOST=postgres
DB_PORT=5432
DB_USER
DB_PASSWORD
DB_NAME=shopdb
```

---

## ☸️ Kubernetes Deployment

### Deployment used:
- `kubectl apply -f kubernetes-manifests.yaml`
- PostgreSQL deployed via `postgres.yaml`
- CheckoutService uses updated image from DockerHub:
```
abw1729/checkoutservice:v2
```

### Exposing Frontend
```
kubectl apply -f frontend-lb.yaml
```

### Exposing SigNoz Dashboard
```
kubectl apply -f signoz-frontend-lb.yaml
```

---

## 🔎 Observability — SigNoz

### Enabled:
✅ Logs  
✅ Metrics  
✅ Traces  

Collector endpoint used by services:
```
signoz-otel-collector.platform.svc.cluster.local:4317
```

Environment Variables:
```
OTEL_EXPORTER_OTLP_ENDPOINT
OTEL_RESOURCE_ATTRIBUTES
OTEL_TRACES_EXPORTER=otlp
OTEL_METRICS_EXPORTER=otlp
```

Checkout service also connects manually using OTLP gRPC.

Dashboard visualizations created for:
- Total orders
- Order frequency
- Checkout service traces
- Shipping, payment latencies
- Error rate breakdown

---

## ☁️ Deployment to AKS (Azure)

Steps:
1. Create AKS cluster
2. Connect cluster via `az aks get-credentials`
3. Apply manifests
4. Create LoadBalancer service for frontend
5. Create LoadBalancer service for SigNoz
6. Validate order ingestion in DB + traces in SigNoz

---

## 🧪 Load Generation
The default demo load generator was used.

---


## 📂 Repository Structure

```
/
├── src/
│   └── checkoutservice/
│       ├── main.go (modified)
│       └── Dockerfile
├── postgres.yaml
├── kubernetes-manifests.yaml
├── signoz-frontend-lb.yaml
└── README.md
```

---


