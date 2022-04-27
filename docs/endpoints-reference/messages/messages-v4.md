# Get Messages

{% swagger src="../../.gitbook/assets/agent-api-public.yaml" path="/v4/stream/{sid}/message" method="get" %}
[agent-api-public.yaml](../../.gitbook/assets/agent-api-public.yaml)
{% endswagger %}

{% swagger method="get" path="/test" baseUrl="test" summary="Summary" %}
{% swagger-description %}
Hello **This** is _so_me ~~te~~xt

`and`
{% endswagger-description %}

{% swagger-parameter in="body" type="Object" required="true" name="id" %}
array
{% endswagger-parameter %}

{% swagger-parameter in="body" %}

{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}
