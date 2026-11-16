# ⚡ Messenger Kit



<p align="center">

  <strong>A TypeScript Messaging Backend & Client SDK</strong>

</p>



<p align="center">

  <img src="https://img.shields.io/badge/TypeScript-Ready-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript" />

  <img src="https://img.shields.io/badge/Express-5-000000?style=for-the-badge&logo=express&logoColor=white" alt="Express 5" />

  <img src="https://img.shields.io/badge/MariaDB-Relational%20Storage-003545?style=for-the-badge&logo=mariadb&logoColor=white" alt="MariaDB" />

  <img src="https://img.shields.io/badge/MongoDB-Message%20Storage-47A248?style=for-the-badge&logo=mongodb&logoColor=white" alt="MongoDB" />

  <img src="https://img.shields.io/badge/npm-1.1.0-CB3837?style=for-the-badge&logo=npm&logoColor=white" alt="npm" />

</p>



<p align="center">

  <em>Build messaging applications on top of a structured backend foundation.</em>

</p>



---



## 🚀 Overview



**ALOKI Messenger Kit** is a TypeScript-based messaging backend and client SDK designed to provide developers with a structured foundation for building messaging applications.



It combines an Express 5 backend, MariaDB for relational data, MongoDB for message storage, and a dedicated TypeScript client SDK.



Rather than starting from scratch, developers can build on top of an existing architecture that covers authentication, account management, private conversations, message operations, and client-side API communication.



### ✨ What makes it different?



- **Structured backend:** Organized around routes, controllers, services, and repositories.

- **Dual-database architecture:** Separates relational application data from message documents.

- **Authentication foundation:** Includes registration, login, verification, session handling, and account recovery flows.

- **Messaging operations:** Supports private conversations, message retrieval, replies, editing, and deletion.

- **Client SDK:** Provides a dedicated interface for communicating with the API.

- **TypeScript-first:** Includes typed client configuration, request options, and error handling.

- **Package-ready:** Distributed through npm with compiled backend and client output.



---



## 📦 Installation



Install the package through npm:



```bash

npm install messenger-kit

```



### Requirements



| Requirement | Purpose |

|---|---|

| Node.js | Runtime environment |

| MariaDB | Relational database |

| MongoDB | Message document storage |

| Environment configuration | Database, authentication, and application settings |

| Email configuration | Verification and account recovery emails |



*> Installing the package does not automatically provision databases or configure the backend environment. Those services must be configured before starting the server.*



---



## 🧩 Package Components



ALOKI Messenger Kit provides two main components:



| Component | Description |

|---|---|

| Backend | Express-based messaging API |

| Client SDK | TypeScript interface for communicating with the API |



```text

                 ALOKI MESSENGER KIT

                         │

             ┌───────────┴───────────┐

             │                       │

        Backend API              Client SDK

             │                       │

       Express 5 API           createMessengerClient()

             │                       │

       ┌─────┴─────┐             ┌─────┴─────┐

       │           │             │           │

    MariaDB     MongoDB        Auth API    Chat API

```



---



## 🏗️ Architecture



The backend follows a layered architecture that separates HTTP handling, application logic, and data access.



```text

Client / Application

        │

        ▼

     API Routes

        │

        ▼

    Controllers

        │

        ▼

     Services

        │

        ▼

   Repositories

        │

   ┌────┴─────┐

   ▼          ▼

MariaDB    MongoDB

```



### Layer responsibilities



| Layer | Responsibility |

|---|---|

| Routes | Defines API endpoints and middleware |

| Controllers | Handles HTTP requests and responses |

| Services | Contains application rules and business logic |

| Repositories | Handles database operations |

| Models | Defines data structures and database models |

| Middleware | Handles authentication, validation, and request processing |



This separation keeps the application easier to maintain, extend, and test.



---



## 🗄️ Dual Database Architecture



ALOKI uses MariaDB and MongoDB for different data responsibilities.



Instead of forcing all application data into one database, each database handles the type of data it is suited for.



### MariaDB — Relational Data



MariaDB manages structured application data, including:



- User accounts

- Authentication sessions

- Verification codes

- Conversations

- Conversation memberships

- Relational constraints and relationships



### MongoDB — Message Documents



MongoDB handles message storage and message-related document operations.



Message documents include information such as:



- Message identifier

- Conversation identifier

- Sender identifier

- Message body

- Creation timestamp

- Reply reference



### Why two databases?



| MariaDB | MongoDB |

|---|---|

| Relational records | Message documents |

| Structured relationships | Flexible document structure |

| Account and session management | Message storage and retrieval |

| Conversation membership | Message lifecycle operations |



This separation keeps relational data management independent from message document storage.



---



## 🔐 Authentication & Account Security



The authentication system provides a collection of account-management operations.



### Supported capabilities



- User registration

- Login

- Login verification

- Registration verification

- Profile retrieval

- Two-factor authentication settings

- Password updates

- Password recovery

- Email address updates

- Profile updates

- Account deletion



### Session management



The backend uses session records with hashed tokens and expiration information.



Session types include:



- **Temporary sessions**

- **Permanent sessions**



Session-based access control allows protected operations to distinguish between different session types.



---



## 💬 Messaging System



The messaging API supports private conversations and message lifecycle operations.



### Conversation capabilities



- Create a private conversation.

- Retrieve the user's conversations.

- Prevent duplicate private conversations through conversation-pair handling.



### Message capabilities



- Send messages.

- Retrieve conversation messages.

- Retrieve messages with pagination options.

- Reply to existing messages.

- Edit messages.

- Delete messages.



### Message constraints



| Setting | Value |

|---|---:|

| Maximum message body length | 4,000 characters |

| Default message retrieval limit | 50 |

| Maximum retrieval limit | 100 |

| Message storage | MongoDB |



The message retrieval API supports `limit` and `before` query parameters.



---



# 🧰 Client SDK



The Client SDK provides a convenient interface for accessing the backend without manually constructing every HTTP request.



It exposes two main API groups:



- `auth` — Authentication and account management.

- `chat` — Conversations and messaging operations.



## 1. Initialize the Client



Import `createMessengerClient` from the package:



```ts

import { createMessengerClient } from "messenger-kit/client";



const messenger = createMessengerClient({

  baseURL: "http://localhost:4000"

});

```



The returned client contains:



```ts

messenger.auth;

messenger.chat;

```



The `baseURL` property defines the backend API address.



The client automatically removes trailing slashes from the configured base URL.



## 2. Authentication API



Access authentication methods through:



```ts

messenger.auth

```



### Available methods



| Method | HTTP | Endpoint | Description |

|---|---|---|---|

| `register(data)` | POST | `/register` | Submits a registration request. |

| `login(data)` | POST | `/login` | Submits user login credentials. |

| `verifyLogin(data)` | POST | `/verifyCode/login` | Verifies a login code. |

| `verifyRegister(data)` | POST | `/verifyCode/register` | Verifies a registration code. |

| `getProfile()` | GET | `/profile` | Retrieves the authenticated user's profile. |

| `updateTwoFactor(data)` | PATCH | `/two-factor` | Updates two-factor authentication settings. |

| `updatePassword(data)` | POST | `/updatepassword` | Submits a password update request. |

| `forgotPassword(data)` | POST | `/forgotPassword/sendemail` | Starts the password recovery email flow. |

| `sendRecoveryCode(data)` | POST | `/forgotPassword/sendcode` | Requests a password recovery code. |

| `verifyRecoveryCode(data)` | POST | `/verifyCode/forgotpassword` | Verifies a password recovery code. |

| `resetPassword(data)` | POST | `/forgotPassword/sendnewpassword` | Submits a new password after recovery verification. |

| `updateEmail(data)` | POST | `/update_email` | Starts an email address update request. |

| `verifyUpdateEmail(data)` | POST | `/verifyCode/update_email` | Verifies the email update code. |

| `updateProfile(data)` | PATCH | `/updateProfile` | Updates profile information using FormData. |

| `deleteAccount()` | DELETE | `/delete_account` | Requests account deletion. |



### Authentication examples



**Register**



```ts

await messenger.auth.register({

// Registration payload

});

```



**Login**



```ts

await messenger.auth.login({

// Login payload

});

```



**Verify login**



```ts

await messenger.auth.verifyLogin({

// Verification payload

});

```



**Get profile**



```ts

const profile = await messenger.auth.getProfile();

```



**Update password**



```ts

await messenger.auth.updatePassword({

// Password update payload

});

```



**Update profile**



```ts

const formData = new FormData();



formData.append("name", "New Name");



await messenger.auth.updateProfile(formData);

```



**Delete account**



```ts

await messenger.auth.deleteAccount();

```



*> Authentication methods accept the payload expected by their corresponding backend endpoints. The SDK forwards the supplied data; it does not define a separate validation schema for these payloads.*



---



## 3. Chat API



Access messaging methods through:



```ts

messenger.chat

```



### Available methods



| Method | HTTP | Endpoint | Description |

|---|---|---|---|

| `createConversation(data)` | POST | `/conversations` | Creates a private conversation with another user. |

| `getConversations()` | GET | `/conversations` | Retrieves the user's conversations. |

| `sendMessage(id, data)` | POST | `/conversations/:id/messages` | Sends a message to a conversation. |

| `getMessages(id, options?)` | GET | `/conversations/:id/messages` | Retrieves conversation messages with optional pagination. |

| `editMessage(id, messageId, data)` | PATCH | `/conversations/:id/messages/:messageId` | Updates an existing message. |

| `deleteMessage(id, messageId)` | DELETE | `/conversations/:id/messages/:messageId` | Deletes a message. |



### Create a conversation



```ts

await messenger.chat.createConversation({

  targetUserId: 12

});

```



**Input type:**



```ts

interface CreateConversationData {

  targetUserId: number;

}

```



### Retrieve conversations



```ts

const conversations = await messenger.chat.getConversations();

```



Retrieves the conversations associated with the authenticated user.



### Send a message



```ts

await messenger.chat.sendMessage(1, {

  body: "Hello from ALOKI!"

});

```



**Input type:**



```ts

interface SendMessageData {

  body: string;

  replyTo?: string | null;

}

```



To reply to another message:



```ts

await messenger.chat.sendMessage(1, {

  body: "This is a reply.",

  replyTo: "MESSAGE_ID"

});

```



### Retrieve messages



```ts

const messages = await messenger.chat.getMessages(1);

```



The method accepts optional pagination settings:



```ts

interface GetMessagesOptions {

  limit?: number;

  before?: string;

}

```



Example:



```ts

const messages = await messenger.chat.getMessages(1, {

  limit: 30,

  before: "MESSAGE_ID"

});

```



| Option | Description |

|---|---|

| `limit` | Maximum number of messages to retrieve. |

| `before` | Message cursor used for retrieving earlier messages. |



### Edit a message



```ts

await messenger.chat.editMessage(

  1,

  "MESSAGE_ID",

  {

    body: "Updated message content."

  }

);

```



**Input type:**



```ts

interface EditMessageData {

  body: string;

}

```



### Delete a message



```ts

await messenger.chat.deleteMessage(

  1,

  "MESSAGE_ID"

);

```



---



## 4. Client Request Handling



The SDK includes a shared request layer used by both authentication and chat APIs.



### Request features



| Feature | Description |

|---|---|

| Base URL normalization | Removes trailing slashes from the configured URL. |

| JSON requests | Automatically serializes regular request bodies as JSON. |

| FormData support | Sends FormData without manually setting the multipart Content-Type header. |

| Custom headers | Allows additional request headers. |

| Cookie credentials | Uses `credentials: "include"` for requests. |

| JSON responses | Parses JSON response bodies automatically. |

| Text responses | Supports non-JSON response bodies. |

| HTTP error handling | Throws a structured `MessengerApiError` for unsuccessful responses. |

| No-content responses | Handles HTTP 204 responses. |



### Custom request options



```ts

interface RequestConfig {

  method?: "GET" | "POST" | "PATCH" | "DELETE";

  body?: unknown;

  headers?: Record<string, string>;

}

```



The underlying client also exposes a generic `request<T>()` method.



```ts

const result = await messenger.chat.getConversations();

```



The public SDK exposes its composed `auth` and `chat` APIs, while the shared request layer handles the actual HTTP communication.



---



## 5. Error Handling



The SDK exports a custom error class:



```ts

import {

  MessengerApiError

} from "messenger-kit/client";

```



### MessengerApiError



```ts

class MessengerApiError extends Error {

  status: number;

  data: ApiErrorResponse | null;

}

```



It provides:



| Property | Description |

|---|---|

| `message` | Error message. |

| `status` | HTTP response status code. |

| `data` | Parsed error response data, when available. |



### Example



```ts

import {

  createMessengerClient,

  MessengerApiError

} from "messenger-kit/client";



const messenger = createMessengerClient({

  baseURL: "http://localhost:4000"

});



try {

const profile = await messenger.auth.getProfile();



  console.log(profile);

} catch (error) {

  if (error instanceof MessengerApiError) {

    console.error("API Error:", error.message);

    console.error("Status:", error.status);

    console.error("Response:", error.data);

  } else {

    console.error("Unexpected error:", error);

  }

}

```



### Exported TypeScript types



The SDK exports the following types:



| Type | Purpose |

|---|---|

| `MessengerClientConfig` | Client configuration, including `baseURL`. |

| `RequestConfig` | HTTP request configuration. |

| `ApiErrorResponse` | API error response structure. |

| `CreateConversationData` | Conversation creation payload. |

| `SendMessageData` | Message sending payload. |

| `EditMessageData` | Message editing payload. |

| `GetMessagesOptions` | Message retrieval and pagination options. |



---



## 🌐 Backend API Reference



The following endpoints are exposed through the Client SDK.



### Authentication & Account



| Method | Endpoint | Operation |

|---|---|---|

| POST | `/register` | Registration |

| POST | `/login` | Login |

| POST | `/verifyCode/login` | Login verification |

| POST | `/verifyCode/register` | Registration verification |

| GET | `/profile` | Retrieve profile |

| PATCH | `/two-factor` | Update two-factor settings |

| POST | `/updatepassword` | Update password |

| POST | `/forgotPassword/sendemail` | Send recovery email |

| POST | `/forgotPassword/sendcode` | Send recovery code |

| POST | `/verifyCode/forgotpassword` | Verify recovery code |

| POST | `/forgotPassword/sendnewpassword` | Reset password |

| POST | `/update_email` | Update email |

| POST | `/verifyCode/update_email` | Verify email update |

| PATCH | `/updateProfile` | Update profile |

| DELETE | `/delete_account` | Delete account |



### Conversations & Messages



| Method | Endpoint | Operation |

|---|---|---|

| POST | `/conversations` | Create conversation |

| GET | `/conversations` | Retrieve conversations |

| POST | `/conversations/:conversationId/messages` | Send message |

| GET | `/conversations/:conversationId/messages` | Retrieve messages |

| PATCH | `/conversations/:conversationId/messages/:messageId` | Edit message |

| DELETE | `/conversations/:conversationId/messages/:messageId` | Delete message |



*> This reference documents the endpoints exposed by the current Client SDK. It is not intended to imply that every internal backend route is part of the public SDK.*



---



## 🖼️ Profile & File Uploads



The backend includes profile update functionality that accepts multipart form data.



The Client SDK supports this through:



```ts

messenger.auth.updateProfile(formData);

```



The shared request layer detects `FormData` and avoids setting `Content-Type: application/json`, allowing the browser to provide the appropriate multipart boundary.



The backend also serves uploaded files through its configured uploads directory.



---



## ⚙️ Server Lifecycle



The backend startup process follows an ordered initialization sequence:



```text

          Server Startup

                │

                ▼

      Verify Project Identity

                │

                ▼

        Connect to MongoDB

                │

                ▼

       Ensure Message Indexes

                │

                ▼

       Connect to MariaDB

                │

                ▼

         Start HTTP Server

                │

                ▼

          Server Running

```



### Startup behavior



- Verifies the project's signed identity.

- Connects to MongoDB.

- Ensures required message indexes.

- Connects to MariaDB.

- Starts the HTTP server.



### Graceful shutdown



The backend handles `SIGINT` and `SIGTERM` signals and performs an orderly shutdown.



Shutdown operations include:



- Closing the HTTP server.

- Closing database connections.

- Cleaning up application resources.

- Applying a shutdown timeout to avoid hanging indefinitely.



---



## 🛡️ Project Identity Verification



ALOKI Messenger Kit includes a digitally signed project identity.



The identity verification system uses:



- `project-signature.json`

- `project-public.pem`



The backend verifies the project signature using the public key before continuing with startup.



This provides a verification mechanism for the packaged project's identity.



---



## 🛠️ Build & Development



For local development or working with the source code, clone the repository and install its dependencies.



```bash

git clone https://github.com/mehrshd/messenger-kit.git

cd messenger-kit

npm install

```



### Available scripts



| Command | Description |

|---|---|

| `npm run dev` | Starts the development server using Nodemon and tsx. |

| `npm run build` | Builds the backend and client SDK. |

| `npm run build:server` | Compiles the backend and copies required identity assets. |

| `npm run build:client` | Bundles the client SDK and generates TypeScript declarations. |

| `npm test` | Runs the Vitest test suite. |



### Build the package



```bash

npm run build

```



The project uses separate build pipelines:



- **TypeScript compiler (`tsc`):** Compiles the backend.

- **tsup:** Bundles the Client SDK and generates declaration files.

- **tsx:** Runs the backend directly during development.



---



## 🚀 Command Line Interface



ALOKI Messenger Kit includes a CLI for starting the backend and managing database migrations.



### Available Commands



| Command | Description |
|---|---|
| `npx messenger-kit` | Starts the packaged backend server. |
| `npx messenger-kit migrate` | Runs all pending MariaDB and MongoDB migrations. |
| `npx messenger-kit migrate:status` | Displays the status of available migrations in both databases. |



### Start the Server



```bash
npx messenger-kit
```



This command starts the packaged backend server. Before running it, configure the required environment variables and make sure the required database services are available.



## 🗄️ Database Migrations



Messenger Kit includes a migration system for managing database changes across MariaDB and MongoDB.



### Run Migrations



After configuring your `.env` file and making sure both database services are available, run:



```bash
npx messenger-kit migrate
```



The migration runner checks both databases and executes migrations that have not been applied yet. Applied migrations are recorded separately in each database.



### Check Migration Status



To view which migrations have been applied and which are still pending, run:



```bash
npx messenger-kit migrate:status
```



### Migration Locations



| Directory | Purpose |
|---|---|
| `migrations/mariadb/` | MariaDB SQL migrations. |
| `migrations/mongodb/` | MongoDB JavaScript migrations. |



### Important Notes



- Run migrations from only one deployment process at a time.
- Do not modify migration files after they have been applied.
- Running the migration command again skips migrations already recorded as applied.
- Back up your databases before applying migrations in a production environment.



---



## 📁 Project Structure



```text

messenger-kit/

│

├── bin/

│   └── messenger-kit.js

│

├── dist/

│   ├── client/

│   │   ├── index.js

│   │   └── index.d.ts

│   │

│   └── server/

│       ├── server.js

│       └── src/

│

├── migrations/
│   ├── mariadb/
│   │   └── 001_conversations.sql
│   └── mongodb/
│       └── 001_messages.js

│

├── scripts/

│   └── copy-assets.mjs

│

├── src/

│   ├── client/

│   │   ├── auth.ts

│   │   ├── chat.ts

│   │   ├── client.ts

│   │   └── index.ts

│   │

│   ├── config/

│   ├── controllers/

│   ├── middleware/

│   ├── models/

│   ├── repositories/

│   ├── routes/

│   ├── services/

│   └── utils/

│

├── server.ts

├── package.json

├── tsconfig.json

├── tsconfig.build.json

├── tsup.config.ts

└── README.md

```



---



## 📦 Package Distribution



ALOKI Messenger Kit is distributed through the npm registry.



The published package includes:



- Compiled backend application.

- Client SDK.

- TypeScript declaration files.

- CLI entry point.

- Database migration files.

- Public project identity verification assets.

- Package documentation.



### Package exports



| Import | Purpose |

|---|---|

| `messenger-kit/client` | Client SDK and exported types |



### CLI command



```bash

npx messenger-kit

```



---



## 🧠 Engineering Decisions



ALOKI Messenger Kit is built around a few core architectural decisions.



**Separation of responsibilities**



The layered backend architecture keeps HTTP handling, business logic, and data access independent.



**Purpose-driven storage**



MariaDB and MongoDB are used for separate data responsibilities rather than duplicating the same storage model across both databases.



**Reusable client communication**



The Client SDK centralizes request handling, credentials, response parsing, and API error handling.



**TypeScript-first development**



The client package includes TypeScript declarations to improve editor support and integration with TypeScript applications.



**Controlled scope**



The package focuses on its current authentication, account, conversation, and messaging capabilities rather than claiming features that are not implemented.



---



## 🔭 Future Direction



The current package establishes a foundation for messaging applications.



Potential areas for future development include:



- Real-time messaging integration.

- WebSocket and Socket.IO communication.

- Read and delivery status.

- Typing indicators.

- Extended conversation features.

- Additional client SDK capabilities.

- More comprehensive automated tests.



These are possible directions, not claims about functionality already included in the current release.



---



## 📄 License



ALOKI Messenger Kit is licensed under the **MIT License**.



See the `LICENSE` file for details.



---



<p align="center">

  <strong>ALOKI Messenger Kit</strong>

  <br />

  <em>Structured foundations. Reusable communication. Built for developers.</em>

  <br /><br />

  <a href="https://www.npmjs.com/package/messenger-kit">npm Package</a>

  ·

  <a href="https://www.npmjs.com/package/messenger-kit">NPM</a>

</p>
