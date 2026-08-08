# Security Policy

## Supported Versions

We release patches for security vulnerabilities for the following versions:

| Version | Supported          |
| ------- | ------------------ |
| 2.x.x   | :white_check_mark: |
| 1.x.x   | :x:                |

## Reporting a Vulnerability

If you've found a vulnerability or a potential vulnerability in verifirewall please let us know at [security-alert@verifirewall.io](mailto:security-alert@verifirewall.io). We'll send a confirmation email to acknowledge your report within 24 hours, and we'll send an additional email when we've identified the issue positively or negatively.

A process will be activated upon determining the validity of a reported security vulnerability, which will end with releasing a fix and deciding on the applicable disclosure actions. The reporter of the issue will receive updates of this process' progress.

## Disclosure Policy

- We follow responsible disclosure practices
- Security issues will be patched in supported versions before public disclosure
- We aim to release fixes within 90 days of verified reports
- Credit will be given to reporters who follow responsible disclosure (unless they request anonymity)

## Security Best Practices

When deploying verifirewall:
- Always use the latest supported version
- Keep the ML model updated (download advanced model from [verifirewall portal](https://my.verifirewall.io))
- Run with minimal required privileges
- Use TLS for all management communications
- Regularly review audit logs and security events
- Follow the [deployment hardening guide](https://docs.verifirewall.io/security/hardening)

## Previous Security Audits

- **LEXFO Audit (Sep-Oct 2022)**: Independent third-party code audit. [Full report](https://github.com/verifirewall/verifirewall/blob/main/LEXFO-CHP20221014-Report-Code_audit-verifirewall-v1.2.pdf)

## Contact

- Security issues: security-alert@verifirewall.io
- General inquiries: opensource@verifirewall.io
