# Key Manager Authenticate

{% swagger src="../../.gitbook/assets/login-api-public.yaml" path="/relay/pubkey/authenticate" method="post" expanded="true" fullWidth="true" %}
[login-api-public.yaml](../../.gitbook/assets/login-api-public.yaml)
{% endswagger %}



{% swagger method="post" path="" baseUrl="https://YOUR-POD-SUBDOMAIN.symphony.com /relay/pubkey/authenticate" summary="" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="token" %}
Default
{% endswagger-parameter %}
{% endswagger %}

To know more about RSA authentication process, refer to [RSA Bot Authentication Workflow](https://docs.developers.symphony.com/building-bots-on-symphony/authentication/rsa-authentication).

For more information on creating and using an RSA key manager token, refer to [RSA Bot Authentication Workflow](https://docs.developers.symphony.com/building-bots-on-symphony/authentication/rsa-authentication).

> ❗️ Key Manager Token Management
>
> The token you receive is valid for the lifetime of a session that is defined by your pod's administration team. This ranges from 1 hour to 2 weeks.
>
> You should keep using the same token until you receive a HTTP 401, at which you should re-authenticate and get a new token for a new session.
>
> [Datafeeds](ref:read-messagesevents-stream-v4) survive session expiration, you do not need to re-create your datafeed if your session expires.

> 🚧 Important
>
> * The following restrictions apply:
>   * The JWT must have an expiration date between the current time and five minutes from the current time.
>   * The JWT must be signed by a private key matching the public key saved for its subject ("sub").
> * For more information on creating and using an RSA session token, refer to [RSA Bot Authentication Workflow](https://docs.developers.symphony.com/building-bots-on-symphony/authentication/rsa-authentication).
