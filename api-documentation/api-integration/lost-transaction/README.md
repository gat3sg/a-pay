# 🔍 Lost transaction

In some cases, a user may successfully complete a payment, but the transaction is not reflected in your system or is not activated. This may happen due to technical issues or user-related actions, such as network errors or when the user deposits an amount different from the one originally requested.

To handle such scenarios, you can use the APIs provided in this section. They allow you to initiate a lost transaction issue, after which the system will attempt to locate and process the missing transaction based on the provided information.

The APIs in this section are designed to:

* Automate the creation of lost transaction issue
* Enable real-time status tracking of submitted issues
* Help you provide better service to your end users

{% hint style="warning" %}
To use these APIs, you must provide either an `order_id` or a `custom_transaction_id`, along with a screenshot confirming the user's payment.
{% endhint %}
