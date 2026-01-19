
# 🛒 Spring Boot E-commerce Backend

A robust, production-ready backend for an e-commerce platform built with **Java Spring Boot** and **MongoDB**. This API manages the complete order lifecycle—from product inventory and shopping cart management to order processing and payment integration.

## 🚀 Key Features

* **Product Management:** Create, read, and manage product inventory with persistent storage.
* **Smart Shopping Cart:** Persistent cart system with real-time stock validation (prevents ordering out-of-stock items).
* **Order Lifecycle:** Seamless conversion of Carts to Orders with state management (`CREATED` → `PENDING` → `PAID`).
* **💳 Payment Integration (Razorpay):**
    * Full implementation of the **Razorpay Java SDK**.
    * **Resilience Architecture:** Includes a "Fail-Safe Simulation Mode" that activates if live banking credentials are unavailable or invalid (see *Compliance Note* below).
* **Webhooks:** Automated status updates via webhook listeners.

## 🛠️ Tech Stack

* **Language:** Java 17
* **Framework:** Spring Boot 3.0
* **Database:** MongoDB (NoSQL)
* **Build Tool:** Maven
* **Payment Gateway:** Razorpay SDK

---

## ⚠️ Compliance & Simulation Note

**Regarding Payment Integration:**
This project includes the **actual production code** for Razorpay integration (`PaymentService.java` uses `RazorpayClient`).

However, due to **strict KYC compliance requirements** for activating a live Merchant Account (which requires business documentation), the system is currently configured to operate in a **Simulation Mode**.
* If valid API Keys are provided, it connects to Razorpay.
* If keys are invalid or missing, it gracefully degrades to a local simulation, generating mock Transaction IDs to ensure the `Order -> Payment -> Webhook` flow can still be demonstrated and tested end-to-end.

---

## ⚙️ Setup & Installation

### Prerequisites
* Java 17 or higher
* Maven
* MongoDB (Running locally on port `27017`)

### 1. Clone the Repository
```bash
git clone [https://github.com/your-username/ecommerce-backend.git](https://github.com/your-username/ecommerce-backend.git)
cd ecommerce-backend

```

### 2. Configure Database & Keys

The application expects a MongoDB instance running locally.
Update `src/main/resources/application.properties` if needed:

```properties
spring.data.mongodb.uri=mongodb://localhost:27017/ecommerce

# Razorpay Credentials (Optional for Simulation Mode)
razorpay.key.id=YOUR_TEST_KEY_ID
razorpay.key.secret=YOUR_TEST_KEY_SECRET

```

### 3. Run the Application

```bash
mvn spring-boot:run

```

The server will start at `http://localhost:8080`.

---

## 🔌 API Endpoints

You can test these endpoints using Postman.

### Products

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/api/products` | Create a new product |
| `GET` | `/api/products` | List all products |

### Cart

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/api/cart/{userId}/add` | Add item to cart (Body: `{productId, quantity}`) |
| `GET` | `/api/cart/{userId}` | View user's cart |

### Orders & Payments

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/api/orders/{userId}` | Convert Cart to Order |
| `POST` | `/api/payment/create` | Initiate Payment (Returns Payment ID) |
| `POST` | `/api/webhooks/simulate-success?orderId={id}` | **Simulation:** Manually trigger successful payment |
| `GET` | `/api/orders/{orderId}` | Check Order Status |

---

## 📂 Project Structure

```text
com.example.ecommerce
├── controller      # REST Controllers (API Layer)
├── dto             # Data Transfer Objects (Request/Response)
├── model           # MongoDB Documents (Entities)
├── repository      # Data Access Layer (Interfaces)
├── service         # Business Logic (Payment, Order, Cart)
└── webhook         # Webhook Listeners & Simulation

```

## 👨‍💻 Author

**Vansh**
*Submitted for E-commerce Backend Assignment*

```

```
