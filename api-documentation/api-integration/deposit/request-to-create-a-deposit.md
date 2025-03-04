# Request to create a deposit

### Request to create a deposit

This section provides information on the most popular request - deposit creation request.

{% openapi src="../../../.gitbook/assets/pretty_a-pay_api_19.12.2024.json" path="/Remotes/create-deposit" method="post" %}
[pretty_a-pay_api_19.12.2024.json](../../../.gitbook/assets/pretty_a-pay_api_19.12.2024.json)
{% endopenapi %}

In order for the request to be created correctly, it is necessary to comply with the documentation requirements: all required fields must be present in the request, field validation must be observed.&#x20;

In a deposit request, the client must send the following information:

* **project\_id** - project id in the A-pay platform, the parameter can be found in the projects table next to the project name.
* **amount** - deposit amount in integer
* **currency** - currency of payment system
* **payment\_system** - name of payment system from A-pay platform
* **custom\_transaction\_id** - unique transaction identifier in the client's system
* **custom\_user\_id** - unique user identifier in the client's system
* **data** - additional data depending on the PS[^1]. To find out what data should be sent, you should click on data and select the necessary PS. These data will be specified [here](https://api.a-pay.one/#tag/Deposit/paths/~1Remotes~1create-deposit/post)

{% hint style="info" %}
Each payment system requires certain information to be sent. Please read our API carefully!
{% endhint %}

### Responses

Below is information about our server's responses to such requests, as well as the data that our server sends to the client in response. The response can be both successful and unsuccessful.

Our server sends the following data to the client in the request:&#x20;

* **success** - a parameter that is responsible for whether the response is successful or not
* **status** - status of deposit creation
* **order\_id** - our internal unique order, which is assigned to each transaction&#x20;
* **data** - additional data that depends on the PS. To find out what data should be sent, you need to click on data and select the desired PS. This data will be spelled out [here](https://api.a-pay.one/#tag/Deposit/paths/~1Remotes~1create-deposit/post) in the response section. In this case, it is a link to the deposit payment

{% hint style="info" %}
Each payment system returns different information to the client. Please read our API carefully!
{% endhint %}

**Examples of the responses:**

{% code title="Success" %}
```json
{
  "success": true,
  "status": "Pending",
  "order_id": "7fa13dbc3b79e05e",
  "data": {
    "paymentpage_url": "string"
  }
}
```
{% endcode %}

{% code title="Error" %}
```json
{
  "success": false,
  "message": "Invalid request",
  "status": "Failed",
  "code": 400,
  "order_id": "7fa13dbc3b79e05e"
}
```
{% endcode %}

[^1]: Payment System
