---
description: This request will allow you to find out the status of the transaction search
---

# Lost transaction information

{% openapi-operation spec="a-pay-api" path="/Remotes/lost-transaction-info" method="get" %}
[Broken link](broken-reference)
{% endopenapi-operation %}

## ⚠️ Error Handling Guide

The following section describes possible error responses for the `POST /Remotes/lost-transaction-info` endpoint and how you can resolve them by yourself.

<details>

<summary>400 Bad Request – Validation Errors</summary>

#### **When it happens:**

* Neither `order_id` nor `custom_transaction_id` was provided
* Provided values are in an incorrect format (e.g., invalid length or pattern)

#### **How to resolve it:**

When the API returns a **400 Bad Request**, it includes detailed information in the response body to help you identify what went wrong.

Make sure to inspect the following fields in the error response:

| Field               | Description                                                                |
| ------------------- | -------------------------------------------------------------------------- |
| `errors[].message`  | Describes the issue (e.g., "ensure this value has at least 16 characters") |
| `errors[].location` | Shows which parameter is invalid (e.g., `["order_id"]`)                    |

</details>

<details>

<summary>401 Unauthorized</summary>

#### **When it happens:**

* The `Authorization` header is missing or contains an invalid API key

#### **How to resolve it:**

* Set the header: `Authorization: Bearer YOUR_API_KEY`
* Make sure the key has access to the specified `project_id`

</details>

<details>

<summary>404 Not Found – Transaction Not Found</summary>

#### **When it happens:**

* The specified `order_id` or `custom_transaction_id` does not match any existing lost transaction issue

#### **How to resolve it:**

* Confirm that the transaction ID you’re querying has actually been submitted previously
* Contact our support team if you're sure the lost transaction issue was created
* Create a new lost transaction issue if none is found

</details>
