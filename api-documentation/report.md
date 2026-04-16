---
description: >-
  Use A-pay Reports API to create transaction reports in CSV format for payment
  checks and daily operations.
---

# 📅 Report

The report is created asynchronously: first you create a task, then check its status, and finally download the file when it is ready.

### How it works

The process has 3 simple steps:

1. Create a report task
2. Check the task status
3. Download the CSV file

This approach is useful for large amounts of data and helps avoid timeouts.

### Authentication

Reports API uses Bearer Token authentication.

First, sign in and get an access token. Then send this token in the `Authorization` header for all report requests.

Example:

```
Authorization: Bearer {token}
```

### Step 1. Create a report task

Use `POST /reports/create` to create a new report task.

#### Request rules

* You must send at least one date filter: `create_at` or `activated_at`
* You can send both filters together
* Maximum period is 31 days
* Report format must be `csv`

#### Example request

```
{
  "filters": {
    "create_at": {
      "from": "2026-01-01T00:00:00Z",
      "to": "2026-01-31T23:59:59Z"
    },
    "statuses": ["completed", "success"]
  },
  "type": "in",
  "format": "csv",
  "columns": [
    "custom_transaction_id",
    "amount",
    "amount_after_commision"
  ]
}
```

#### Main parameters

* `filters` — filters for report data
* `create_at` — transaction creation date
* `activated_at` — transaction activation date
* `statuses` — transaction statuses
* `type` — operation type: `in` or `out`
* `format` — file format, must be `csv`
* `columns` — list of fields included in the report

#### Example response

```
{
  "task_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "pending",
  "created_at": "2026-01-14T10:00:00Z"
}
```

### Step 2. Check report status

Use `GET /reports/status/{task_id}` to check if the report is ready.

#### Possible statuses

* `pending` — task is in queue
* `processing` — report is being created
* `completed` — report is ready
* `failed` — report creation failed
* `expired` — task is no longer available
* `cancelled` — task was cancelled

#### Recommended check interval

* Check every 5 seconds
* Maximum recommended wait time: 200 seconds

#### Example response while processing

```
{
  "task_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "processing",
  "created_at": "2026-01-14T10:00:00Z",
  "updated_at": "2026-01-14T10:01:30Z"
}
```

#### Example response when completed

```
{
  "task_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "completed",
  "file_id": "abc123def456",
  "created_at": "2026-01-14T10:00:00Z",
  "updated_at": "2026-01-14T10:02:30Z",
  "expires_at": "2026-01-15T10:00:00Z",
  "record_count": 15234
}
```

### Step 3. Download the report

When the task status is `completed`, use `GET /reports/download/{file_id}` to download the file.

#### Request headers

```
Authorization: Bearer {token}
Accept: text/csv
```

#### Example response

```
custom_transaction_id;amount;amount_after_commision
TXN-001;150.50;145.25
TXN-002;200.00;194.00
```

### CSV format

The report file has the following format:

* Encoding: `UTF-8`
* Delimiter: `;`
* First row: column names
* Decimal separator: `.`
* Date format: `ISO 8601`
* Column order matches the order sent in the `columns` parameter

### Required fields for payment checks

The minimum recommended fields are:

* `custom_transaction_id`
* `amount`
* `amount_after_commision`

Example:

```
{
  "columns": [
    "custom_transaction_id",
    "amount",
    "amount_after_commision"
  ]
}
```

These fields help you match transactions and calculate the fee:

`fee = amount - amount_after_commision`

### Available columns

You can request all available columns or only the columns you need.

Commonly used columns:

* `custom_transaction_id`
* `amount`
* `amount_after_commision`
* `status`
* `created_at`
* `updated_at`
* `activated_at`
* `custom_user_id`
* `order_id`
* `transaction_id`

### Error responses

Possible error codes:

* `400` — invalid request parameters
* `401` — unauthorized
* `404` — file not found
* `410` — file expired
* `422` — date range is too large

### Summary

A-pay Reports API lets you:

* create report tasks
* check task status
* download CSV reports
* choose only the fields you need for payment checks and reporting
