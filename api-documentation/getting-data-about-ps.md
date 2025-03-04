# 🈁 Getting data about PS

{% hint style="warning" %}
The integration of our service to your platform is expected to be done with the help of developers, so make sure you involve them
{% endhint %}

There is a request, by creating and sending which you will find out which payment system is connected to your project and what limits there are on it. API docs about that request is [here](https://api.a-pay.one/#tag/Payment-system/paths/~1Remotes~1payment-systems-info/get).

{% openapi src="../.gitbook/assets/pretty_a-pay_api_19.12.2024.json" path="/Remotes/payment-systems-info" method="get" %}
[pretty_a-pay_api_19.12.2024.json](../.gitbook/assets/pretty_a-pay_api_19.12.2024.json)
{% endopenapi %}

**Example of the responses:**

{% code title="Success" %}
```json
{
  "success": true,
  "payment_systems": [
    {
      "name": "mpesa",
      "currency": "INR",
      "min_deposit": "10.00",
      "max_deposit": "10000.00",
      "min_withdrawals": "10.00",
      "max_withdrawals": "10000.00"
    }
  ]
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

### Auto display method and auto update minimum and maximum payment limits

You can use this request to automate the display of our payment methods and their limits on your platform. The functionality is straightforward: **once every 5 minutes**, you send us a request, and we respond with the active payment systems and their limits. Based on our response, you can enable or disable the display of our payment methods, as well as adjust the minimum and/or maximum deposit and withdrawal limits.

There is a slight difference depending on how you are integrated with us

#### **For H2H integration**

You need to implement the following logic. Send us a request for "getting data about the payment system." If the response includes a payment system that you have integrated, display that method. If the payment system does not appear in the response but is integrated, hide the method. Additionally, we send limits in the response, which you can automatically adjust on your frontend and backend.

#### **For integration via our payment page**

There are two ways to use the current functionality:

a. **Single payment system request**:\
When you request only one payment system via our payment page, the functionality is the same as the H2H integration (see above).

b. **Multiple payment systems request**:\
When you request a list of payment systems through our payment page, the logic differs slightly. You should hide a payment method on your platform only when none of the requested payment systems are returned in the response. Display the payment method if at least one of the requested payment systems is included in the response.

For example, if you request the following payment systems via the payment page: `upi_p2p`, `phonepe`, `paytm`, and you periodically send us requests to check their availability. If at some point we respond with only `upi_p2p`, do not hide the method, as `upi_p2p` is still available. However, if none of the requested payment systems (i.e., `upi_p2p`, `phonepe`, `paytm`) are returned in the response, the method should be hidden from the frontend. Similarly, if at least one payment system from the list (e.g., `phonepe`) appears in the response, you should display the method.
