# Deposit Activation

Some payment systems also provide activation of deposits. In simple words, 2 more actions are added to the request processing.

{% openapi src="../../../.gitbook/assets/pretty_a-pay_api_19.12.2024.json" path="/Remotes/deposit-activate" method="post" %}
[pretty_a-pay_api_19.12.2024.json](../../../.gitbook/assets/pretty_a-pay_api_19.12.2024.json)
{% endopenapi %}

A perfect example of a payment system that provides for activation is upi\_p2p. Let's analyze an example of request processing and activation:

1. We see that activation is required for this PS in [Request to create a deposit](request-to-create-a-deposit.md).
2. [Here](https://api.a-pay.one/#tag/Deposit/operation/DepositActivation) we can see that in this PS A-pay sends the UPI address parameter to the client after processing the request. In this case, it means that A-pay sends the address to which the user needs to transfer money in Responses.
3. After the user has transferred money, he must activate his deposit. It transmits the necessary information to the client server. For upi\_p2p this is the Transaction ID (UTR). The client server sends to A-pay an activation request, in which certain data is transmitted. After that, A-pay processes the activation request and responds to the client with success or failure, depending on the situation.

**Examples of the responses:**

{% code title="Success" %}
```json
{
  "success": true,
  "order_id": "7fa13dbc3b79e05e",
  "data": {
    "verify_otp": true,
    "require_new_otp": false
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
