---
icon: lastfm
---

# Example

Here is a brief description of the payment process via Telegram Payments and A-Pay.

## **1. Create Invoice**

The user contacts your bot and requests to make a payment. The bot forms an invoice message with an amount to be paid

Use the `sendInvoice` method to generate an invoice and send it to a chat. The `provider_token` parameter is where you put the `token` value that you've obtained earlier via Botfather

Invoice messages with a pay button can be sent to chats of any type: private chats with the user, groups, or channels. The resulting invoice message will look like this:

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

You can adjust behaviour of the pay button in your invoices. Detailed information here: [Choose Forwarding Behavior](https://core.telegram.org/bots/payments#2-choose-forwarding-behavior)

## **2. Pre-Checkout**

The user chooses the A-Pay as Payment Method and presses the next pay button.

{% hint style="info" %}
**Note:** Your customers can’s save their payment information using telegram payments with A-Pay
{% endhint %}

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

At this moment the Bot API sends an Update with the field `pre_checkout_query` to your merchant bot that contains all the available information about the order. Your bot must reply using `answerPrecheckoutQuery` within **10 seconds** after receiving this update or the transaction is canceled.

The bot may return an error if it can't process the order for any reason. We highly recommend specifying a reason for failure to complete the order in human readable form (e.g. _"Sorry, we're all out of rubber ducks! Would you be interested in a cast iron bear instead?"_). Telegram will display this reason to the user.

{% hint style="warning" %}
**Warning:** It is critical to make sure your bot only accepts multiple payments when the order can be processed correctly. This is especially important if you are using multi-chat, inline or single-chat, multi-use invoices.
{% endhint %}

## **3. Checkout**

In case the bot confirms the order, Telegram requests the A-Pay Payment Page and opens it in the same window:

<figure><img src="../../.gitbook/assets/image (23).png" alt="" width="375"><figcaption></figcaption></figure>

Now the customer should follow the steps for the selected method. If the payment is completed correctly the Telegram API will send a receipt message of the type `successful_payment` from the user. Once your bot receives this message, it should proceed crediting the funds to the customer’s account.

If the invoice message was sent in the chat with your merchant bot, it becomes a Receipt in the UI for the user — they can open this receipt at any time and see all the details of the transaction:

If the message was sent to any other chat, the **Pay button** remains and can be used again. It is up to the merchant bot whether to actually accept multiple payments.

## **Going Live**

Once you've tested everything and confirmed that your payments implementation works, you're ready to switch to go live. To do this, follow steps above, but choose `Connect Paykassma Live` and enter the data from your live integration. Do not give this data to any third parties!

Before your merchant bot goes into live mode, please ensure that you've completed all points from the Live Checklist below.

## **Live Checklist**

* We highly recommend turning on [2-step verification](https://telegram.org/faq#q-how-does-2-step-verification-work) for the Telegram account that controls your bot.
* You as the bot owner have **full responsibility** in case any conflicts or disputes arise. You must be prepared to correctly process disputes and chargebacks.
* To prevent any misunderstandings and possible legal issues, make sure your bot can respond to a `/terms` command (or offers a similarly easy way of accessing your Terms and Conditions). Your Terms and Conditions should be written in a clear way and easy to understand for your users. The users must confirm that they have read and agree to your terms before they make the purchase.
* Your bot must provide support for its customers, either by responding to a `/support` command or by some other clearly communicated means. Users must have a clear way of contacting you about their purchases and you must process their support requests in a timely fashion. You must notify your users that Telegram support or [bot support](https://t.me/botsupport) will not able to help them with purchases made via your bot.
* Make sure that your server hardware and software is stable. Use backups to make sure that you don't lose data about your users' payments.
