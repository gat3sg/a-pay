---
description: >-
  This request will allow you to create transaction search requests in our
  system.
---

# Create lost transaction

{% openapi-operation spec="a-pay-api" path="/Remotes/create-lost-transaction" method="post" %}
[Broken link](broken-reference)
{% endopenapi-operation %}

## ⚠️ Error Handling Guide

The following section describes possible error responses for the `POST /Remotes/create-lost-transaction` endpoint and how you can resolve them by yourself.

<details>

<summary>400 Bad Request – Validation Errors</summary>

#### **When it happens:**

* `order_id` or `custom_transaction_id` is missing or has an invalid format
* Field values do not meet length or pattern requirements

#### **How to resolve it:**

The response body contains detailed information about what went wrong.\
Make sure to inspect the following fields in the JSON response:

| Field               | Description                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------- |
| `target`            | Indicates which part of the request failed (e.g. `"form_data"`, `"query"`)                  |
| `errors[].message`  | Description of what exactly is wrong (e.g., “ensure this value has at least 16 characters”) |
| `errors[].location` | Path to the field that caused the error (e.g., `["order_id"]`)                              |

</details>

<details>

<summary>400 Bad Request – Duplicate Request</summary>

#### **When it happens:**

A lost transaction issue has already been created for the same ID.

#### **How to resolve it:**

Use `GET /lost-transaction-info` to check existing issue status

</details>

<details>

<summary>401 Unauthorized</summary>

#### **When it happens:**

Missing or incorrect API key in the request.

#### **How to resolve it:**

* Include the header: `Authorization: Bearer YOUR_API_KEY`
* Check that your key is valid and linked to the correct `project_id`

</details>

<details>

<summary>404 Not Found</summary>

#### **When it happens:**

The provided `order_id` or `custom_transaction_id` does not match any transaction.

#### **How to resolve it:**

* Check that you’re sending the correct ID
* Confirm the transaction exists in your system
* Contact our support team if you're certain the payment was made

</details>

<details>

<summary>422 Unprocessable Entity – File Upload Issues</summary>

#### **When it happens:**

Issues with the uploaded `file` field:

* Missing file
* Unsupported file format
* File size too large
* Too many files

#### **How to resolve it:**

* Make sure the uploaded file is in one of the supported formats: `jpg`, `png`, `heic`, `jpeg`, `pdf`
* Ensure the file size does not exceed **20 MB**
* Attach **no more than 16 files** in a single request

</details>
