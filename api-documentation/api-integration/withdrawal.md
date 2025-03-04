# 📤 Withdrawal

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption><p>Sequence diagram of the Withdrawal via API integration</p></figcaption></figure>

### Request to create a withdrawal

The section is similar to the [**Request to create a deposit**](withdrawal.md#request-to-create-a-withdrawal) section. Only the data required to create a withdrawal request will change.

{% openapi src="../../.gitbook/assets/pretty_a-pay_api_19.12.2024.json" path="/Remotes/create-withdrawal" method="post" %}
[pretty_a-pay_api_19.12.2024.json](../../.gitbook/assets/pretty_a-pay_api_19.12.2024.json)
{% endopenapi %}

All the necessary information can be found in our [API documentation](https://api.a-pay.one/#tag/Withdrawal/paths/~1Remotes~1create-withdrawal/post).

**Example of the responses:**

{% code title="Success" %}
```json
{
  "success": true,
  "status": "Pending",
  "order_id": "7fa13dbc3b79e05e",
  "data": {}
}
```
{% endcode %}

{% code title="Error" %}
```json
{
  "success": false,
  "status": "Failed",
  "message": "Invalid request",
  "code": 400,
  "order_id": "7fa13dbc3b79e05e"
}
```
{% endcode %}

### Withdrawal information

{% openapi src="../../.gitbook/assets/pretty_a-pay_api_19.12.2024.json" path="/Remotes/withdrawal-info" method="get" %}
[pretty_a-pay_api_19.12.2024.json](../../.gitbook/assets/pretty_a-pay_api_19.12.2024.json)
{% endopenapi %}

**Examples of the responses:**

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
  "code": 400,
  "message": "Invalid request"
}
```
{% endcode %}
