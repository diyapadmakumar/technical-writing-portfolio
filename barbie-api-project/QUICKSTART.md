# Barbie Store API: 5-Minute Quickstart

This guide will help you authenticate and place your first mock order in under five minutes. 

## 1. Prerequisites
* A tool for sending HTTP requests (like **cURL** or **Postman**).
* Your **API Bearer Token**.
* Base URL: `https://api.barbiestore.com/v1`

## 2. Authenticate
Include your Bearer Token in your header for every call.
**Header:**
`Authorization: Bearer YOUR_ACCESS_TOKEN` 

## 3. Step 1: Browse the Catalog
Run this to fetch a product. Note: IDs must follow the `prod_xxx` format. \
```curl -X GET "https://api.barbiestore.com/v1/products/prod_b01" -H```\
```"Authorization: Bearer YOUR_ACCESS_TOKEN"```

## 4. Step 2: Place Your First Order
This API performs an Atomic Inventory Check to prevent overselling.

```curl -X POST "https://api.barbiestore.com/v1/orders" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Content-Type: application/json" -d '{"product_id": "prod_b01", "quantity": 1, "customer_email": "ken@mojo-dojo.com"}'```

## Troubleshooting (RFC 7807)

- 401 Unauthorized: Missing or invalid token.
- 409 Conflict: Item sold out during processing (Inventory mismatch).
- 422 Unprocessable Entity: ID format doesn't match ```prod_xxx```.

Link back to main docs: [Go to the Full API Reference](./README.md)