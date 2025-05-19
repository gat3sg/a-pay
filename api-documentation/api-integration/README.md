# 🖥️ API Integration

This integration is usually chosen by those clients who want to keep the user from navigating to third-party sites. It has the best conversion rate with excellent customization of the site's UX/UI and deposit payment process, but requires more control over the payment systems.

{% hint style="warning" %}
JSON responses returned by our API may be extended over time. To ensure stable and future-proof integrations, follow the recommendations below:
{% endhint %}

<details>

<summary>Recommendations for Handling JSON Responses (Ensuring Backward Compatibility)</summary>

Client-side implementation guidelines:

1. Ignore unknown fields. When handling API responses, process only the fields your integration explicitly depends on. Any additional or unrecognized fields must be ignored, not treated as errors.
2. Avoid strict field validation. Do not implement logic that relies on the exact number or set of fields returned. Instead, access only the fields by name that are relevant to your business logic.
3. Configure your JSON parsers to allow extra fields.
4. Do not compare full JSON payloads as raw strings. Logic based on raw string comparison is fragile and may break when new fields are added, even if the response remains semantically valid.

</details>

You can learn more about API integration here:

* [Deposit](deposit/)
* [Withdrawal](withdrawal.md)
* [Lost transaction](https://app.gitbook.com/o/LNbZs3dNMDs1xm16Y9hk/s/dRt6CV4HPvnslXUaqL9U/~/changes/116/api-documentation/api-integration/lost-transaction)
