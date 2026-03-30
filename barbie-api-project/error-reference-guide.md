# Error Reference Guide

The Barbie Store API uses standard HTTP response codes to indicate the success or failure of an API request. Errors are returned as RFC 7807 compliant JSON objects.

## Error Object Schema
| Field | Type | Description |
|------------|------|----------|
| `type` | string | A URL reference that identifies the specific error type. | 
| `title` | string | A short, human-readable summary of the problem. | 
| `status` | number | The HTTP status code. |
| `detail` | string | A detailed explanation of why the error occurred. |


## Troubleshooting Table
| Status Code | Title | Cause | Fix |
|------------|------|----------|-------------|
| `400` | Invalid Filter Value | Wrong order status value | Use: pending, confirmed, shipped, delivered, cancelled|
| `401` | Unauthorized | Missing, malformed, or expired Bearer Token. | Ensure the `Authorization` header includes the `Bearer` prefix followed by a valid token. Regenerate your token in the Developer Portal if it has expired. |
| `404` | Resource Not Found | product_id doesn't exist | Check ID follows prod_001 format |
| `409` | Insufficient Stock | Item out of stock at order time | Check stock with GET /products first |
| `422` | Invalid Stock Value | Negative stock submitted | Use 0 or any positive integer |