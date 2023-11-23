---
title: Create Message
slug: create-message-v4
excerpt: |-
  `Available on Agent 2.53.0 and above.` 
  Posts a message to an existing stream.
hidden: false
metadata: null
image: []
createdAt: Sat May 13 2017 00:49:21 GMT+0000 (Coordinated Universal Time)
updatedAt: Wed May 24 2023 12:36:40 GMT+0000 (Coordinated Universal Time)
---

# Test



* You can specify one or more attachments to the message. The attachment file can be uploaded along with the message using this endpoint.
* You can also specify [Structured Objects](https://docs.developers.symphony.com/building-bots-on-symphony/messages/structured-objects) as part of the message, and use [Apache FreeMarker templates](http://freemarker.org/) with these objects. See **Use Apache FreeMarker Templates** section below.
* Interactive Elements can be sent with your messages. See **Sending messages with Symphony Elements** section below.
* For authentication, you must either use the `sessionToken` that was created for delegated application access, or both the `sessionToken` and `keyManagerToken` together.
* When calling this as an [OBO-Enabled Endpoints](ref:obo-enabled-endpoints), use the [OBO User Authenticate](ref:obo-user-authenticate) token for `sessionToken`.

> 🚧 Known Limitations
>
> * You can’t send a message that contains only an attachment without any message content. `message` must contain a least one space.
> * DLP (Expression Filters) only works with 1.53 version onwards.
> * Symphony Elements only works with 1.55.3 version onwards.
> * To send messages to not public rooms, the caller needs to be part of the stream.
> * To send numeric cashtags as signals, add a `*` before the number such as `$*122450`.\
>   E.g. `<messageML><cash tag="*122450"/></messageML>`\
>   For more information, refer to [Message Format - MessageML](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-messageml).
> * Attachments: The limit is set to 30Mb total size; also, it is recommended not to exceed 25 files.
> * For more information on the size limits of messages, please refer to [Messages](https://docs.developers.symphony.com/building-bots-on-symphony/messages#message-size-limits) under the section Messages Size Limits.

### Sending messages with Symphony Elements

[Symphony Elements](https://docs.developers.symphony.com/building-bots-on-symphony/symphony-elements) is a collection of interactive elements that can be sent within messages to facilitate communication with Symphony users.

Through the use of the elements, bots can send messages that contain forms with text fields, dropdown menus, person selectors, buttons and more! To use the Elements, you just need to call the `/agent/v4/stream/:sid/message/create` API using the [MessageML](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-messageml) format. For more information and examples, refer to [Symphony Elements](https://docs.developers.symphony.com/building-bots-on-symphony/symphony-elements).

_Sending Symphony Elements is only supported for `SYSTEM` users (Service Accounts)._

### Use Apache FreeMarker Templates

The `message` and `data` parameters of the [Create Message v4](ref:create-message-v4) endpoint supports using [Apache FreeMarker templates](http://freemarker.org/) with [Structured Objects](https://docs.developers.symphony.com/building-bots-on-symphony/messages/structured-objects).

**Important!** To support the use of Freemarker variables, the top-level variable name in the `div` or `span` tag in `<messageML>` must be `$entity` or `$data`.

Using the [Freemarker](https://freemarker.apache.org/) template, you can also create tables that contain a special column with `checkboxes` or `buttons` on it, allowing users to select one or more rows of the table. For more information, refer to [Symphony Elements - Table Select](https://docs.developers.symphony.com/building-bots-on-symphony/symphony-elements/available-elements/table-select).

The first and second examples show the FreeMarker template content enclosed in `<messageML>` for the `message` parameter and the associated data model for the `data` parameter. The third example shows the resulting [PresentationML](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-presentationml) representation of the message.

See also **cURL - Message + Data** in the examples below.

```xml
<messageML>
	<div>Messages for user <b>${data["messageList"].user.userName}</b>:</div>
	<table>
		<#list data["messageList"].messages as msg>
		<tr>
			<td>${msg.timestamp}</td>
			<td>${msg.text}</td>
		</tr>
		</#list>
	</table>
</messageML>
```

```json
{
  "messageList": {
    "version": "1.0",
    "user": {
      "id": 123456,
      "userName": "bot.user01"
    },
    "messages": [
      {
        "timestamp": "1504736061894",
        "text": "Ping"
      },
      {
        "timestamp": "1504736023993",
        "text": "Pong"
      }
    ]
  }
}
```

```xml
<div data-format="PresentationML" data-version="2.0">
   <div>Messages for user <b>bot.user01</b>:</div>
   <table>
      <tr>
         <td>1504736061894</td>
         <td>Ping</td>
      </tr>
      <tr>
         <td>1504736023993</td>
         <td>Pong</td>
      </tr>
   </table>
</div>
```

### Other request examples

```curl
curl -X POST \
  https://acme.symphony.com/agent/v4/stream/xFfPil_o26qXzGwWvcw5RH___qQr0W7EdA/message/create \
  -H 'Content-Type: multipart/form-data' \
  -H 'sessionToken: SESSION_TOKEN' \
	-H 'keyManagerToken: KEY_MANAGER_TOKEN' \
  --form-string 'message=<messageML>Hello world!</messageML>'
```

```curl
curl -X POST \
  https://acme.symphony.com:443/agent/v4/stream/xFfPil_o26qXzGwWvcw5RH___qQr0W7EdA/message/create \
  -H 'content-type: multipart/form-data' \
  -H 'keyManagerToken: KEY_MANAGER_TOKEN' \
  -H 'sessionToken: SESSION_TOKEN' \
  --form-string 'message=<messageML>Hello world!</messageML>' \
  -F 'attachment=@image1.png' \
  -F 'attachment=@image2.png'
```

```curl
curl -X POST \
https://acme.symphony.com:443/agent/v4/stream/xFfPil_o26qXzGwWvcw5RH___qQr0W7EdA/message/create \
  -H 'Content-Type: multipart/form-data' \
  -H 'keyManagerToken: KEY_MANAGER_TOKEN' \
  -H 'sessionToken: SESSION_TOKEN' \
  --form-string 'message=<messageML>This is an <div class="entity" data-entity-id="object001"><b>important</b></div> message.</messageML>' \
  -F 'data={
    "object001":
    {
        "type":     "org.symphonyoss.taxonomy",
        "version":  "0.1",
        "id":
        [
            {
                "type":     "org.symphonyoss.taxonomy.keyword",
                "value":    "important"
            }
        ]
    }
}'
```

```curl
curl -X POST \
  https://acme.symphony.com:443/agent/v4/stream/xFfPil_o26qXzGwWvcw5RH___qQr0W7EdA/message/create \

-H "sessionToken: SESSION_TOKEN" \
-H "keyManagerToken: KEY_MANAGER_TOKEN" \
-H "Content-Type: application/x-www-form-urlencoded" \
-d "message='<messageML>Hello world</messageML>'&data='{'foo':'bar'}'"
```

```curl
curl -X POST \
  https://acme.symphony.com:443/agent/v4/stream/xFfPil_o26qXzGwWvcw5RH___qQr0W7EdA/message/create \

-H "sessionToken: SESSION_TOKEN" \
-H "keyManagerToken: KEY_MANAGER_TOKEN" \
-H "Content-Type: application/json" \
-d '{
        "message": "<messageML>Hello world</messageML>",
        "data": "{\"foo\":\"bar\"}"
    }'
```

> 📘 See also
>
> [Message](https://docs.developers.symphony.com/building-bots-on-symphony/messages)\
> [MessageML](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-messageml)\
> [Message ID](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-messageml#message-identifiers)\
> [Message Format - MessageML](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-messageml)\
> [PresentationML](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-presentationml)\
> [Message Format - ExtensionML](https://docs.developers.symphony.com/building-extension-applications-on-symphony/overview-of-extension-api/extension-api-services/entity-service/message-format-extensionml)\
> [Colors](https://docs.developers.symphony.com/developer-tools/developer-tools/ui-style-guide/colors)
