# 📤 Withdrawal

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption><p>Sequence diagram of the Withdrawal via API integration</p></figcaption></figure>

### Request to create a withdrawal

The section is similar to the [**Request to create a deposit**](withdrawal.md#request-to-create-a-withdrawal) section. Only the data required to create a withdrawal request will change.

{% openapi-operation spec="a-pay-api" path="/Remotes/create-withdrawal" method="post" %}
[Broken link](broken-reference)
{% endopenapi-operation %}

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

{% openapi src="../../.gitbook/assets/apay_openapi.json" path="/Remotes/withdrawal-info" method="get" %}
[apay_openapi.json](../../.gitbook/assets/apay_openapi.json)
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
