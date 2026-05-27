# RavenHawkTech SMTP Mailer

RavenHawkTech SMTP Mailer is a lightweight WordPress SMTP relay and diagnostics plugin built for RavenHawkTech sites and plugins. It hooks into WordPress `wp_mail()` so WordPress core, RavenHawkTech plugins, and compatible third-party plugins can send mail through a configured SMTP account instead of relying on the host's default PHP mail behavior.

This repository contains the RavenHawkTech SMTP Mailer plugin package, update metadata, screenshots directory, and repository documentation.

## Current Version

**v1.0.5**

The plugin is intended to make email delivery easier to troubleshoot, especially on shared hosting where SMTP issues may involve DNS, Cloudflare proxying, TLS settings, authentication methods, or provider-side restrictions.

## What it does

- Overrides WordPress mail delivery through `phpmailer_init` when enabled.
- Sends all WordPress `wp_mail()` traffic through the configured SMTP relay.
- Provides RavenHawkTech admin menu integration.
- Provides a RavenHawkTech-styled settings page.
- Supports common SMTP encryption modes:
  - None
  - STARTTLS
  - SSL/TLS
- Supports authentication mode selection:
  - LOGIN
  - PLAIN
  - CRAM-MD5
- Provides port presets:
  - 465 for SSL/TLS
  - 587 for STARTTLS
  - 25 for legacy SMTP
  - 2525 for alternate SMTP
  - Custom port
- Includes diagnostics modes for faster troubleshooting.
- Logs recent WordPress mail handoff attempts.

## Admin location

After activation, the settings page appears under:

```text
RavenHawkTech → SMTP Mailer
```

If the RavenHawkTech parent menu does not already exist, the plugin creates it automatically and places SMTP Mailer underneath it.

## SMTP settings

The settings page allows configuration of:

- Enable or disable SMTP override
- SMTP host
- Port preset or custom port
- Encryption type
- Authentication type
- Username
- Password
- From email
- From name
- Mail timeout
- Connection timeout
- SMTP debug level

For most cPanel or shared hosting mailboxes, a typical configuration is:

```text
Host: mail.example.com or the server hostname from cPanel
Port: 465
Encryption: SSL/TLS
Authentication: LOGIN
Username: full mailbox email address
From Email: same full mailbox email address
```

For STARTTLS-based SMTP, use:

```text
Port: 587
Encryption: STARTTLS
Authentication: LOGIN
```

## Diagnostics

The plugin includes selectable diagnostics so testing does not always require a full email send.

Available diagnostic modes:

```text
DNS only - fastest
TCP connect only
SMTP handshake only
Full send test
```

### DNS only

Checks whether the SMTP hostname resolves. This is useful for identifying DNS mistakes, such as a mail hostname accidentally being proxied through Cloudflare.

### TCP connect only

Attempts to open a socket to the SMTP host and port. This helps identify blocked ports, firewall issues, or hosting restrictions.

### SMTP handshake only

Connects to the SMTP server and reads the banner/capabilities without sending a message. This can show supported authentication methods such as `PLAIN` or `LOGIN`.

### Full send test

Runs the full WordPress `wp_mail()` flow and attempts to send a test message. This mode captures PHPMailer debug output and is useful for authentication, TLS, relay, and sender-policy troubleshooting.

## Sent mail log

The Sent Mail Log tab captures recent WordPress mail handoff attempts. It shows:

- Date sent
- Recipient
- Subject
- Simple status
- Status message

The log is intended for quick troubleshooting and visibility into whether WordPress attempted to send an email. A successful log entry means WordPress/PHPMailer handed the message to the mail system successfully; final mailbox delivery still depends on the SMTP provider and recipient-side filtering.

## How it works

The plugin uses WordPress and PHPMailer hooks:

```php
phpmailer_init
wp_mail_succeeded
wp_mail_failed
```

When SMTP is enabled, the plugin configures PHPMailer with the saved SMTP host, port, encryption, authentication type, username, password, From email, and From name.

Any plugin or WordPress feature that uses `wp_mail()` can then send through the configured SMTP relay.

Examples include:

- WordPress password reset messages
- WordPress admin notifications
- RavenHawkTech Bulletin Board account verification emails
- RavenHawkTech Bulletin Board moderator notifications
- RavenHawkTech plugin alerts and notifications

## Security notes

- Use a real mailbox for SMTP authentication.
- Match the From Email to the authenticated mailbox when possible.
- Avoid using aliases, forwarders, or receive-only addresses for SMTP authentication.
- Cloudflare Email Routing addresses are receive-only and cannot be used as SMTP accounts.
- Do not proxy SMTP hostnames through Cloudflare. Mail hostnames should normally be DNS only.

## Recommended troubleshooting flow

1. Confirm the mailbox exists and can log into webmail.
2. Confirm the SMTP hostname is DNS only if managed through Cloudflare.
3. Run DNS only diagnostics.
4. Run TCP connect only diagnostics.
5. Run SMTP handshake only diagnostics.
6. Confirm the server advertises the selected authentication method.
7. Run Full send test.
8. Review the Sent Mail Log.

## Repository Structure

This repository tracks the SMTP Mailer plugin package, update metadata, screenshots, and documentation.

```text
/
├─ README.md
├─ SECURITY.md
├─ COPYRIGHT.md
├─ releases/
│  └─ ravenhawk-smtp-mailer.zip
├─ screenshots/
│  └─ README.md
└─ updates/
   └─ rht-smtp-mailer.json
```

The release package contains the full WordPress plugin folder, including the plugin PHP file and RavenHawkTech admin integration.

## Installation

1. In WordPress Admin, go to **Plugins → Add Plugin**.
2. Click **Upload Plugin**.
3. Choose the RavenHawkTech SMTP Mailer package.
4. Click **Install Now**.
5. Activate the plugin.
6. Open **RavenHawkTech → SMTP Mailer** in WordPress Admin.

## Future releases

Planned future work includes tighter integration with other RavenHawkTech plugins so they can detect and use RavenHawkTech SMTP Mailer more intelligently.

Potential future enhancements:

- Interconnect RavenHawkTech plugins with SMTP Mailer status checks.
- Add compatibility notices inside other RavenHawkTech plugins when SMTP is not configured.
- Add centralized RavenHawkTech notification routing.
- Add per-plugin mail categories or tags in the Sent Mail Log.
- Add resend support for failed plugin-generated emails.
- Add exportable diagnostics bundles for support and troubleshooting.
- Add optional masking/redaction controls for diagnostic output.

## Support RavenHawkTech

<a href="https://www.buymeacoffee.com/wbakke7496c" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me a Coffee" style="height: 60px !important;width: 217px !important;" ></a>

## RavenHawkTech

Website: https://ravenhawktech.com

## License

This plugin is maintained for RavenHawkTech WordPress infrastructure and related plugin workflows.
