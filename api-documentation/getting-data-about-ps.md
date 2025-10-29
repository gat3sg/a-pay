# 🈁 Getting data about PS

{% hint style="warning" %}
The integration of our service into your platform is expected to be done with the help of developers, so make sure you involve them
{% endhint %}

There is a request, by creating and sending which you will find out which payment system is connected to your project and what limits there are on it. API docs about that request are [here](https://api.a-pay.one/#tag/Payment-system/paths/~1Remotes~1payment-systems-info/get).

{% openapi-operation spec="a-pay-api" path="/Remotes/payment-systems-info" method="get" %}
[OpenAPI a-pay-api](https://api.a-pay.one/openapi.json)
{% endopenapi-operation %}

### Auto display method and auto update minimum and maximum payment limits

Use this request to automatically display our active payment methods and their limits on your platform. You can call the method periodically (e.g., every 5 minutes) to keep your data up to date.

The response also contains information about which directions are currently available — deposits, withdrawals, or both.

There is a slight difference depending on how you are integrated with us:

#### **H2H integration**

For H2H integration, follow this logic:

1. Send a request to get available payment systems.
2. If a payment system you have integrated is present in the response — display it.\
   If it’s missing — hide it.
3. Update minimum and maximum limits according to the response.
4. Use the `deposit` and `withdrawal` flags to determine whether the payment system should be displayed, and for which directions it’s available.

#### **Payment page integration**

For integrations via our payment page, there are two options:

**a. Single payment system request**

The behavior is similar to the H2H integration. However, withdrawals are not available when using our payment page. In this case, you should ignore the `withdrawal` flag and display the method **only if `deposit = true`**.

**b. Multiple payment systems request**

When you send a request containing several payment systems (e.g., `upi_p2p`, `phonepe`, `paytm`):

* Display the payment method if **at least one** of the requested systems is returned in the response with `deposit = true`.
* Hide the method only if **none** of the requested systems appear, or all of them have `deposit = false`.
* You can ignore the `withdrawal` flag, as withdrawals are not available when using our payment page.

For example, if you request the following payment systems via the payment page: `upi_p2p`, `phonepe`, `paytm`, and you periodically send us requests to check their availability:

* If we respond with only `upi_p2p` where `deposit = true` — do **not** hide the method, since a deposit option is still available.
* If we respond with only systems where `deposit = false`, or none of the requested systems are returned — hide the method.
