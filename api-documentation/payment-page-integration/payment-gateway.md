# Payment Gateway

Payment gateway is a page that allows your user to select the payment system they wish to use to make a payment, or a page that allows the user to complete the payment process directly from your site. API: [Create payment page](https://api.a-pay.one/#tag/Payment-page/paths/~1Remotes~1create-payment-page/post)

{% hint style="warning" %}
The payment page cannot be integrated without diving into the API, so if you choose this method of integration, you will still need to customize the backend of your site
{% endhint %}

Here's what the page looks like:

<details>

<summary>Payment Gateway on mobile</summary>

![](<../../.gitbook/assets/image (5).png>)

</details>

<details>

<summary>Payment Gateway on desktop</summary>

![](<../../.gitbook/assets/image (1).png>)

</details>

### What languages does the payment gateway support?

The payment page supports all languages of the geo where A-pay operates. If the language is not passed in the payment gateway creation request, the geo-country language will be selected by default.

### How to use the payment page?

{% hint style="info" %}
Before using the payment gateway, be sure to write to our [sales](https://t.me/apay_sales) for initial integration. Then, with all the necessary data, you can start using the payment page.
{% endhint %}

It is necessary to create a request to open a payment window so that the user can go to the payment gateway. As a client, you can choose different ways to use the A-pay payment window:

1. Send a request to receive a link from A-pay to open the payment page to the user with a choice of available payment systems
2. Send a request to receive a link from A-pay to pay in a specific payment system

{% hint style="warning" %}
In both cases you will need to filter primary and secondary traffic on your website. Our payment window does not support traffic filtering.
{% endhint %}

If you have an account in our system, you can create a link to the payment page in the Request Configurator tab.

#### Payment page with a choice of payment systems

In the first case, you just need to specify the currency, payment systems and a return link on your website in the request.

<details>

<summary>Example of the request for the first case</summary>

```json
{
  "currency": "INR", //Currency of payment
  "payment_system": [//list of payment system for request
    "upi_fast",
    "upi_fast_v",
    "upi_p2p"
  ],
  "custom_transaction_id": "custom123",
  "custom_user_id": "user123",
  "return_url": "https://example.com",
  "language": "EN"
}
```

</details>

{% hint style="warning" %}
For primary traffic - some payment systems, for secondary traffic - others. Which ones to use in this or that case, please specify in the support chat.
{% endhint %}

In response to this request, a link to our payment gateway will be provided, where the user will select a payment system and create a deposit on their own. Users should be redirected to this link if they wish to make a deposit.

{% hint style="info" %}
The list of payment systems will be displayed in the order you put them in your request
{% endhint %}

**Return URL with Query Parameters**

When the user completes the payment on the payment gateway page, the system redirects them to the specified `return_url`. At this point, query parameters are appended to the URL to pass information about the transaction status.

Example parameters:

* `status`: the result of the payment (`Created`, `Fail`, `Cancel`).
  * `Created`- parameter is passed in the link when the user successfully created a deposit but, for some reason, didn't complete the payment.
  * `Fail`- parameter is sent when the user encountered an error while creating the payment.
  * `Cancel`- parameter is sent when the user initiated payment on the payment provider's site but, for some reason, did not complete it.

The difference between `created` and `cancel` is that with `created`, the user hasn’t begun the payment process—they saw the payment details but chose not to proceed. In the case of `cancel`, the user started the payment on the payment provider’s site but interrupted it and returned to the merchant’s site.

Example of the returned URL:

```
{https://example.com?status=Created}
```

These parameters provide insight into the stage at which the user interrupted the payment process.

### Using the amount buttons

You can also create buttons for quick selection of the amount for the user, for this purpose, it is necessary to add the buttons' parameter to the request.&#x20;

<details>

<summary>Example of the request with the amount buttons</summary>

<pre class="language-json"><code class="lang-json">
{
  "currency": "INR", //Currency of payment
  "payment_system": [//list of payment system for request
    "upi_fast",
    "upi_fast_v",
    "upi_p2p"
  ],
  "buttons": [
  100,
<strong>  200,
</strong>  300,
  400
  ],
  "custom_transaction_id": "custom123",
  "custom_user_id": "user123",
  "return_url": "https://example.com",
  "language": "EN"
}
</code></pre>

</details>

Once this parameter is added, the user will have buttons to quickly enter the amount.

{% hint style="info" %}
Opening a payment window without an amount isn't considered a deposit request and will not affect the conversion rate in this case.
{% endhint %}

You can actually block the payment amount, but leave the choice of payment system and send us this request. In this case, the user will not be able to change the payment amount but will be able to choose any convenient payment system that is currently connected to your project and is available to the user by traffic type. This is especially handy if you have a payment for services.

<details>

<summary>Example of the request with fixed amount</summary>

```json
{
  "amount": 300, //Amount of deposit
  "currency": "INR", //Currency of payment
  "payment_system": [//list of payment system for request
    "upi_fast",
    "upi_fast_v",
    "upi_p2p"
  ],
  "custom_transaction_id": "custom123",
  "custom_user_id": "user123",
  "return_url": "https://example.com",
  "language": "EN"
}
```

</details>

<details>

<summary>Example of the answer to the payment page request</summary>

```json
{
  "success": true,
  "url": "https://a-pay.one/pay?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MjI5LCJleHAiOjE3MDE0NDMwODd9.zZXK5K1ipGYj01G2MGFzERdRW5vIXQgxKo1B6GSmYcQ",
  "order_id": "7fa13dbc3b79e05e"
}
```

</details>

The user path in this case is:

1. The user wants to make a deposit
2. The user clicks the deposit button
3. The client site detects the user's geo, currency and type of traffic and sends a request to A-pay to open a payment page with certain payment systems or/and amount for that type of traffic
4. A-pay payment window opens with payment systems in the user's currency
5. The user selects the payment system
6. The user completes payment
7. Once the payment has been completed, the user will be redirected to the returnlink

### Payment gateway with complete data

In the second case, you make a more comprehensive request for a specific payment system.

{% hint style="info" %}
Priority method of use
{% endhint %}

This method has a higher conversion rate because it is the final stage of the payment process. For your part, you provide more detailed information to request a payment link, which then redirects the user to your website when they click on the "Pay" button.

{% hint style="danger" %}
The required parameters for this method of using the payment gateway are described in the example. The payment gateway will not work in such a case when any of the parameters are provided incorrectly or not provided at all.
{% endhint %}

<details>

<summary>Example of the request for the second case</summary>

```json
{
  "amount": 300, //Required. The amount of deposit that the user has entered on your site
  "currency": "INR", //Required. Currency in which the user makes a deposit
  "payment_system": [ //Required. Only 1 payment system. Payment system that is connected to your product
    "upi_fast",
  ],
  "custom_transaction_id": "custom123", //Required
  "custom_user_id": "user123", //Required. Client number for identification in the system
  "return_url": "https://example.com", //Required. returnlink to your site
  "language": "EN"
}
```

</details>

When making such a request, A-pay will send a link, following which the user will only have to complete the payment. The entire flow in the payment window is customized by us to achieve the best conversion rate for your users.

<details>

<summary>Example of the answer to the payment page request</summary>

```json
{
  "success": true,
  "url": "https://a-pay.one/pay?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MjI5LCJleHAiOjE3MDE0NDMwODd9.zZXK5K1ipGYj01G2MGFzERdRW5vIXQgxKo1B6GSmYcQ",
  "order_id": "7fa13dbc3b79e05e"
}
```

</details>

{% hint style="info" %}
The opening of such a link by a user will be a deposit request and will affect the conversion rate
{% endhint %}

<details>

<summary>Example of UI for payment gateway for mobile</summary>

![](<../../.gitbook/assets/image (2).png>)![](<../../.gitbook/assets/image (3).png>)

</details>

The user path in this case is as follows:

1. The user wants to make a deposit
2. The user clicks the deposit button
3. On the client's site, the user selects the payment system and the amount of top-up&#x20;
4. The user confirm the deposit
5. The client site sends a request to create a payment link
6. A-pay sends the link
7. The client site redirects the user to this link
8. The user completes the payment
9. The user is redirected to the returnlink

### Iframe integration

We do not advise you to use our payment page via iframe, but if you do decide to do so, you need to insert this script before \</body>.

```html
<!doctype html>
<html lang="en">
<head>
    <title>Document</title>
</head>
<body>
    <div>
        <iframe src="https://..." frameborder="0"></iframe>
    </div>


    <script type="text/javascript" src="https://a-pay1.one/pay/iframe-script.js"></script>


</body>
</html>
```
