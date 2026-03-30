# Barbie Store API Style Guide
**Version:** 1.2.0  
**Owner:** Diya
**Last Updated:** 03/30/2026

This guide defines the standards for all Barbie Store API documentation. It ensures that our content is accessible, high-trust, and optimized for developer success.

---

## 1. The Audience (Who are we writing for?)
Our primary readers are **Frontend Developers** and **E-commerce Integrators**.
* **The Context:** They are often working under tight deadlines to integrate retail functionality into third-party apps.
* **The Goal:** Reduce "Time-to-First-Call." Every word should serve the speed of implementation. We prioritize scannability over prose.

---

## 2. Voice and Tone
We use a **"Helpful Expert"** voice. We are precise like an engineer, but encouraging like a partner.

| Rule | ✗ Avoid This | ✓ Use This |
| :--- | :--- | :--- |
| **Be Direct** | Execute the request to observe the product resource. | Run this request to get product details. |
| **Minimize Jargon** | Utilize the Bearer token to authorize the session. | Use your token to authenticate. |
| **Active Ownership** | An error will be returned by the system. | The API returns an error. |

---

## 3. Terminology & Rationale
Consistent naming reduces the "cognitive load" for developers switching between our docs and their terminal.

| Term | Use Instead Of | Rationale |
| :--- | :--- | :--- |
| **Endpoint** | Route / URL | "Endpoint" is the term used in industry-standard tools like Postman and Swagger. Using it consistently helps developers map our docs to their workspace. |
| **Payload** | Data / Body | "Payload" specifically refers to the JSON transmitted in a request. It signals a "transfer of data," which fits our retail inventory context. |
| **Attribute** | Field / Property | Our API is modeled on retail objects. "Attribute" aligns with inventory terminology (e.g., color, size). |

---

## 4. Content Patterns
Every endpoint entry must follow this "Atomic" structure to ensure predictability:
1. **Action-Oriented Title:** (e.g., `Browse Products`)
2. **The "Why":** A one-sentence business value statement.
3. **The cURL Command:** A syntax-highlighted block using `\` for line breaks.
4. **Success Response:** A JSON example showing exactly what `200 OK` looks like.
5. **Error Link:** A direct link to the **RFC 7807** section for that endpoint.

---

## 5. When to Break the Rules
These guidelines exist to serve **Clarity**. 
> **Editorial Maturity:** If following a specific formatting rule makes a complex retail concept (like tax calculation logic) harder to understand, **Clarity wins.** Break the rule, explain why in the pull request, and keep the developer moving.