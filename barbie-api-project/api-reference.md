New here? Start with the [Quickstart Guide](./quickstart.md)

# Barbie Store API Reference (v1.2.0)

This document provides technical specifications for the Barbie Store API. This service uses a RESTful architecture, ISO 8601 timestamps, and follows RFC 7807 standards for error reporting.

## Global Specifications

- Base URL: ```https://api.barbiestore.com/v1```

- Content-Type: ```application/json```

- Authentication: Authentication: All write operations (POST) require a Bearer Token. Unauthorized requests return a 401 Unauthorized response. See the [error guide](./error-reference-guide.md).

- Date Format: All timestamps are returned in UTC.


## 1. Product Management

### a. GET /products/{product_id}

**Description**

Retrieves the details of a single product by its unique ID, including name, price, category, and current stock level.

**Path Parameters**

| Parameters | Type | Required | Description |
|------------|------|----------|-------------|
| product_id | string | required | The unique ID of the product |

**Constraints**

- ```product_id``` must follow a Regex pattern ```^prod_[0-9]{3}$```


_**Example Request**_

```GET /products/prod_102```

**Success Response (200 OK)**
```
{
    "id": "prod_102",
    "name": "Barbie Mermaid Fantasy",
    "category_id": "cat_01",
    "price": 24.50,
    "stock": 35,
    "created_at": "2026-02-21T11:00:00Z"
}
```

**Error Response (404 Not Found)**
```
{
  "type": "https://api.barbiestore.com/errors/not-found",
  "title": "Resource Not Found",
  "status": 404,
  "detail": "No product with ID prod_999 exists.",
  "help": "Ensure your ID starts with 'prod_' followed by a three-digit number.",
  "instance": "/products/prod_999"
}
```

### b. POST /products/{product_id}/stock

**Description**

Updates the stock inventory for the product identified by ```product_id```.

**Path Parameters**

| Parameters | Type | Required | Description |
|------------|------|----------|-------------|
| product_id | string | required | The unique ID of the product. Must match `^prod_[0-9]{3}$`|

**Business Logic**

- This endpoint is intended for manual restocks or inventory corrections.

- Side Effects: Updating stock via this endpoint does not trigger order recalculations but will be reflected immediately in the GET /products list.

**Request Body**
| Field | Type | Required | Description |
|------------|------|----------|-------------|
| stock | integer | yes | New inventory count. Must be ≥ 0. |

**Success Response (200 OK)**
```
{
  "message": "Inventory synchronized successfully.",
  "data": {
    "id": "prod_101",
    "stock": 150,
    "updated_at": "2026-03-02T10:45:00Z"
  }
}
```

**Error Response (422 Unprocessable Entity)**

```
{
    "type": "https://api.barbiestore.com/errors/validation-error",
    "title": "Invalid Stock Value",
    "status": 422,
    "detail": "Stock value cannot be negative.",
    "invalid_params": [
    { "name": "stock", "reason": "Must be a non-negative integer." }
  ]
}
```

## 2. Order Fulfillment

### a. GET /orders

**Description**

Returns a list of all orders. Supports filtering by the status of the order.

**Query Parameters**

| Parameters | Type | Required | Description |
|------------|------|----------|-------------|
| order_status | string | no | Filter orders by status (pending, confirmed, shipped, delivered, cancelled).|

**_Example Request_**

```GET /orders?order_status=shipped```

**Success Response (200 OK)**

```
{
    "count": 1,
    "data": [
        {
            "order_id": "order_002",
            "product_ids": ["prod_103"],
            "total_amount": 19.99,
            "order_status": "shipped",
            "created_at": "2026-02-25T09:10:00Z"
        }
    ]
}
```

**Error Response (400 Bad Request)**

```
{
  "type": "https://api.barbiestore.com/errors/invalid-filter",
  "title": "Invalid Filter Value",
  "status": 400,
  "detail": "The status 'delivered_soon' is not a valid order status.",
  "help": "Allowed values are: pending, confirmed, shipped, delivered, cancelled."
}
```


### b. POST /orders

**Description**

Creates a new order in the Barbie Store. The system automatically calculates the ```total_amount``` based on the product prices and initializes the ```order_status``` to "pending".

**Integrity & Logic**

- Atomic Inventory Check: The system verifies stock availability for all ```product_ids``` before finalizing. If any item is out of stock, the entire transaction is rolled back.

- Server-Side Pricing: ```total_amount``` is calculated using the database's "Live Price" at the timestamp of execution to prevent client-side price injection.

- Defaults: New orders are initialized with a ```pending``` status.

**Request Headers**

```Content-Type: application/json```

**Request Body**
```
{
    "product_ids": ["prod_101", "prod_105"]
}
```

**Success Response (201 Created)**
```
{
    "message": "Order created successfully.",
    "data": {
        "order_id": "order_005",
    "product_ids": ["prod_101", "prod_105"],
    "total_amount": 39.98,
    "order_status": "pending",
    "created_at": "2026-03-02T14:20:00Z"
    }         
}
```

**Error Response (409 Conflict)**

```
{
  "type": "https://api.barbiestore.com/errors/inventory-conflict",
  "title": "Insufficient Stock",
  "status": 409,
  "detail": "One or more items in the 'product_ids' list exceed current inventory levels or do not exist.",
  "help": "Verify that all product IDs start with 'prod_' and exist in the Products table."
}
```
