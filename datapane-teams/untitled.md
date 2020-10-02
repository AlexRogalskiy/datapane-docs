# Authentication and Sharing

_Datapane for Teams_ allows secure, authenticated sharing of reports, scripts, blobs, and secrets. Each user on your Datapane instance \(e.g. https://acme.datapane.net\) has a private password-protected account.

{% hint style="info" %}
Each Datapane instance exists as a separate database tenancy, so accounts are not shared between instances. You cannot use the same account to authenticate across multiple instances.
{% endhint %}

All items which can be created on your Datapane instance have a `visibility` setting which dictates the users who can access them. The visibility options are `PUBLIC`, `PRIVATE`, and `ORG` , and are typically configured on object creation. The default permission is `ORG`.

For instance, with a report:

```python
# Publicly available to anyone with the link
report.publish(visibility='PUBLIC')

# Available to every registed user on your Datapane instance
report.publish(visibility='ORG')

# Available to only you, and not other users in the tenancy
report.publish(visibility='PRIVATE')
```

For reports and scripts, you can also configure visibility settings through the web interface.

![](../.gitbook/assets/image%20%2898%29.png)

### Access Tokens

If you want to share a private report with an outside party - such as a client or contractor - you can use the link provided next to the **Share** button to generate a secure signed token. This link contains this token which allows anyone with the link to access the report, without signing up to Datapane.

![](../.gitbook/assets/image%20%2897%29.png)

This token also works across embeds, so you can [embed](../tutorials/embedding-reports-in-social-platforms.md#business-tooling) a private report into platforms such as Confluence or your own webpage. For security reasons, access tokens are revoked after 24 hours, so this is not a suitable method for long-term sharing.

