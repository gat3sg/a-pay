# 💰 Deposit

{% hint style="warning" %}
**We recommend you additionally make the following improvements:**

1. Add hints in text form for each payment system (PS) to explain to a user all payment steps.
2. Add a visual explanation of the payment process - GIFs showing the process and screenshots of the payment steps.
3. Screenshots and all the necessary minimum text information can be obtained directly from A-pay TS.
{% endhint %}

#### Let's see how it works:&#x20;

* User makes a deposit via our payment method on the client project (flow configured by the client)
* Client server sends a request to our server
* Our server processes the request (checks the deposit payment)
* After successful processing, our server sends a deposit webhook (postback, callback) to the client server
* Client server processes our request
* After successful processing, the client project credits funds to user's account

So, communication takes place with the help of postback requests (postbacks). After each client request for input or output, a postback is sent to the client with the result of the operation. You can also review the process below:

<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption><p>Sequence diagram of the Deposit via API integration</p></figcaption></figure>
