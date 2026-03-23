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
Run this to fetch a product. Note: IDs must follow the `prod_001` format. \
```
curl -X GET "https://api.barbiestore.com/v1/products/prod_101" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Step 2: Place Your First Order
This API performs an Atomic Inventory Check to prevent overselling.

```curl -X POST "https://api.barbiestore.com/v1/orders" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Content-Type: application/json" -d '{"product_ids": ["prod_101", "prod_105"]}'```

## Troubleshooting (RFC 7807)

- 401 Unauthorized: Missing or invalid token.
- 409 Conflict: Item sold out during processing (Inventory mismatch).
- 422 Unprocessable Entity: ID format doesn't match ```prod_001```.

Link back to main docs: [Go to the Full API Reference](./api-reference.md)