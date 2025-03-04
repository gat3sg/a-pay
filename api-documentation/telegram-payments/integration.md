# 🖥️ Integration

{% hint style="info" %}
First of all, request the Telegram Bot connection from [a-pay support](https://t.me/apay_sales). We'll create a new webhook for your project with URL:[https://paykassmapayment-tgb.com/apay/deposit/postback\
](https://paykassmapayment-tgb.com/apay/deposit/postback)

After confirmation, follow these steps.
{% endhint %}

## Step 0

If you don’t have your own bot already, create one with [@BotFather](https://t.me/Botfather), following [Telegram instructions](https://core.telegram.org/bots).

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

## Step 1

Find the `Paykassma Payments` in Bot Settings

* Type `/mybots`
* Then choose your Bot

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

* Choose `Payments`, then find and choose `Paykassma` (use buttons `«` and `»`)

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

* Choose `Connect Paykassma Live` to connect the final integration or choose `Connect Paykassma Test` to test your bot. Test example described at [Example](https://app.gitbook.com/o/LNbZs3dNMDs1xm16Y9hk/s/dRt6CV4HPvnslXUaqL9U/~/changes/85/api-documentation/telegram-payments/example)

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

* You will go to @paykassma\_bot

## **Step 2**

Send all the necessary Data from A-Pay Dashboard

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

The @paykassma\_bot will request the following data:

* `apikey**:** (32 characters: [a-z0-9]{32})`
* `project id**:** (up to 7 digits: [0-9]{1,7})`
* `webhook id**:** (7 digits: [0-9]{7})`
* `webhook access key**:** (32 characters: [a-z0-9]{32})`
* `webhook private key**:** (32 characters: [a-z0-9]{32})`

> Note: if you choice `Connect Paykassma Test`, @BotFather will send you the token without requesting any data and you can skip this step

You can find this data on your A-pay Dashboard:

* Open the A-pay Dashboard and choose table [`Projects`](https://pay-crm.com/Table/22/44)
* Copy and paste in Telegram `apikey`**,** `project id`**,** `webhook access key` and `webhook private key` of your project (use right-click and copy-button)

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

* Then click `OPEN PROJECT` and find the row with `Webhook depost URL: https://paykassmapayment-tgb.com/apay/deposit/postback`

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

* Copy and paste in Telegram the `webhook id` of this row (use right-click and copy-button)

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

Now you can send the data to @paykassma\_bot. All the parameters must be included in a single message, each on a new line, in the following order

1. `apikey**:** (32 characters: [a-z0-9]{32})`
2. `project id**:** (up to 7 digits: [0-9]{1,7})`
3. `webhook id**:** (7 digits: [0-9]{7})`
4. `webhook access key**:** (32 characters: [a-z0-9]{32})`
5. `webhook private key**:** (32 characters: [a-z0-9]{32})`

**For example:**

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

If the data is correct, @paykassma\_bot will respond with: _"The project data has been successfully saved"_

{% hint style="warning" %}
**Attention!** Do not give this data to any third parties!
{% endhint %}

## **Step 3**

Obtain the token from @BotFather

Once the data is saved, you can return to @BotFather to obtain the token to implement payments to your merchant bot.

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

## **Step 4**

Pass the obtained token to your dev team, so they can use in implementation of Telegram Payments in your merchant bot. You can find the necessary methods for building your payment implementation in the [Payments Section of the Bot API Manual](https://core.telegram.org/bots/api#payments).
