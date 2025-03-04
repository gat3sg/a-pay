# ❗ Errors

### Payment system data is not valid <a href="#id-dokumentaciya-paymentsystemdataisnotvalid" id="id-dokumentaciya-paymentsystemdataisnotvalid"></a>

The data in the data parameter is invalid, or the field is missing. Our client either does not send the data parameter in the request or sends incorrect data in this parameter.

**How can you resolve it by yourself?** Using the deposit creation as an example:

[Link](https://api.a-pay.one/#tag/Deposit/paths/~1Remotes~1create-deposit/post) -> Data -> one of PS, for example, M-Pesa.

You must send for the mpesa payment system a cell phone number

If you do send a number, please note that it must be a 12-digit number. This is specified in string `[12..12]`

<details>

<summary>Example</summary>

Your request for deposit creation

```json
{
  "amount": 10,
  "currency": "INR",
  "payment_system": "mpesa",
  "custom_transaction_id": "custom123",
  "custom_user_id": "user123",
  "data": {
  }
}
```

Right request

```json
{
  "amount": 10,
  "currency": "INR",
  "payment_system": "mpesa",
  "custom_transaction_id": "custom123",
  "custom_user_id": "user123",
  "data": {
    "phone_number": "254712345678"
  }
}
```

Note that the number must be a 12-digit number.

</details>

If the payment system is different, the data will be different.

### Required field missing <a href="#id-dokumentaciya-requiredfieldmissing" id="id-dokumentaciya-requiredfieldmissing"></a>

The required fields are missing. Our client missed a mandatory field in the request (such fields are marked as required in the [documentation](https://api.a-pay.one)).

**How can you resolve it by yourself?** Using the deposit creation as an example:

[Link](https://api.a-pay.one/#tag/Deposit/paths/~1Remotes~1create-deposit/post) -> See which fields are mandatory in the request

<details>

<summary>Example</summary>

Your request:

```json
{
  "amount": 10,
  "payment_system": "mpesa",
  "custom_transaction_id": "custom123",
  "custom_user_id": "user123",
  "data": {
    "phone_number": "254712345678"
  }
}
```

Right request:

```json
{
  "amount": 10,
  "currency": "INR",
  "payment_system": "mpesa",
  "custom_transaction_id": "custom123",
  "custom_user_id": "user123",
  "data": {
    "phone_number": "254712345678"
  }
}
```

Our client forgot to specify the currency of deposit creation in the request.

</details>

### No payment system found with such params or payment system is disabled

The error tells us that:

1. The data in the request is not valid
2. The specified PS is not connected or enabled for the project
3. The PS does not support the specified currency
4. The request is beyond the limits of the payment system

**How can you resolve it by yourself?**

1. You should look at the request and compare the currency in the request with the data in our documentation
2. Check the payment system and its limits [by this method](getting-data-about-ps.md).
3. If your payment system is enabled and the amount is within limits, but the error still persists, contact us via your client chat.

### Amount of deposit is out of limits

The error tells us that the requested amount exceeded the limits of the payment system

**How can you resolve it by yourself?**

Check the payment system and its limits [with this method](getting-data-about-ps.md).

### Amount of withdrawal is more than balance sum <a href="#id-dokumentaciya-amountofwithdrawalismorethanbalancesum" id="id-dokumentaciya-amountofwithdrawalismorethanbalancesum"></a>

The amount of withdrawal is more than there is on the balance. This error occurs only when creating a withdrawal request.

In this case, our client needs to top up the account and write to our financier

### Transaction failed <a href="#id-dokumentaciya-transaction_failed" id="id-dokumentaciya-transaction_failed"></a>

Contact our support team via your client chat to resolve this issue

### There are no active end accounts <a href="#id-dokumentaciya-therearenoactiveendaccounts" id="id-dokumentaciya-therearenoactiveendaccounts"></a>

Contact our support team via your client chat to resolve this issue

If you don't find the error here, send the full request body and error text to your customer chat, and our support will help you out ASAP.
