# Deposit information

{% openapi src="../../../.gitbook/assets/pretty_a-pay_api_19.12.2024.json" path="/Remotes/deposit-info" method="get" %}
[pretty_a-pay_api_19.12.2024.json](../../../.gitbook/assets/pretty_a-pay_api_19.12.2024.json)
{% endopenapi %}

**Example of the responses:**

{% code title="Success" %}
```json
{
  "success": true,
  "status": "Success",
  "order_id": "7fa13dbc3b79e05e",
  "amount": 10,
  "currency": "INR",
  "payment_system": "mpesa",
  "custom_transaction_id": "custom123",
  "custom_user_id": "user123",
  "created_at": 1665135515
}
```
{% endcode %}

{% code title="Error" %}
```json
{
  "success": false,
  "message": "Invalid request",
  "code": 400
}
```
{% endcode %}
