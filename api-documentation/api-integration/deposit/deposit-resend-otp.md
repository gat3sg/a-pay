# Deposit resend OTP

Some payment methods resends the OTP code.

{% openapi-operation spec="a-pay-api" path="/Remotes/deposit-resend-otp" method="post" %}
[Broken link](broken-reference)
{% endopenapi-operation %}

**Examples of the responses:**

{% code title="Success" %}
```json
{
  "success": true,
  "order_id": "7fa13dbc3b79e05e",
  "data": {
    "sent": true,
    "phone": "string",
    "wait": 0
  }
}
```
{% endcode %}

{% code title="Error" %}
```json
{
  "success": false,
  "message": "Invalid request",
  "code": 400,
  "order_id": "7fa13dbc3b79e05e"
}
```
{% endcode %}
