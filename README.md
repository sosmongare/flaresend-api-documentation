# Flaresend WhatsApp Messaging API Documentation

## Overview

**Flaresend API** provides a simple and reliable way to integrate WhatsApp messaging into your applications. From building chatbots, notification systems, to customer support tools, Flaresend makes it easy to send, receive, and manage messages at scale all through secure RESTful endpoints.
Get started by creating an account, setting up a WhatsApp instance, scanning the QR code, and generating your API key.

**Base URL:** `https://api.flaresend.com`

**API Version:** `v1.0.0`

---

## How It Works

### Step 1: Account Creation

1. Visit [https://www.flaresend.com](https://www.flaresend.com)
2. Click **“Get Started”** to log in/sign up
3. Fill in the required signup information.
4. Log in to your dashboard.

---

### Step 2: Creating an Instance

> ⚠️ **Important:** You must have a connected WhatsApp instance before sending any messages or receive any notifications.

1. Navigate to the **“Your WhatsApp Instances”** section in your dashboard.
2. Click **“Create New Instance”** at the top-right corner, a new instance will be generated automatically.
3. Click **Show QR** button to scan the generated QR code.
4. (**If instance(s) is disconnected**) Click the **“Reconnect”** button to link your WhatsApp number.
5. Scan the QR code displayed on the screen following the on-screen instructions.

---

### Step 3: Generate an API Key

1. Go to the **“API Keys”** section in your dashboard.
2. Copy your generated API key linked to the connected whatsapp number, you’ll use it for all authenticated requests to the Flaresend API.

> 🔒 **Security Tip:** Keep your API key secret. It authenticates your application and grants full access to your account.

---

## Authentication

All API requests must be authenticated using your **API Key**.

Include your API key in the `Authorization` header as follows:

```bash
Authorization: Bearer YOUR_API_KEY
```

Example cURL request:

```bash
curl -X GET https://api.flaresend.com/send-message \
     -H "Authorization: Bearer YOUR_API_KEY"
```

---

## Messaging

**Base URL:**
`https://api.flaresend.com`



### Send Message

This endpoint allows you to send WhatsApp messages through your connected instance.
You can send **text**, **documents**, **media** (image, audio, or video), or **template** messages depending on your integration setup.

**Endpoint:**
`POST /send-message`

---

### **Headers**

| Header          | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| `Authorization` | string | Bearer token used for authentication. |
| `Content-Type`  | string | Must be `application/json`.           |

---

### **Request Body**

| Parameter    | Type   | Required             | Description                                                                       |
| ------------ | ------ | -------------------- | --------------------------------------------------------------------------------- |
| `recipients` | array  | ✅                    | List of recipient phone numbers in international format (e.g., `"2547XXXXXXXX"`). |
| `type`       | string | ✅                    | Message type , e.g., `text`, `image`, `video`, `document`, etc.                   |
| `text`       | string | ✅ (if `type` = text) | The message content to send.                                                      |

---

### **Example cURL Request**

```bash
curl --location 'https://api.flaresend.com/send-message' \
--header 'Authorization: Bearer YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data '{
    "recipients": ["254712345678"],
    "type": "text",
    "text": "Hello from the APIs!"
}'
```

---

### **Example Response**

**Success Response (200)**

```json
{
    "success": true,
    "message": "Message sending completed: 2 successful, 0 failed",
    "sent_at": "2023-12-01T12:01:00Z",
    "total_recipients": 2,
    "successful_recipients": ["254712345678@s.whatsapp.net", "254798765432@s.whatsapp.net"],
    "failed_recipients": [],
    "invalid_recipients": []
}
```

**Error Response (401)**

```json
{
    "success": false,
    "error": "Authorization header required"
}
```

**Error Response (425)**

```json
{
    "success": false,
    "message": "Instance not ready for messaging",
    "status": "initializing",
    "action": "Please ensure the QR code is scanned and the instance is connected"
}
```

**Error Response (400)**

```json
{
    "success": false,
    "message": "Recipients array is required"
}
```

---

## Additional Message Types

Below are examples of how to send different message types using the same `/send-message` endpoint.

---

### **Document Message (File Upload)**

```bash
curl --location 'https://api.flaresend.com/send-message' \
--header 'Authorization: Bearer YOUR_API_KEY' \
--form 'recipients[]="254712345678"' \
--form 'recipients[]="254718841608"' \
--form 'type="document"' \
--form 'file=@"/C:/Users/sosmo/Documents/Sec1L1.pdf"'
```

---

### **Document Message (via URL)**

```json
{
    "recipients": ["254712345678"],
    "type": "document",
    "url": "https://example.com/file.pdf",
    "fileName": "Report.pdf"
}
```

---

### **Image Message (via URL)**

```json
{
    "recipients": ["254712345678"],
    "type": "image",
    "url": "https://example.com/image.jpg",
    "caption": "Check this out!"
}
```

---

### **Image Message (File Upload)**

**Form Data:**

* `recipients[]`: ["254712345678"]
* `type`: `image`
* `caption`: `"Check this out!"`
* `file`: *(select image file)*

---

### **Video Message (via URL)**

```json
{
    "recipients": ["254712345678"],
    "type": "video",
    "url": "https://example.com/video.mp4",
    "caption": "Watch this!"
}
```

---

### **Audio Message (via URL)**

```json
{
    "recipients": ["254712345678"],
    "type": "audio",
    "url": "https://example.com/audio.mp3"
}
```
---

Here is a polished, ready-to-paste **Schedule Messages** section matching the formatting and tone of your existing documentation:

---

## Scheduled Messaging

Flaresend allows you to schedule WhatsApp messages to be delivered at a future date and time. This is useful for reminders, notifications, campaigns, and automated workflows.

---

### Schedule a Message

**Endpoint:** `POST /schedule-message`

### **Headers**

| Header          | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| `Authorization` | string | Bearer token used for authentication. |
| `Content-Type`  | string | Must be `application/json`.           |

---

### **Request Body**

| Parameter    | Type   | Required              | Description                                                             |
| ------------ | ------ | --------------------- | ----------------------------------------------------------------------- |
| `recipients` | array  | ✅                     | List of phone numbers in international format (e.g., `"2547XXXXXXXX"`). |
| `type`       | string | ✅                     | Message type — `text`, `image`, `video`, `audio`, `document`, etc.      |
| `text`       | string | Required if type=text | Message content.                                                        |
| `sendAt`     | string | ✅                     | Scheduled time in `YYYY-MM-DD HH:mm:ss` format (24-hour).               |

---

### **Example cURL Request**

```bash
curl --location 'https://api.flaresend.com/schedule-message' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_API_KEY' \
--data '{
    "recipients": ["254708920430"],
    "type": "text",
    "text": "Again!",
    "sendAt": "2025-11-14 13:48:00"
}'
```

---

### **Success Response (200)**

```json
{
  "success": true,
  "message": "Message scheduled successfully",
  "scheduled_for": "2025-11-14 13:48:00",
  "task_id": "SCHED-9128371923"
}
```

---

## View Scheduled Messages

Retrieve all pending or upcoming scheduled messages.

**Endpoint:** `GET /scheduled-messages`

### **Headers**

| Header          | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| `Authorization` | string | Bearer token used for authentication. |

---

### **Example cURL Request**

```bash
curl --location 'https://api.flaresend.com/scheduled-messages' \
--header 'Authorization: Bearer YOUR_API_KEY'
```

---

### **Example Response**

```json
{
  "success": true,
  "message": "Scheduled messages fetched successfully",
  "data": [
    {
      "id": "SCHED-9128371923",
      "recipients": ["254708920430"],
      "type": "text",
      "text": "Again!",
      "sendAt": "2025-11-14 13:48:00",
      "status": "pending"
    }
  ]
}
```

---

## Group Management

Manage WhatsApp groups using the Flaresend API.
You can create groups, list existing groups, view details, generate invite links, and send messages to groups.

**Base URL:**
`https://api.flaresend.com`

---

### 1. Create Group

**Endpoint:**
`POST /groups`

Creates a new WhatsApp group with a specified subject and list of participants.

---

#### **Headers**

| Header          | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| `Authorization` | string | Bearer token used for authentication. |
| `Content-Type`  | string | Must be `application/json`.           |

---

#### **Request Body**

| Parameter      | Type   | Required | Description                                                |
| -------------- | ------ | -------- | ---------------------------------------------------------- |
| `subject`      | string | ✅        | The name of the group.                                     |
| `participants` | array  | ✅        | List of participant phone numbers in international format. |

---

#### **Example Request**

```bash
curl --location 'https://api.flaresend.com/groups' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_API_KEY' \
--data '{ 
    "subject": "Family Group 4", 
    "participants": ["254712345678","254723456789"] 
}'
```

#### **Example Response**

```json
{
    "success": true,
    "message": "Group created successfully",
    "data": {
        "id": "120363420800326612@g.us",
        "addressingMode": "lid",
        "subject": "Family Group 4",
        "subjectOwner": "163514152964174@lid",
        "subjectTime": 1755072787,
        "size": 2,
        "creation": 1755072787,
        "owner": "163514152964174@lid",
        "restrict": false,
        "announce": false,
        "isCommunity": false,
        "isCommunityAnnounce": false,
        "joinApprovalMode": false,
        "memberAddMode": false,
        "participants": [
            {
                "id": "163514152964174@lid",
                "admin": "superadmin"
            },
            {
                "id": "268813446717676@lid",
                "admin": null
            }
        ]
    }
}
```

---

### 2. List All Groups

**Endpoint:**
`GET /groups`

Fetches all existing groups in your connected WhatsApp instance.

---

#### **Headers**

| Header          | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| `Authorization` | string | Bearer token used for authentication. |

---

#### **Optional Query Parameters**

| Parameter | Type    | Description                 |
| --------- | ------- | --------------------------- |
| `page`    | integer | Page number for pagination. |
| `limit`   | integer | Number of results per page. |

---

#### **Example Request**

```bash
curl --location 'https://api.flaresend.com/groups?page=1&limit=10' \
--header 'Authorization: Bearer YOUR_API_KEY'
```

#### **Example Response**

```json
{
    "success": true,
    "message": "Fetched all groups successfully",
    "data": {
        "120363403582097887@g.us": {
            "id": "120363403582097887@g.us",
            "subject": "Again again",
            "size": 2,
            "owner": "268813446717676@lid",
            "participants": [
                {
                    "id": "163514152964174@lid",
                    "admin": null
                },
                {
                    "id": "268813446717676@lid",
                    "admin": "superadmin"
                }
            ]
        },
        "120363420250618357@g.us": {
            "id": "120363420250618357@g.us",
            "subject": "Family Group 4",
            "size": 2,
            "owner": "163514152964174@lid",
            "participants": [
                {
                    "id": "163514152964174@lid",
                    "admin": "superadmin"
                },
                {
                    "id": "268813446717676@lid",
                    "admin": null
                }
            ]
        }
    }
}
```

---

### 3. Get Group Information

**Endpoint:**
`GET /groups/{groupId}`

Retrieves detailed information about a specific group.

---

#### **Headers**

| Header          | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| `Authorization` | string | Bearer token used for authentication. |

---

#### **Example Request**

```bash
curl --location 'https://api.flaresend.com/groups/120363420250618357@g.us' \
--header 'Authorization: Bearer YOUR_API_KEY'
```

#### **Example Response**

```json
{
    "success": true,
    "message": "Group info fetched successfully",
    "data": {
        "id": "120363420250618357@g.us",
        "name": "Weekend Hiking Plans",
        "desc": "Plan for our next hike.",
        "createdAt": "Aug 13, 2025, 11:01 PM",
        "owner": "163514152964174@lid",
        "isCommunity": false,
        "participants": [
            {
                "id": "50479304319053@lid",
                "admin": "admin"
            },
            {
                "id": "163514152964174@lid",
                "admin": null
            },
            {
                "id": "268813446717676@lid",
                "admin": "admin"
            }
        ]
    }
}
```

---

### 4. Get Group Invite Link

**Endpoint:**
`GET /groups/{groupId}/invite-link`

Fetches the group’s WhatsApp invite link.

---

#### **Headers**

| Header          | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| `Authorization` | string | Bearer token used for authentication. |

---

#### **Example Request**

```bash
curl --location 'https://api.flaresend.com/groups/120363403582097887@g.us/invite-link' \
--header 'Authorization: Bearer YOUR_API_KEY'
```

---

### 5. Send Group Message

**Endpoint:**
`POST /groups/send-message`

Sends a message to one or more WhatsApp groups.

---

#### **Headers**

| Header          | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| `Authorization` | string | Bearer token used for authentication. |
| `Content-Type`  | string | Must be `application/json`.           |

---

#### **Request Body**

| Parameter   | Type   | Required             | Description                                |
| ----------- | ------ | -------------------- | ------------------------------------------ |
| `groupJids` | array  | ✅                    | List of WhatsApp group IDs (`@g.us`).      |
| `type`      | string | ✅                    | Message type , e.g., `text`, `image`, etc. |
| `text`      | string | ✅ (if `type` = text) | The message content to send.               |

---

#### **Example Request**

```bash
curl --location 'https://api.flaresend.com/groups/send-message' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_API_KEY' \
--data-raw '{
    "groupJids": ["120363403582097887@g.us"],
    "type": "text",
    "text": "Hello group again!"
}'
```

#### **Example Response**

```json
{
    "success": true,
    "message": "Message sent successfully to 1 group",
    "data": {
        "delivered": ["120363403582097887@g.us"],
        "failed": []
    }
}
```
---
Here’s the refined documentation for your **6th API endpoint** written in the same style and format as your provided template:

---

### 6. Manage Group Participants

**Endpoint:**
`POST /groups/:groupId/participants`

Performs participant management actions in a WhatsApp group, including adding, removing, promoting, or demoting users.

---

#### **Headers**

| Header          | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| `Authorization` | string | Bearer token used for authentication. |
| `Content-Type`  | string | Must be `application/json`.           |

---

#### **Request Body**

| Field          | Type   | Required | Description                                                                  |
| -------------- | ------ | -------- | ---------------------------------------------------------------------------- |
| `action`       | string | ✅ Yes    | The operation to perform. Accepts: `add`, `remove`, `promote`, or `demote`.  |
| `participants` | array  | ✅ Yes    | List of participant WhatsApp IDs (`@s.whatsapp.net`) to apply the action to. |

---

#### **Example Request**

```bash
curl --location 'https://api.flaresend.com/groups/120363422712338174@g.us/participants' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_API_KEY' \
--data-raw '{
    "action": "add",
    "participants": [
        "254712345678",
        "254798765432"
    ]
}'
```

---

#### **Success Responses**

**Add Participants**

```json
{
    "success": true,
    "message": "Participants successfully added",
    "groupId": "120363422712330174@g.us",
    "participants": [
        "254712345678",
        "254798765432"
    ]
}
```

**Remove Participants**

```json
{
    "success": true,
    "message": "Participants successfully removed",
    "groupId": "120363422712330174@g.us",
    "participants": [
        "254798765432"
    ]
}
```

**Promote Participants**

```json
{
    "success": true,
    "message": "Participants successfully promoted",
    "groupId": "120363422712330174@g.us",
    "participants": [
        "254798765432"
    ]
}
```

**Demote Participants**

```json
{
    "success": true,
    "message": "Participants successfully demoted",
    "groupId": "120363422712330174@g.us",
    "participants": [
        "254798765432"
    ]
}
```

---

#### **Error Responses**

**Participants already in group**

```json
{
    "success": false,
    "message": "Some participants are already in the group.",
    "invalid": [
        "254798765432"
    ]
}
```

**Participants not found or invalid**

```json
{
    "success": false,
    "message": "Some participants are not in the group.",
    "invalid": [
        "254700000000"
    ]
}
```

**Invalid action**

```json
{
    "success": false,
    "message": "Invalid action. Allowed: add, remove, promote, demote."
}
```

---

#### **Notes**

* The endpoint uses the group’s unique identifier (e.g., `120363422712339174@g.us`) as a URL parameter.
* Only authorized users with valid Bearer tokens can perform group management actions.
* The API will return clear messages indicating the result of each participant operation.

---

## Webhooks

Flaresend can notify your server in real time when specific events occur, such as receiving new WhatsApp messages. When a webhook is triggered, Flaresend sends a `POST` request with event data to your registered webhook URL.

**Endpoint:** `POST {your_webhook_url}`
**Content-Type:** `application/json`

---

### Setup

1. Go to the **Your WhatsApp Instances** section in your Flaresend dashboard.
2. Click the **Edit Webhook** button.
3. Enter your webhook endpoint URL (must start with `https://`).
4. Save and verify your webhook by sending a message to the connected WhatsApp number or testing via Postman.

> **Note:** During verification, your endpoint may need to respond to a challenge or return a **200 OK** status to confirm successful setup.

---

### Example Event: Message Received

```json
{
  "event": "message_received",
  "instanceId": "9b540e83-e1ca-49b4-8c34-01fd31991d04",
  "data": {
    "from": "254712345678@s.whatsapp.net",
    "message": {
      "conversation": "Hi, how are you",
      "messageContextInfo": {
        "deviceListMetadata": {
          "senderKeyHash": "k0KrwvtTGeJWyg==",
          "senderTimestamp": "1760137298",
          "recipientKeyHash": "fBBUmTapmjlK9w==",
          "recipientTimestamp": "1760704527"
        },
        "deviceListMetadataVersion": 2,
        "messageSecret": "40cV3I0agULcjIrFPsHdaDiA5BuAmFR4CkdpOlUpOHs="
      }
    },
    "timestamp": 1760705280,
    "id": "AC2EB4F50AAAC4C20D27DA8130FC844A"
  },
  "timestamp": "2025-10-17T12:47:59.689Z"
}
```

---

### Event Fields

| Field                             | Type    | Description                                                      |
| --------------------------------- | ------- | ---------------------------------------------------------------- |
| `event`                           | string  | Type of event. Example: `message_received`.                      |
| `instanceId`                      | string  | Unique identifier of the connected WhatsApp instance.            |
| `data.from`                       | string  | WhatsApp ID of the sender (JID format).                          |
| `data.message.conversation`       | string  | The actual text content of the message.                          |
| `data.message.messageContextInfo` | object  | Metadata about the message and device keys (encryption-related). |
| `data.id`                         | string  | Internal message identifier.                                     |
| `data.timestamp`                  | integer | Unix timestamp (seconds) when the message was received.          |
| `timestamp`                       | string  | ISO-formatted timestamp when the webhook was delivered.          |

---

### Responding to Webhooks

Your webhook endpoint should return a **200 OK** response to acknowledge successful receipt. If Flaresend receives anything other than a 200 response repeatedly, delivery may be paused or retried according to the dashboard settings.

**Example acknowledgement response:**

```json
{
  "status": "received"
}
```

---

### Tips & Best Practices

* Use HTTPS endpoints and validate incoming requests (HMAC signature or token) to ensure the payload is from Flaresend.
* Retry logic: design your endpoint to handle duplicate events (idempotency).
* Log webhook payloads and response status codes for debugging.
* Filter events you don't need in the dashboard to reduce traffic.

---

## Check WhatsApp Number

This endpoint allows you to verify whether a given phone number is registered on WhatsApp.
It’s useful before attempting to send messages or add participants to groups.

**Endpoint:** `POST /check-number`
**Base URL:** `https://api.flaresend.com`

---

### **Headers**

| Header          | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| `Content-Type`  | string | Must be `application/json`.           |
| `Authorization` | string | Bearer token used for authentication. |

---

### **Request Body**

| Parameter | Type   | Required | Description                                                               |
| --------- | ------ | -------- | ------------------------------------------------------------------------- |
| `number`  | string | ✅        | The phone number to check, in international format (e.g. `254712345678`). |

---

### **Example cURL Request**

```bash
curl --location 'https://api.flaresend.com/check-number' \
--header 'Authorization: Bearer YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data '{
    "number": "254798765432"
}'
```

---

### **Example Responses**

**✅ Success (200)**

```json
{
    "success": true,
    "number": "254798765432",
    "exists": true
}
```

**❌ Error (400)**

```json
{
    "success": false,
    "message": "Invalid number format or missing field"
}
```

**❌ Error (401)**

```json
{
    "success": false,
    "error": "Authorization header required"
}
```
---
### **Support**

Need assistance or have questions about the API?
Reach out to our support team via:

* 📧 **Email:** [support@flaresend.com](mailto:support@flaresend.com)
* 💬 **WhatsApp:** [Chat with Support](https://wa.me/254708920430?text=Hi%20Flaresend%20team%2C%20I%20need%20help%20with%20the%20API)