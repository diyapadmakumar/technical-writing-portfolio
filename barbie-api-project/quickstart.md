# Barbie Store API: 5-Minute Quickstart

This guide will help you authenticate and place your first mock order in under five minutes. 

## Prerequisites
* A tool for sending HTTP requests (like **cURL** or **Postman**).
* Your **API Bearer Token**.
* Base URL: `https://api.barbiestore.com/v1`

## Authenticate
Include your Bearer Token in your header for every call.
**Header:**
`Authorization: Bearer YOUR_ACCESS_TOKEN` 

## Step 1: Browse the Catalog
Run this to fetch a product. Note: IDs must follow the `prod_001` format.
```
curl -X GET "https://api.barbiestore.com/v1/products/prod_101"
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```
**Success Response (200 OK)**
```
{
  "id": "prod_101",
  "name": "Malibu Dreamhouse",
  "price": 199.99,
  "stock": 15,
  "created_at": "2026-03-30T10:00:00Z"
}
```
**Common Error (401 Unauthorized)**
```
{
  "type": "https://api.barbiestore.com/errors/unauthorized",
  "title": "Unauthorized Access",
  "status": 401,
  "detail": "Bearer token is missing or invalid."
}
```
## Step 2: Place Your First Order
This API performs an Atomic Inventory Check to prevent overselling.

```
curl -X POST "https://api.barbiestore.com/v1/orders" 
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" 
  -H "Content-Type: application/json" 
  -d '{"product_ids": ["prod_101", "prod_105"]}'
```
**Expected Success (201 Created)**
```
{
  "message": "Order created successfully.",
  "data": {
    "order_id": "order_005",
    "product_ids": ["prod_101", "prod_105"],
    "total_amount": 249.98,
    "order_status": "pending",
    "created_at": "2026-03-30T14:20:00Z"
  }
}
```
## Troubleshooting (RFC 7807)

If your request fails, please refer to our [Full Error Reference Guide](./error-reference-guide.md) for detailed resolution steps.

What's Next?
- Now that you've placed your first order, explore these advanced workflows:

* **[Update your inventory](./api-reference.md#b-post-productsproduct_idstock)** – Learn how to manually sync stock levels.
* **[View all orders](./api-reference.md#a-get-orders)** – Filter and manage your store's order history.
* **[Read the full API Reference](./api-reference.md)** – Explore all available endpoints and RFC 7807 error schemas.