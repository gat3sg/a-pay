---
description: >-
  Communication takes place with the help of postback requests (postbacks).
  After each Client request for input or output, a postback is sent to the
  Client with the result of the operation.
---

# 🔙 Postback

Funds should only be credited to your users based on the amount specified in the postback. Similarly, funds should only be debited from your users according to the amount indicated in the postback.

### Receiving the postback <a href="#postbacken-receivingthepostback-postbake" id="postbacken-receivingthepostback-postbake"></a>

To accept postbacks, you need to implement a separate path that you can use to receive postbacks. They are sent via a POST request in JSON format.

The A-pay server is waiting for a response in json

```json
{ 
"status": "OK" 
}
```

Response code 200, otherwise, when receiving a different response, A-pay will forward the postback with a certain frequency.

### Deposit

#### Client-side signature generation

When sending webhooks, A-Pay also sends a signature: a specifically generated hash string that is created using a private key.

**The signature itself is calculated as follows:**

```json
$signature = sha1($access_key . $private_key . md5($transactions->toJson(JSON_UNESCAPED_SLASHES | JSON_UNESCAPED_UNICODE)));
```

**a string of three parameters is passed to the sha1 function:**

| Parameter     | Description                                                                                  |
| ------------- | -------------------------------------------------------------------------------------------- |
| $access\_key  | The public key is specified in the settings of the A-Pay technical support personal account  |
| $private\_key | The private key is specified in the settings of the A-Pay technical support personal account |
| md5(...)      | Hash from the MD5 function of the entire array of transactions in JSON format                |

As a result of executing this code, a string is obtained that cannot be faked without having a private key that is not passed in webhooks.

The client can compare the generated signature with the received signature from the webhook and thereby make sure that the data that came was actually sent and not faked by a scammer.

The webhook will include the status of the transactions:

* If the status is “Success”, the client should proceed with depositing the funds into the user account.
* If the status is “Failed” or “Rejected,” no funds should be deposited, or the deposit should be canceled.

#### Postback of transactions for deposit <a href="#postbacken-postbackoftransactionsfordeposit" id="postbacken-postbackoftransactionsfordeposit"></a>

**Request body schema:**

| Name         | Type                     | Description                                                                                                                    |
| ------------ | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| access\_key  | string                   | The access key is specified in the settings of the A-Pay technical support personal account                                    |
| signature    | <p>string</p><p><br></p> | The signature was used to verify the validity of the postback. Concept of generation in the description of the deposit webhook |
| transactions | Array of objects below   | Information about the deposits made (one or more) is passed                                                                    |

**List of transactions:**

| order\_id               | string             | Deposit ID in the A-Pay system                                                                                                                                                                                                                                |
| ----------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| status                  | string             | <p>Enum: <code>"Success"</code> <code>"Failed"</code> <code>"Rejected"</code></p><p>Deposit status, if the deposit was successful, the status will be <mark style="color:red;"><code>Success</code></mark></p>                                                |
| amount                  | number >= 0        | Deposit amount                                                                                                                                                                                                                                                |
| currency                | string \[ 3 .. 3 ] | Currency name                                                                                                                                                                                                                                                 |
| payment\_system         | string             | <p>Enum: <code>"upi_fast", "upi_fast_v", "upi_p2p", "upi_a", "upi_fast_qr", "bankalfalah", "bkash_a", "apaybdt", "upay", "mpesa", "phonepe", "nagad_api_v", "payme", "paytm", "easypaisa"</code></p><p>The payment system from which the deposit was made</p> |
| custom\_transaction\_id | string             | Transaction ID in the client's system                                                                                                                                                                                                                         |
| custom\_user\_id        | string             | User ID in the client's system                                                                                                                                                                                                                                |
| created\_at             | integer            | Deposit creation time, in timestamp UTC format                                                                                                                                                                                                                |
| activated\_at           | integer            | Deposit activation date, in timestamp UTC format                                                                                                                                                                                                              |

**Example of a postback sent by a POST request in JSON format:**

{% code fullWidth="false" %}
```json
{
  "access_key": "mrOYReXJphqo7lkL",
  "signature": "dfsfrwe3344d",
  "transactions": [
    {
      "order_id": "7fa13dbc3b79e05e",
      "status": "Success",
      "amount": 6008.39,
      "currency": "INR",
      "payment_system": "mpesa",
      "custom_transaction_id": "string",
      "custom_user_id": "string",
      "created_at": 1665731710,
      "activated_at": 1665731710
    }
  ]
}
```
{% endcode %}

**Responses:**

<mark style="color:green;">If</mark> <mark style="color:green;"></mark><mark style="color:green;">`{"status":"OK"}`</mark> <mark style="color:green;"></mark><mark style="color:green;">was passed, we consider the webhook successfully delivered.</mark>

**Example of a response:**

```json
{
    "status": "OK"
}
```

### Withdrawal

#### Client-side signature generation

When sending webhooks, A-Pay also sends a signature: a specifically generated hash string that is created using a private key.

**The signature itself is calculated as follows:**

```json
$signature = sha1($access_key . $private_key . md5($transactions->toJson(JSON_UNESCAPED_SLASHES | JSON_UNESCAPED_UNICODE)));
```

**a string of three parameters is passed to the sha1 function:**

| Parameter     | Description                                                                                  |
| ------------- | -------------------------------------------------------------------------------------------- |
| $access\_key  | The public key is specified in the settings of the A-Pay technical support personal account  |
| $private\_key | The private key is specified in the settings of the A-Pay technical support personal account |
| md5(...)      | Hash from the MD5 function of the entire array of transactions in JSON format                |

As a result of executing this code, a string is obtained that cannot be faked without having a private key that is not passed in webhooks.

The client can compare the generated signature with the received signature from the webhook and thereby make sure that the data that came was actually sent, and not faked by a scammer.

The webhook will include the status of the transactions:

* If the status is “Success”, the client should proceed with withdrawal the funds from the user account.
* If the status is “Failed” or “Rejected,” no funds should be withdrawal, or the withdraw should be canceled.

#### Postback of transactions for deposit <a href="#postbacken-postbackoftransactionsfordeposit" id="postbacken-postbackoftransactionsfordeposit"></a>

**Request body schema:**

| Name         | Type                     | Description                                                                                                                    |
| ------------ | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| access\_key  | string                   | The access key is specified in the settings of the A-Pay technical support personal account                                    |
| signature    | <p>string</p><p><br></p> | The signature was used to verify the validity of the postback. Concept of generation in the description of the deposit webhook |
| transactions | Array of objects below   | Information about the deposits made (one or more) is passed                                                                    |

**List of transactions:**

| order\_id               | string             | Withdrawal ID in the A-Pay system                                                                                                                                                                                                                                     |
| ----------------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| status                  | string             | <p>Enum: <code>"Success"</code> <code>"Failed"</code> <code>"Rejected"</code></p><p>Withdrawal status, if the deposit was successful, the status will be <mark style="color:red;"><code>Success</code></mark></p>                                                     |
| amount                  | number >= 0        | Withdrawal amount                                                                                                                                                                                                                                                     |
| currency                | string \[ 3 .. 3 ] | Currency name                                                                                                                                                                                                                                                         |
| payment\_system         | string             | <p>Enum: <code>"upi_fast", "upi_fast_v", "upi_p2p", "upi_a", "upi_fast_qr", "imps", "bankalfalah", "bkash_a", "apaybdt", "upay", "mpesa", "phonepe", "nagad_api_v", "payme", "paytm", "easypaisa"</code></p><p>The payment system from which the deposit was made</p> |
| custom\_transaction\_id | string             | Transaction ID in the client's system                                                                                                                                                                                                                                 |
| custom\_user\_id        | string             | User ID in the client's system                                                                                                                                                                                                                                        |
| created\_at             | integer            | Deposit creation time, in timestamp UTC format                                                                                                                                                                                                                        |
| activated\_at           | integer            | Deposit activation date, in timestamp UTC format                                                                                                                                                                                                                      |

**Example of a postback sent by a POST request in JSON format:**

{% code fullWidth="false" %}
```json
{
  "access_key": "mrOYReXJphqo7lkL",
  "signature": "dfsfrwe3344d",
  "transactions": [
    {
      "order_id": "7fa13dbc3b79e05e",
      "status": "Success",
      "amount": 6008.39,
      "currency": "INR",
      "payment_system": "mpesa",
      "custom_transaction_id": "string",
      "custom_user_id": "string",
      "created_at": 1665731710,
      "activated_at": 1665731710
    }
  ]
}
```
{% endcode %}

**Responses:**

<mark style="color:green;">If</mark> <mark style="color:green;"></mark><mark style="color:green;">`{"status":"OK"}`</mark> <mark style="color:green;"></mark><mark style="color:green;">was passed, we consider the webhook successfully delivered.</mark>

**Example of a response:**

```json
{
    "status": "OK"
}
```

#### **Expected responses to postbacks from a client:**

| Сode | Message                  |
| ---- | ------------------------ |
| 200  | Ok                       |
| 400  | error receiving          |
| 401  | error validation         |
| 404  | not found http exception |
| 500  | not enough fields        |
| 501  | empty postback           |
| 502  | incorrect signature      |
| 503  | data integrity error     |

* If successful, expect the client to have http status - 2XX.
* &#x20;All 200th codes should be accompanied by "status" = "ok"
* In case of failure, expect from the client http status other than 2XX (depending on the error) and an error message. For example, "error validation" or "not enough fields".&#x20;
