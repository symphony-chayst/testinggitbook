---
title: "Create Message"
slug: "create-message-v4"
excerpt: "`Available on Agent 2.53.0 and above.` \nPosts a message to an existing stream."
hidden: false
metadata: 
image: []
createdAt: "Sat May 13 2017 00:49:21 GMT+0000 (Coordinated Universal Time)"
updatedAt: "Wed May 24 2023 12:36:40 GMT+0000 (Coordinated Universal Time)"
---
* You can specify one or more attachments to the message. The attachment file can be uploaded along with the message using this endpoint.
* You can also specify [Structured Objects](https://docs.developers.symphony.com/building-bots-on-symphony/messages/structured-objects) as part of the message, and use [Apache FreeMarker templates](http://freemarker.org/) with these objects. See **Use Apache FreeMarker Templates** section below.
* Interactive Elements can be sent with your messages. See **Sending messages with Symphony Elements** section below.
* For authentication, you must either use the `sessionToken` that was created for delegated application access, or both the `sessionToken` and `keyManagerToken` together.
* When calling this as an [OBO-Enabled Endpoints](ref:obo-enabled-endpoints), use the [OBO User Authenticate](ref:obo-user-authenticate) token for `sessionToken`.

[block:callout]
{
  "type": "warning",
  "title": "Known Limitations",
  "body": "* You can’t send a message that contains only an attachment without any message content. `message` must contain a least one space.\n* DLP (Expression Filters) only works with 1.53 version onwards.\n* Symphony Elements only works with 1.55.3 version onwards.\n* To send messages to not public rooms, the caller needs to be part of the stream.\n* To send numeric cashtags as signals, add a `*` before the number such as `$*122450`. \nE.g. `<messageML><cash tag=\"*122450\"/></messageML>`\nFor more information, refer to [Message Format - MessageML](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-messageml).\n* Attachments: The limit is set to 30Mb total size; also, it is recommended not to exceed 25 files.\n* For more information on the size limits of messages, please refer to [Messages](https://docs.developers.symphony.com/building-bots-on-symphony/messages#message-size-limits) under the section Messages Size Limits."
}
[/block]

[block:api-header]
{
  "title": "Sending messages with Symphony Elements"
}
[/block]
[Symphony Elements](https://docs.developers.symphony.com/building-bots-on-symphony/symphony-elements) is a collection of interactive elements that can be sent within messages to facilitate communication with Symphony users.

Through the use of the elements, bots can send messages that contain forms with text fields, dropdown menus, person selectors, buttons and more! To use the Elements, you just need to call the `/agent/v4/stream/:sid/message/create` API using the [MessageML](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-messageml) format. For more information and examples, refer to [Symphony Elements](https://docs.developers.symphony.com/building-bots-on-symphony/symphony-elements).

*Sending Symphony Elements is only supported for `SYSTEM` users (Service Accounts).* 
[block:api-header]
{
  "title": "Use Apache FreeMarker Templates"
}
[/block]
The `message` and `data` parameters of the [Create Message v4](ref:create-message-v4) endpoint supports using [Apache FreeMarker templates](http://freemarker.org/) with [Structured Objects](https://docs.developers.symphony.com/building-bots-on-symphony/messages/structured-objects). 

**Important!** To support the use of Freemarker variables, the top-level variable name in the `div` or `span` tag in `<messageML>` must be `$entity` or `$data`.

Using the [Freemarker](https://freemarker.apache.org/) template, you can also create tables that contain a special column with `checkboxes` or `buttons` on it, allowing users to select one or more rows of the table. For more information, refer to [Symphony Elements - Table Select](https://docs.developers.symphony.com/building-bots-on-symphony/symphony-elements/available-elements/table-select).

The first and second examples show the FreeMarker template content enclosed in `<messageML>` for the `message` parameter and the associated data model for the `data` parameter. The third example shows the resulting [PresentationML](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-presentationml) representation of the message.

See also **cURL - Message + Data** in the examples below.
[block:code]
{
  "codes": [
    {
      "code": "<messageML>\n\t<div>Messages for user <b>${data[\"messageList\"].user.userName}</b>:</div>\n\t<table>\n\t\t<#list data[\"messageList\"].messages as msg>\n\t\t<tr>\n\t\t\t<td>${msg.timestamp}</td>\n\t\t\t<td>${msg.text}</td>\n\t\t</tr>\n\t\t</#list>\n\t</table>\n</messageML>",
      "language": "xml",
      "name": "messageML"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"messageList\": {\n    \"version\": \"1.0\",\n    \"user\": {\n      \"id\": 123456,\n      \"userName\": \"bot.user01\"\n    },\n    \"messages\": [\n      {\n        \"timestamp\": \"1504736061894\",\n        \"text\": \"Ping\"\n      },\n      {\n        \"timestamp\": \"1504736023993\",\n        \"text\": \"Pong\"\n      }\n    ]\n  }\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "<div data-format=\"PresentationML\" data-version=\"2.0\">\n   <div>Messages for user <b>bot.user01</b>:</div>\n   <table>\n      <tr>\n         <td>1504736061894</td>\n         <td>Ping</td>\n      </tr>\n      <tr>\n         <td>1504736023993</td>\n         <td>Pong</td>\n      </tr>\n   </table>\n</div>",
      "language": "xml"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Other request examples"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n  https://acme.symphony.com/agent/v4/stream/xFfPil_o26qXzGwWvcw5RH___qQr0W7EdA/message/create \\\n  -H 'Content-Type: multipart/form-data' \\\n  -H 'sessionToken: SESSION_TOKEN' \\\n\t-H 'keyManagerToken: KEY_MANAGER_TOKEN' \\\n  --form-string 'message=<messageML>Hello world!</messageML>'",
      "language": "curl",
      "name": "cURL - Message"
    },
    {
      "code": "curl -X POST \\\n  https://acme.symphony.com:443/agent/v4/stream/xFfPil_o26qXzGwWvcw5RH___qQr0W7EdA/message/create \\\n  -H 'content-type: multipart/form-data' \\\n  -H 'keyManagerToken: KEY_MANAGER_TOKEN' \\\n  -H 'sessionToken: SESSION_TOKEN' \\\n  --form-string 'message=<messageML>Hello world!</messageML>' \\\n  -F 'attachment=@image1.png' \\\n  -F 'attachment=@image2.png'",
      "language": "curl",
      "name": "cURL - Attachments"
    },
    {
      "code": "curl -X POST \\\nhttps://acme.symphony.com:443/agent/v4/stream/xFfPil_o26qXzGwWvcw5RH___qQr0W7EdA/message/create \\\n  -H 'Content-Type: multipart/form-data' \\\n  -H 'keyManagerToken: KEY_MANAGER_TOKEN' \\\n  -H 'sessionToken: SESSION_TOKEN' \\\n  --form-string 'message=<messageML>This is an <div class=\"entity\" data-entity-id=\"object001\"><b>important</b></div> message.</messageML>' \\\n  -F 'data={\n    \"object001\":\n    {\n        \"type\":     \"org.symphonyoss.taxonomy\",\n        \"version\":  \"0.1\",\n        \"id\":\n        [\n            {\n                \"type\":     \"org.symphonyoss.taxonomy.keyword\",\n                \"value\":    \"important\"\n            }\n        ]\n    }\n}' ",
      "language": "curl",
      "name": "cURL - Message + Data"
    },
    {
      "code": "curl -X POST \\\n  https://acme.symphony.com:443/agent/v4/stream/xFfPil_o26qXzGwWvcw5RH___qQr0W7EdA/message/create \\\n\n-H \"sessionToken: SESSION_TOKEN\" \\\n-H \"keyManagerToken: KEY_MANAGER_TOKEN\" \\\n-H \"Content-Type: application/x-www-form-urlencoded\" \\\n-d \"message='<messageML>Hello world</messageML>'&data='{'foo':'bar'}'\" ",
      "language": "curl",
      "name": "cURL - Message + Data Form encoded"
    },
    {
      "code": "curl -X POST \\\n  https://acme.symphony.com:443/agent/v4/stream/xFfPil_o26qXzGwWvcw5RH___qQr0W7EdA/message/create \\\n\n-H \"sessionToken: SESSION_TOKEN\" \\\n-H \"keyManagerToken: KEY_MANAGER_TOKEN\" \\\n-H \"Content-Type: application/json\" \\\n-d '{\n        \"message\": \"<messageML>Hello world</messageML>\",\n        \"data\": \"{\\\"foo\\\":\\\"bar\\\"}\"\n    }' ",
      "language": "curl",
      "name": "cURL - Message + Data JSON"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "See also",
  "body": "[Message](https://docs.developers.symphony.com/building-bots-on-symphony/messages)\n[MessageML](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-messageml)\n[Message ID](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-messageml#message-identifiers)\n[Message Format - MessageML](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-messageml)\n[PresentationML](https://docs.developers.symphony.com/building-bots-on-symphony/messages/overview-of-presentationml)\n[Message Format - ExtensionML](https://docs.developers.symphony.com/building-extension-applications-on-symphony/overview-of-extension-api/extension-api-services/entity-service/message-format-extensionml)\n[Colors](https://docs.developers.symphony.com/developer-tools/developer-tools/ui-style-guide/colors)\n[Symphony Elements](https://docs.developers.symphony.com/building-bots-on-symphony/symphony-elements)"
}
[/block]
