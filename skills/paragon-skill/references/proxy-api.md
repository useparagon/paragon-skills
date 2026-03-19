# Proxy API

## Overview
The Proxy API is used to send API requests directly to the integration provider on behalf of end-users. Auth is still fully managed by Paragon. 

If ActionKit does not have a pre-built action for an integration, use the Proxy API to directly hit the integration's API.

<example>
ActionKit may not have an action for `ZOOM_GET_TRANSCRIPT`, but the user can use the Proxy API with Zoom's API endpoint directly.
</example>

## How to use Proxy API

### 1. Client-side with the Paragon SDK
Use `paragon.request` with the following parameters:
* integrationType: The short name for the integration. i.e. “salesforce” or “googleCalendar”. You can find this string on the Overview tab of the integration you want to access, on your Paragon dashboard.
     * Note: When using a custom integration, the integrationType name is prefixed with "custom." For example, a custom integration titled “TaskLab” would be called "custom.tasklab".
* path: The path (without the hostname) of the API request you are trying to access. An example might be "v1/charges” for Stripe’s charge API or “chat.postMessage" for Slack’s Web API.
* requestOptions: Request options to include, such as:
    * body: An object representing JSON contents of the request.
    * method: An HTTP verb such as “GET” or “POST”. Defaults to GET.

The function returns a Promise for the request output, which will have a shape that varies depending on the integration and API endpoint.

```typescript
await paragon.request('slack', '/chat.postMessage', {
	method: 'POST',
	body: {
		channel: 'CXXXXXXX0' // Channel ID,
		text: 'This message was sent with Paragon Connect :exploding_head:'
	}
});

// -> Responds with { ok: true }, and sends a Slack message :)
```

### 2. Server Side with the Proxy API's REST API
Send a request with the following path parameters:
* Paragon Project ID
* Integration name (i.e. "salesforce" or "slack")
    * For custom integrations prefix the integration name with "custom." (i.e. "custom.spotify")
* API path (i.e. "v1/charges” for Stripe’s charge API or “chat.postMessage")

```bash
curl --request POST \
  --url https://proxy.useparagon.com/projects/PARAGON_PROJECT_ID/sdk/proxy/INTEGRATION_NAME/API_PATH
  --header 'Authorization: Bearer PARAGON_JWT' \
  --header 'Content-Type: application/json'
  --data '
{ 
    "channel": "CXXXXXXX0", 
    "text": "This message was sent with Paragon Connect :exploding_head:" 
}
'
```

## Advanced Usage
* Pass `X-Paragon-Use-Raw-Response: 1` in the header if the user would like the raw response for use cases like getting binary data. For example: 

```bash
curl --request GET \
  -- url https://proxy.useparagon.com/projects/19d...012/sdk/proxy/googledrive/files/<File ID>/?alt=media \
  --header 'Authorization: Bearer PARAGON_JWT' \
  --header 'X-Paragon-Use-Raw-Response: 1'
```

* In some cases, the Base URL included automatically in the Proxy API isn’t the one you want to send requests to. To change the Base URL of a Proxy request, simply use a fully-qualified URL rather than a relative path:

```
https://proxy.useparagon.com/projects/[Project ID]/sdk/proxy/googledrive/https://sheets.googleapis.com/v4/spreadsheets
```

* Most users will be using Paragon's Cloud offering. The base URL for cloud is: "https://proxy.useparagon.com"
     * For On-premise environments use: "https://worker-proxy.<your on-prem host name>"
