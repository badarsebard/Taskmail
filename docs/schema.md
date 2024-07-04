---
title: Data Schema
permalink: /schema
---

:warning: __WARNING: This document is a work in progress and may not be accurate or complete during active development.__ :warning:

# App Collections

## Accounts `accounts`

| Field             | Type                | Notes           |
|-------------------|---------------------|-----------------|
| _id               | UUIDv4              | Clover internal |
| address           | string              | Email address   |
| active            | bool                |                 |
| folderDeltaLink   | string              |                 |
| messageDeltaLinks | map\[string\]string |                 |
| expandMenu        | bool                |                 |

# Account Collections

## Folders `folders`

| Field          | Type     | Notes           |
|----------------|----------|-----------------|
| _id            | UUIDv4   | Clover internal |
| id             | string   | MS Graph Id     |
| displayName    | string   |                 |
| parentFolderId | string   | MS Graph Id     |
| isHidden       | bool     |                 |
| childFolders   | string[] | MS Graph Ids    |

## Messages `messages`

| Field                      | Type      | Notes           |
|----------------------------|-----------|-----------------|
| _id                        | UUIDv4    | Clover internal |
| id                         | string    | MS Graph Id     |
| conversationIndex          | byte[]    |                 |
| sentDateTime               | timestamp |                 |
| sender                     | object    |                 |
| ⤷ name                     | string    |                 |
| ⤷ address                  | string    |                 |
| subject                    | string    |                 |
| bodyPreview                | string    |                 |
| @odata.type                | string    |                 |
| createdDateTime            | timestamp |                 |
| lastModifiedDateTime       | timestamp |                 |
| changeKey                  | string    |                 |
| categories                 | string[]  |                 |
| receivedDateTime           | timestamp |                 |
| hasAttachments             | bool      |                 |
| internetMessageId          | string    |                 |
| importance                 | string    | enum: 0, 1, 2   |
| parentFolderId             | string    | MS Graph Id     |
| conversationId             | string    | MS Graph Id     |
| isDeliveryReceiptRequested | bool      |                 |
| isReadReceiptRequested     | bool      |                 |
| isRead                     | bool      |                 |
| isDraft                    | bool      |                 |
| webLink                    | string    |                 |
| inferenceClassification    | string    | enum: 0, 1      |
| body                       | object    |                 |
| ⤷ contentType              | string    |                 |
| ⤷ content                  | string    |                 |
| from                       | object    |                 |
| ⤷ name                     | string    |                 |
| ⤷ address                  | string    |                 |
| toRecipients               | object[]  |                 |
| ⤷ name                     | string    |                 |
| ⤷ address                  | string    |                 |
| ccRecipients               | object[]  |                 |
| ⤷ name                     | string    |                 |
| ⤷ address                  | string    |                 |
| bccRecipients              | object[]  |                 |
| ⤷ name                     | string    |                 |
| ⤷ address                  | string    |                 |
| replyTo                    | object[]  |                 |
| ⤷ name                     | string    |                 |
| ⤷ address                  | string    |                 |
| flag                       | object    |                 |
| ⤷ flagStatus               | string    | enum: 0, 1, 2   |
| attachments                | object[]  |                 |
| ⤷ id                       | string    | MS Graph Id     |
| ⤷ contentType              | string    |                 |
| ⤷ isInLine                 | bool      |                 |
| ⤷ lastModifiedDateTime     | timestamp |                 |
| ⤷ name                     | string    |                 |
| ⤷ size                     | int       |                 |

## Attachments `attachments`

| Field                | Type      | Notes           |
|----------------------|-----------|-----------------|
| _id                  | UUIDv4    | Clover internal |
| id                   | string    | MS Graph Id     |
| contentType          | string    |                 |
| isInLine             | bool      |                 |
| lastModifiedDateTime | timestamp |                 |
| name                 | string    |                 |
| size                 | int       |                 |
| contentBytes         | byte[]    |                 |
| contentId            | string    |                 |
| contentLocation      | string    |                 |

## Conversations `conversations`

| Field               | Type      | Notes           |
|---------------------|-----------|-----------------|
| _id                 | UUIDv4    | Clover internal |
| id                  | string    | MS Graph Id     |
| messages            | object[]  | MessageSummary  |
| ⤷ id                | string    | MS Graph Id     |
| ⤷ conversationIndex | byte[]    |                 |
| ⤷ receivedDateTime  | timestamp |                 |
| ⤷ sender            | object    |                 |
| ⤷ ⤷ name            | string    |                 |
| ⤷ ⤷ address         | string    |                 |
| ⤷ subject           | string    |                 |
| ⤷ bodyPreview       | string    |                 |
| folders             | string[]  | MS Graph Ids    |
| latestTime          | timestamp |                 |

## Cards `cards`

| Field          | Type      | Notes                     |
|----------------|-----------|---------------------------|
| _id            | UUIDv4    | Clover internal           |
| done           | bool      |                           |
| seen           | bool      |                           |
| conversationId | UUIDv4    | fk: conversations._id     |
| sender         | object    | message only              |
| ⤷ name         | string    |                           |
| ⤷ address      | string    |                           |
| folders        | string[]  | fk: conversations.folders |
| title          | string    |                           |
| time           | timestamp |                           |
| bodyPreview    | string    |                           |
| body           | string    |                           |
| column         | UUIDv4    | fk: columns._id           |
| tags           | object[]  |                           |
| ⤷ name         | string    |                           |
| ⤷ color        | string    | 6-digit HEX code          |
| snoozeDate     | timestamp |                           |
| dueDate        | timestamp |                           |
| todoItems      | object[]  |                           |
| ⤷ text         | string    |                           |
| ⤷ completed    | bool      |                           |
| notes          | object[]  |                           |
| ⤷ text         | string    |                           |
| ⤷ time         | timestamp |                           |
| attachments    | UUIDv4[]  | fk: attachments._id       |
| parentId       | UUIDv4    | fk: cards._id             |
| hasChildren    | bool      |                           |

## Columns `columns`

| Field     | Type   | Notes                                                           |
|-----------|--------|-----------------------------------------------------------------|
| _id       | UUIDv4 | Clover internal                                                 |
| board     | string | Value is static `default. Other values to be implemented later. |
| position  | int    |                                                                 |
| title     | string |                                                                 |
| color     | string | 6-digit HEX code                                                |
| collapsed | bool   |                                                                 |
| sortType  | string | enum: due, received, created                                    |
| sortDir   | string | enum: asc, desc                                                 |