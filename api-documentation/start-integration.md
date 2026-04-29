# 🚀 Start Integration

A-Pay provides APIs (x2x, h2h, server-to-server, post-to-post) as well as a payment gateway (page) integration to process your transactions.

* **Payment gateway** — A fast and simple way to accept payments. Customers are redirected to a secure  payment page hosted by A-Pay, and you receive only the result. Minimal development required, with all security handled by us.
* **API** — Full control over the payment process directly on your website. You create the interface for entering payment details and manage the entire payment flow via API. Ideal for custom workflows.
* **API + Payment Gateway** — You can use both options depending on your need.

You can integrate the payment gateway and use the API, so the method of integration depends entirely on your preference!

{% hint style="warning" %}
In all cases, integration requires implementation into your backend.
{% endhint %}

## **Getting Started**

Before you begin, please follow these steps:

1. **Contact Support:** Reach out to our support team through your client chat. They will assist you with the integration process.
2. **Customize Integration:** Our technical support team is available to help you configure the integration in the way that best suits your needs.
3. **Provide URLs:** Submit the URLs for postback notifications for deposits and withdrawals. These URLs are where we will send postback data as needed.

## **Authentication**

To authenticate your requests with A-Pay, use the following parameters:

• **apikey:** Your unique API key.

• **project\_id:** Your project identifier.

## **Test Project**

* Access a test project to verify the integration before onboarding live users
* Use separate test API keys and Project ID, and replace them with production credentials before going live
* For transactions in the test project, we manually send callbacks with the statuses you need in order to test your webhook handling. To set the required status, please send a request in the support chat.
