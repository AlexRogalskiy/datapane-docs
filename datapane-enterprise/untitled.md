# Authentication and Sharing

Datapane allows secure, authenticated sharing of reports, scripts, blobs, and secrets. Each user on your Datapane instance \(e.g. https://acme.datapane.net\) has a private password-protected account.

For information on installing the Datapane CLI and authenticating with your Datapane server, please see the relevant section of the [Getting Started](../tut-getting-started.md#datapane-enterprise) guide. 

{% hint style="info" %}
Each Datapane instance exists as a separate database tenancy, so accounts are not shared between instances. You cannot use the same account to authenticate across multiple instances \(including Datapane Community\).
{% endhint %}

## Access Tokens

If you want to share a private report with an outside party - such as a client or contractor - you can use the link provided next to the **Share** button to generate a secure signed token. This link contains this token which allows anyone with the link to access the report, without signing up to Datapane.

![](../.gitbook/assets/image%20%28119%29.png)

This token also works across embeds, so you can [embed](../reports/embedding-reports-in-social-platforms/#business-tooling) a private report into platforms such as Confluence or your own webpage. 

{% hint style="info" %}
For security reasons, access tokens are revoked after 48 hours, but this can be configured by your instance admin.
{% endhint %}

## Groups

Your administrator can create user groups on your Datapane Server. This allows reports and apps to be restricted to specific groups of users -- for instance an external client, or a specific internal team. When a new user is added to your Datapane server, the administrator can add them to a specific user group. 

{% hint style="warning" %}
Datapane Servers come with a default user group. All reports and scripts are shared with the `default` group automatically. If you do not want specific internal or external users to view these reports, **do not add them to the `default` group**.
{% endhint %}

You can also change the group of a report via the web UI \(as per the image above\).

