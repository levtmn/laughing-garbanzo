# Security

## Security goals

This project exists to provide a narrow and controlled way to manage Telemt users via Telegram. The primary goal is not feature breadth. The primary goal is reducing the chance of misuse, accidental exposure, and credential leakage.

The baseline assumption is that Telemt is a privileged internal service and must not be exposed directly to the public internet.

## Security model

### Trusted actors

Trusted actors are limited to:

- the host operating system administrator
- the bot process running under a dedicated service account
- Telegram users whose numeric Telegram IDs are explicitly allowlisted

Everything else is untrusted.

### Untrusted input

Treat all Telegram input as hostile by default, including input from known users. This includes:

- usernames
- plan names
- extension values
- callback payloads
- forwarded content
- message formatting

Never trust input because it came through Telegram.

## Core controls

### 1. Explicit Telegram allowlist

Only a fixed set of numeric Telegram user IDs may use the bot.

Required behavior:

- deny all non-allowlisted users
- log access denial safely
- do not grant access based on Telegram username alone
- do not trust display names

Recommended environment variable:

```env
ALLOWED_TELEGRAM_IDS=123456789,987654321