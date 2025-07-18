# Polyshop: gRPC Microservices with GraphQL Gateway



Polyshop is a backend system built using a microservices architecture in **Go**, utilizing **gRPC** for internal communication and **GraphQL** as the API gateway. It includes services for **account management**, **product catalog**, and **order processing**—each running independently and communicating efficiently.

## 🧩 Tech Stack

- **Go (Golang)** – Primary language for all microservices  
- **gRPC** – High-performance inter-service communication  
- **GraphQL** – Unified API gateway  
- **PostgreSQL** – Used by Account & Order services  
- **Elasticsearch** – Used by the Catalog service  
- **Docker & Docker Compose** – Service containerization and orchestration

## 📁 Project Structure

```
go-commerce/
│
├── account-service/       # User account service (PostgreSQL)
├── catalog-service/       # Product catalog service (Elasticsearch)
├── order-service/         # Order management service (PostgreSQL)
├── graphql-gateway/       # API gateway exposing GraphQL
├── pb/                    # Protobuf definitions for gRPC
└── docker-compose.yml     # Orchestrates all services
```

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd go-commerce
```

### 2. Start All Services with Docker

```bash
docker-compose up -d --build
```

> This command will start PostgreSQL, Elasticsearch, all microservices, and the GraphQL gateway.

### 3. Access the GraphQL Playground

Open your browser and go to:  
**http://localhost:8000/playground**

## 🔧 Generate gRPC Code

If you make changes to `.proto` files, regenerate gRPC files as follows:

```bash
# Step 1: Download protoc
wget https://github.com/protocolbuffers/protobuf/releases/download/v23.0/protoc-23.0-linux-x86_64.zip
unzip protoc-23.0-linux-x86_64.zip -d protoc
sudo mv protoc/bin/protoc /usr/local/bin/

# Step 2: Install gRPC plugins
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest

# Step 3: Export Go path
export PATH="$PATH:$(go env GOPATH)/bin"
source ~/.bashrc

# Step 4: Create pb directory and generate files
mkdir -p pb
protoc --go_out=./pb --go-grpc_out=./pb account.proto
```

> Ensure each `.proto` file contains:
```proto
option go_package = "./pb";
```

## 📡 Sample GraphQL Queries

### 🔍 Query All Accounts

```graphql
query {
  accounts {
    id
    name
  }
}
```

### ➕ Create an Account

```graphql
mutation {
  createAccount(account: {name: "Jane Doe"}) {
    id
    name
  }
}
```

### 🛍 Get Products

```graphql
query {
  products {
    id
    name
    price
  }
}
```

### 🆕 Create a Product

```graphql
mutation {
  createProduct(product: {
    name: "Sneakers", 
    description: "Comfortable running shoes", 
    price: 59.99
  }) {
    id
    name
    price
  }
}
```

### 📦 Create an Order

```graphql
mutation {
  createOrder(order: {
    accountId: "account_id", 
    products: [{id: "product_id", quantity: 2}]
  }) {
    id
    totalPrice
    products {
      name
      quantity
    }
  }
}
```

## 📈 Advanced Queries

### 🧾 Get Account with Order History

```graphql
query {
  accounts(id: "account_id") {
    name
    orders {
      id
      createdAt
      totalPrice
      products {
        name
        quantity
        price
      }
    }
  }
}
```

### 🔍 Product Search + Pagination

```graphql
query {
  products(pagination: {skip: 0, take: 5}, query: "shoe") {
    id
    name
    description
    price
  }
}
```

### 💰 Calculate Total Spent by an Account

```graphql
query {
  accounts(id: "account_id") {
    name
    orders {
      totalPrice
    }
  }
}
```

## 📌 Developer Note

**Why use an older Go version?**  
Stability is key in systems with many moving parts—especially when using technologies like gRPC, PostgreSQL, Docker, and Elasticsearch. This project is intentionally built with a tested Go version to minimize friction from breaking changes. If you're managing this alone or in a small team, locking to stable tool versions can save significant debugging time.

## 🙌 Contribution

You're welcome to contribute by:
- Reporting bugs
- Submitting improvements
- Adding new features

## 📄 License

This project is licensed under the [MIT License](LICENSE).
