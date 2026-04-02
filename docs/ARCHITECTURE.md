# Architecture

## Overview

`telemt-bot` is a small Telegram-controlled administration layer for Telemt. Its purpose is to expose a narrow, audited set of operations to trusted Telegram users without exposing the Telemt API directly to the public internet.

The design prioritizes:

- minimal attack surface
- strong input validation
- explicit authorization
- strict separation between chat handling and Telemt API access (https://github.com/telemt/telemt/blob/main/docs/API.md)
- small operational complexity

## Core design

The architecture is intentionally simple:

```text
Telegram
  |
  v
Bot Handlers
  |
  v
Authorization + Validation + Policy
  |
  v
Telemt Service Layer
  |
  v
Telemt Client
  |
  v
Telemt API (localhost/private only)