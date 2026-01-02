# Postman Test Cases for Inventory Management System

This document outlines the test cases for the backend API. Ideally, import these into Postman or use them to verify endpoints manually.

## 🛠️ Environment Setup
Create a new Postman Environment with the following variables:
- **URL**: `http://localhost:3000`
- **TOKEN**: (Leave empty initially, populate after Login)

---

## 📂 1. Authentication (`/api/auth`)

### 1.1 Register User (Admin)
- **Method**: `POST`
- **URL**: `{{URL}}/api/auth/register`
- **Body** (JSON):
  ```json
  {
    "name": "Admin User",
    "email": "admin@example.com",
    "password": "password123",
    "role": "admin"
  }
  ```
- **Expected Status**: `201 Created`
- **Action**: Save the `token` from the response to your `TOKEN` environment variable.

### 1.2 Register User (Staff - Optional)
- **Method**: `POST`
- **URL**: `{{URL}}/api/auth/register`
- **Body** (JSON):
  ```json
  {
    "name": "Staff User",
    "email": "staff@example.com",
    "password": "password123",
    "role": "staff"
  }
  ```
- **Expected Status**: `201 Created`

### 1.3 Login User
- **Method**: `POST`
- **URL**: `{{URL}}/api/auth/login`
- **Body** (JSON):
  ```json
  {
    "email": "admin@example.com",
    "password": "password123"
  }
  ```
- **Expected Status**: `200 OK`
- **Action**: Update `TOKEN` variable if needed.

### 1.4 Get Current User Profile
- **Method**: `GET`
- **URL**: `{{URL}}/api/auth/me`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Expected Status**: `200 OK`

---

## 📂 2. Products (`/api/products`)

### 2.1 Create Product (Admin Only)
- **Method**: `POST`
- **URL**: `{{URL}}/api/products`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Body** (JSON):
  ```json
  {
    "name": "Laptop Pro",
    "category": "Electronics",
    "price": 1200,
    "quantity": 50,
    "minStock": 5,
    "description": "High performance laptop"
  }
  ```
- **Expected Status**: `201 Created`
- **Note**: Copy the `_id` from the response for future tests (e.g., store in `PRODUCT_ID` variable).

### 2.2 Get All Products
- **Method**: `GET`
- **URL**: `{{URL}}/api/products`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Expected Status**: `200 OK`

### 2.3 Get Product by ID
- **Method**: `GET`
- **URL**: `{{URL}}/api/products/{{PRODUCT_ID}}`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Expected Status**: `200 OK`

### 2.4 Update Product (Admin Only)
- **Method**: `PUT`
- **URL**: `{{URL}}/api/products/{{PRODUCT_ID}}`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Body** (JSON):
  ```json
  {
    "price": 1150,
    "description": "Updated description for Laptop Pro"
  }
  ```
- **Expected Status**: `200 OK`

### 2.5 Get Low Stock Products
- **Method**: `GET`
- **URL**: `{{URL}}/api/products/low-stock`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Expected Status**: `200 OK`

### 2.6 Delete Product (Admin Only)
- **Method**: `DELETE`
- **URL**: `{{URL}}/api/products/{{PRODUCT_ID}}`
  - *Note: Only run this if you want to remove the product created.*
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Expected Status**: `200 OK`

---

## 📂 3. Stock Operations (`/api/stock`)

### 3.1 Stock In (Add Stock)
- **Method**: `POST`
- **URL**: `{{URL}}/api/stock/in`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Body** (JSON):
  ```json
  {
    "productId": "{{PRODUCT_ID}}",
    "quantity": 10,
    "reason": "Restock from supplier"
  }
  ```
- **Expected Status**: `200 OK`

### 3.2 Stock Out (Reduce Stock)
- **Method**: `POST`
- **URL**: `{{URL}}/api/stock/out`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Body** (JSON):
  ```json
  {
    "productId": "{{PRODUCT_ID}}",
    "quantity": 5,
    "reason": "Sales Order #1234"
  }
  ```
- **Expected Status**: `200 OK`

### 3.3 Get Stock History
- **Method**: `GET`
- **URL**: `{{URL}}/api/stock/history`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Expected Status**: `200 OK`

---

## 📂 4. Reports & Exports

### 4.1 Get Dashboard Summary
- **Method**: `GET`
- **URL**: `{{URL}}/api/reports/summary`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Expected Status**: `200 OK`

### 4.2 Get Stock Movement Report
- **Method**: `GET`
- **URL**: `{{URL}}/api/reports/stock-movement`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Expected Status**: `200 OK`

### 4.3 Export Products to PDF
- **Method**: `GET`
- **URL**: `{{URL}}/api/export/products/pdf`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Expected Status**: `200 OK` (Should trigger download/return file)

### 4.4 Export Products to Excel
- **Method**: `GET`
- **URL**: `{{URL}}/api/export/products/excel`
- **Headers**:
  - `Authorization`: `Bearer {{TOKEN}}`
- **Expected Status**: `200 OK` (Should trigger download/return file)

---

## ⚠️ Common Error Codes
- `400 Bad Request`: Validation error or missing fields.
- `401 Unauthorized`: Invalid or missing token.
- `403 Forbidden`: Insufficient permissions (e.g., Non-Admin trying to Create/Delete).
- `404 Not Found`: Resource (User, Product) does not exist.
