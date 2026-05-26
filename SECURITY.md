# Security Policy

## Supported Versions

The latest public release of RavenHawkTech SMTP Mailer is the supported version.

## Reporting a Vulnerability

Please do not publicly disclose security issues before they are reviewed.

To report a vulnerability, open a private contact request through RavenHawkTech or contact the repository maintainer directly.

## Security Scope

This plugin stores SMTP configuration in WordPress options and configures WordPress PHPMailer through `phpmailer_init`.

Security-sensitive areas include:

- SMTP host, username, and password handling
- WordPress admin-only settings access
- Diagnostic output redaction
- Mail log visibility
- Capability checks for settings and log clearing

## Notes

Do not share SMTP passwords, diagnostic transcripts containing private infrastructure details, or raw server logs publicly.
