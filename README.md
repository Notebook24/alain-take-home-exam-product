# Products API Quick Start

## Description:
This API allows you to manage products using Dockerized Node.js and MySQL. This app supports full CRUD operations, as well as automated testing made using Jest and Supertest

## Features

- CRUD operations on products (Create, Read, Update, Delete)
- Input validation and error handling
- Pagination support for fetching products
- Dockerized environment for easy setup
- MySQL database utilization
- Optional environment variables for configuration

## Technology Stack

- **Backend:** Node.js, Express.js
- **Database:** MySQL
- **Dependencies:** mysql2, express, supertest, jest (for testing)
- **Containerization:** Docker, Docker Compose

## How to Run

### 0. Install Docker and Git
1. Download and install Docker Desktop from this link: https://www.docker.com/products/docker-desktop/
2. During installation, make sure **WSL 2 backend** is selected if you are on Windows.
3. After installation, open Docker Desktop to make sure it’s running.
4. Verify Docker is working:
```bash
docker --version
docker-compose --version
```
5. Download and install Git.
6. Verify Git is installed:
```bash
git --version
```

### 1. Clone the repository
```bash
git clone https://github.com/Notebook24/LSCS_TakeHomeExam.git
cd LSCS_TakeHomeExam
```

### 2. Configure environment
**Copy example env file**
```bash
cp .env.example .env
```

**Edit the .env (can be opened in any editor) and fill in:**
```bash
DB_HOST=mysql-db
DB_USER=<yourusername>        
DB_PASSWORD=<yourpassword>    
DB_NAME=examDB
PORT=3000
```

**Example:**
```bash
DB_HOST=mysql-db
DB_USER=Alain
DB_PASSWORD=123
DB_NAME=examDB
PORT=3000
```

### 3. Build and start the app using Docker
```bash
docker-compose up -d --build
```

### Check running containers
```bash
docker ps
```
**Should see:**
```bash
mysql:8.0
node-app (Express server)
```

### 4. Example API Usage (curl)

**Note:** curl syntax presented in this section are windows-specific.
**Note:** Windows users may need to escape `&` in URLs with `^`. Mac/Linux users can use `&`.

**These are the input rules for creating or updating a product:**
- id: Primary key, auto-increment, automatically set (The system does not allow ID input)
- name: Product name, required
- price: Product price, required, must be greater than 0
- description: Product description, required
- stock_quantity: Quantity in stock, must be greater than or equal to 0
- weight: Product weight, must be greater than 0
- created_at: Timestamp when product is created
- updated_at: Timestamp when product is updated
- expiry_date: Optional expiry date, must not be before creation (This condition is checked by the node.js, not the database schema)
- brand: Brand name, required

- **Note:** The system automatically assigns value to `created_at` and `updated_at` fields upon creation. Any attempt to input a different value upon creation is overriden by the system to be the current time to ensure product transparency.
- **Note:** The user cannot update the `created_at`, `updated_at`, and `expiry_date` fields. These fields are automatically managed by the system and are usually uneditable in real-world scenarios to ensure product transparency.

**a. Create a product**
**Example: Creating a product**
```bash
curl -X POST http://localhost:3000/products -H "Content-Type: application/json" -d "{\"name\":\"Sample Product\",\"price\":99.99,\"description\":\"Test product\",\"stock_quantity\":10,\"weight\":1.5,\"expiry_date\":\"2025-12-31\",\"brand\":\"TestBrand\"}"
```
**Note:** CREATE requires proper HTTP method, typing URL in browser won't create.

**b. Get all products (pagination)**
**For accessing Page 1, 10 products per page**
```bash
curl "http://localhost:3000/products?page=1^&limit=10"
```
**For accessing Page 2, 10 products per page**
```bash
curl "http://localhost:3000/products?page=2^&limit=10"
```

**To access Page 2, 10 products per page, you may also put the following URL in the browser**
"http://localhost:3000/products?page=2^&limit=10"

**c. Get product by ID**
**Example: Getting the product with id of 1**
```bash
curl http://localhost:3000/products/1
```

**d. Update product**
**Example: Updating the product with id of 1, to have a price of 120.50**
```bash
curl -X PUT http://localhost:3000/products/1 -H "Content-Type: application/json" -d "{\"price\":120.50}"
```
**Note:** UPDATE requires proper HTTP method, typing URL in browser won't update.

**e. Delete product**
**Example: Deleting the product with id of 1**
```bash
curl -X DELETE http://localhost:3000/products/1
```
**Note:** DELETE requires proper HTTP method, typing URL in browser won't delete.

### 5. Run tests
```bash
docker-compose run --rm node-app npm test
```

### 6. Stop the app
```bash
docker-compose down
```

**Data persists in Docker volumes, so products remain after stopping/restarting.**

**Base URL: http://localhost:3000**

