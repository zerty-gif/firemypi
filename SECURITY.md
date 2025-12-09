# Security Policy

## Supported Versions

The following versions of FireMyPi are currently supported with security updates:

| Version | Supported          |
| ------- | ------------------ |
| v1.7.x  | :white_check_mark: |
| < v1.7  | :x:                |

## IPFire Compatibility

FireMyPi v1.7 is compatible with:
- IPFire 2.29 Core Update 186 and later
- IPFire 2.29 Core Update 190 (current stable)
- IPFire 2.29 Core Update 194 and beyond

## Reporting a Vulnerability

We take security seriously. If you discover a security vulnerability in FireMyPi, please report it responsibly.

### How to Report

1. **Do NOT open a public GitHub issue** for security vulnerabilities
2. Use [GitHub's private vulnerability reporting](https://github.com/zerty-gif/firemypi/security/advisories/new) to submit security issues confidentially
3. Include as much detail as possible:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Any suggested fixes (optional)

### What to Expect

- **Acknowledgment**: We will acknowledge receipt of your report within 48 hours
- **Assessment**: We will assess the vulnerability and determine its severity
- **Updates**: We will keep you informed of our progress
- **Resolution**: We aim to resolve critical vulnerabilities within 30 days
- **Credit**: With your permission, we will credit you in the security advisory

### Scope

The following are in scope for security reports:

- Authentication bypass or privilege escalation
- Secrets exposure (passwords, keys, certificates)
- Command injection vulnerabilities
- Insecure default configurations
- Vulnerabilities in generated IPFire configurations

### Out of Scope

- Issues in IPFire itself (report to [IPFire Security](https://www.ipfire.org/docs/devel/security))
- Issues requiring physical access to the Raspberry Pi
- Social engineering attacks
- Denial of service attacks

## Security Best Practices

When using FireMyPi:

1. **Protect your secrets**: Keep the `secrets/` directory secure and never commit it to version control
2. **Use strong passwords**: Use the provided password generation scripts (`mk-root-secret.sh`, etc.)
3. **Keep software updated**: Regularly update IPFire using the Web GUI or Pakfire
4. **Backup configurations**: Keep backups of your configuration files in a secure location
5. **Review firewall rules**: Regularly audit your firewall rules for unnecessary openings

## Security Features

FireMyPi includes several security features:

- SSH access limited to GREEN interface (internal network only)
- Web GUI limited to GREEN interface
- Strong password generation utilities
- Support for IPsec VPN with modern encryption
- OpenVPN support for secure remote access
- IP address and location blocking capabilities
