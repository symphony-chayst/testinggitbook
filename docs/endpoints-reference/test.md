# Test

{% swagger expanded="true" fullWidth="true" method="get" path="/hello" baseUrl="" summary="test" %}
{% swagger-description %}
Hello **bold** _italic_ ~~strikethrough~~

:thumbsup::construction::warning::information\_source:

`this is a code` block <mark style="color:blue;background-color:red;">colors blue on red</mark>

• difficult to insert bullet points...

![](../../.gitbook/assets/violationRule.PNG)

$$f(x) = x * e^{2 pi i \xi x}$$
{% endswagger-description %}

{% swagger-parameter in="path" name="pathParamName" type="integer" required="true" %}
pathParamDescription
{% endswagger-parameter %}

{% swagger-parameter in="body" name="bodyParamName" type="string" %}

{% endswagger-parameter %}

{% swagger-parameter in="header" name="headerParamName" type="key" %}
lol
{% endswagger-parameter %}

{% swagger-parameter in="query" name="queryParamName" %}

{% endswagger-parameter %}

{% swagger-response status="200: OK" description="responseDescription" %}
```javascript
{
    "message": "Hello world!",
    "type": {
        right: "hello",
        left: 26
    }
}
```
{% endswagger-response %}

{% swagger-response status="201: Created" description="responseDescription" %}
```javascript
//Text
```
{% endswagger-response %}

{% swagger-response status="203: Non-Authoritative Information" description="responseDescription" %}
```javascript
//Text
```
{% endswagger-response %}

{% swagger-response status="204: No Content" description="responseDescription" %}
```javascript
//Text
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="responseDescription" %}
```javascript
//Text
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="responseDescription" %}
```javascript
//Text
```
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="responseDescription" %}
```javascript
//Text
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="responseDescription" %}
```javascript
//Text
```
{% endswagger-response %}

{% swagger-response status="500: Internal Server Error" description="responseDescription" %}
```javascript
//Text
```
{% endswagger-response %}

{% endswagger %}