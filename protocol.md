# Chat Protocol based on MQTT v5

## Abstract

This document delineates a chat protocol grounded in the MQTT v5 standard, promoting secure and decentralized messaging through MQTT clients. The protocol specifies the topic structure, message format, and operational guidelines to foster compatibility and interoperability across a diverse range of MQTT clients.

## Table of Contents

- [Abstract](#abstract)
- [Introduction](#introduction)
- [Conventions and Terminology](#conventions-and-terminology)
- [Protocol Overview](#protocol-overview)
- [Topic Structure](#topic-structure)
- [Message Format](#message-format)
- [Operational Guidelines](#operational-guidelines)
- [Security Considerations](#security-considerations)
- [References](#references)

## Introduction

Messaging platforms have proliferated, driving the necessity for a standardized protocol that can facilitate secure and decentralized communications. Leveraging the MQTT v5 standard, this protocol aims to foster interoperable messaging solutions, enabling individuals to communicate securely and freely through a decentralized messaging ecosystem implemented by different MQTT clients.

## Conventions and Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

### Definitions

- **Client**: An MQTT client that follows the MQTT v5 standard.
- **Server**: An MQTT broker that adheres to the MQTT v5 standard.
- **Topic**: A UTF-8 string used by the MQTT broker to filter messages for each connected client.
- **Payload**: The content of the message exchanged between clients.

## Protocol Overview

This protocol defines the rules and structure for facilitating a chat service using MQTT v5. Clients SHALL communicate through a predefined topic structure, abiding by the message format and operational guidelines set forth in this document.

## Topic Structure

The topic structure for the chat protocol SHALL adhere to the following pattern:

```
chat/{chatroom_id}/{user_id}
```

- **chatroom_id**: A unique identifier for a chat room.
- **user_id**: A unique identifier for a user in a chat room.

### Topic Examples

- `chat/12345/john_doe`: Represents user "john_doe" in chat room "12345".

#### Direct Messages

In addition to chat rooms, the protocol supports direct messaging between users. The topic structure for direct messages SHALL adhere to the following pattern:

```
direct/{sender_user_id}/{receiver_user_id}
```

- **sender_user_id**: A unique identifier for the user sending the message.
- **receiver_user_id**: A unique identifier for the user receiving the message.

#### Direct Messages Subscription Pattern

To receive all direct messages, a client SHALL subscribe to a topic pattern following this structure:

```
direct/+/{user_id}
```

- **user_id**: The unique identifier for the user receiving the messages.

#### Topic Examples

- `direct/+/jane_doe`: Jane Doe will receive all direct messages sent to her from any user.

## Message Format

Messages exchanged MUST follow a JSON structure containing the following fields:

- `timestamp`: The time the message was sent, in ISO 8601 format.
- `message_id`: A unique identifier for the message.
- `user_id`: The unique identifier for the user sending the message.
- `content`: The content of the message.
- `type`: The type of message (e.g., text, image, video).

### Message Examples

```json
{
  "timestamp": "2023-09-12T10:15:30Z",
  "message_id": "abc123",
  "user_id": "john_doe",
  "content": "Hello, world!",
  "type": "text"
}
```

## Operational Guidelines

### Connecting to a Chat Room

- Clients MUST subscribe to the chat room topic to participate in the chat.
- Clients SHOULD retain a local copy of the chat history for future reference.

### Sending Messages

- Clients MUST publish messages to the chat room topic following the defined message format.
- Clients MUST include a unique message ID for each message sent.

### Receiving Messages

- Clients SHOULD acknowledge receipt of messages to ensure message delivery.
- Clients MAY implement mechanisms to handle message types differently (e.g., displaying images versus text).

#### Initiating Direct Messages

- To initiate a direct message, a user MUST subscribe to a topic following the direct message topic structure.
- Users SHOULD notify the receiver through a predefined message in the chat room before initiating a direct message.

#### Participating in Direct Messages

- Users MUST subscribe to the direct message topic to receive direct messages.
- Users SHOULD verify the identity of the user initiating the direct message to prevent impersonation.

#### Receiving Direct Messages

- Clients wishing to receive all direct messages SHOULD subscribe using the wildcard subscription pattern to facilitate this.

## Security Considerations

- Clients SHOULD implement secure communication channels, such as TLS/SSL, to ensure message confidentiality and integrity.
- Clients MUST validate the message format to prevent injection attacks.

## References

- [MQTT Version 5.0 (OASIS Standard)](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html)
- [RFC 2119](https://tools.ietf.org/html/rfc2119)

## Acknowledgements

The author acknowledges the contributions of all individuals and organizations that provided insights and feedback on this protocol.
