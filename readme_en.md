# Veribenim Laravel SDK

**KVKK & GDPR Compliant Cookie Consent & Data Protection Platform**

[![Packagist Version](https://img.shields.io/packagist/v/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)
[![PHP Version](https://img.shields.io/badge/php-%3E%3D%208.1-blue?style=flat-square)](https://www.php.net/)
[![Laravel Versions](https://img.shields.io/badge/laravel-10%20%7C%2011%20%7C%2012-red?style=flat-square)](https://laravel.com/)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)
[![Downloads](https://img.shields.io/packagist/dm/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)

Veribenim Laravel SDK is **the most comprehensive Data Privacy and Cookie Consent Management solution**. Designed for developers, this SDK makes personal data processing completely controllable and transparent. Provides full compliance with GDPR, KVKK, EDPB and all global data protection regulations.

**Enterprise-grade Consent Management Platform (CMP) and Data Subject Rights (DSR) management based on Privacy by Design principles, integrated directly into Laravel applications.**

---

## Table of Contents

- [Why Veribenim?](#why-veribenim)
- [Quick Start](#quick-start)
- [How to Install](#how-to-install)
- [Usage](#usage)
- [Security Standards](#security-standards)
- [Requirements](#requirements)
- [License](#license)

---

## Why Veribenim?

### ✅ Best Practice Data Privacy Implementation

Veribenim implements standards referenced by regulatory authorities worldwide. Personal data processing responsibility is completely under your control:

- **Explicit Consent Management**: Users maintain granular control across categories (Necessary, Analytics, Marketing, Preferences)
- **Audit Trails**: Every consent decision, withdrawal, and data processing event is securely recorded
- **DSAR (Data Subject Rights)**: Access, erasure, rectification, restriction, portability, objection, automated decision-making — all integrated with the SDK
- **Data Residue Management**: Automatic processing when consent is withdrawn

### 🌍 Global GDPR & International Standards

- **GDPR Articles 6, 7, 25, 32** — Privacy by Design, Explicit Consent, Data Protection by Default
- **EDPB Guidance** — Full compliance with consent and cookie policy requirements
- **ISO 27001 Aligned** — Data encryption, TLS 1.3+ transmission, zero-knowledge architecture

### 🔒 Enterprise-Grade Security

- **End-to-End Encryption**: AES-256 (data at rest), TLS 1.3+ (in transit)
- **Zero-Knowledge Architecture**: Veribenim servers cannot store personal data
- **IP Anonymization**: Compliant with GDPR requirements, last octets removed
- **Silent Audit**: All API calls, form submissions, DSAR requests are logged

### ⚡ Developer Friendly

- **Auto-Discovered ServiceProvider**: Install and use immediately
- **Facade API**: Clean `Veribenim::` calls
- **Blade Directives**: Template-level control
- **Middleware**: Route-level consent enforcement

### 🚀 High Performance

- **~2KB Gzip**: Minimal JavaScript footprint
- **Multi-Region CDN**: 99.99% uptime guarantee
- **Async Loading**: Zero page speed impact

---

## Quick Start

```bash
composer require veribenim/laravel
php artisan vendor:publish --tag=veribenim-config
```

Add to .env:
```env
VERIBENIM_TOKEN=your_api_token_here
VERIBENIM_LANG=en
```

Add to Blade:
```blade
@veribenimScript
```

---

## How to Install

### Prerequisites

- PHP 8.1+
- Laravel 10, 11, or 12
- ext-curl or ext-json
- Veribenim account

### Installation Steps

**1. Install via Composer**

```bash
composer require veribenim/laravel
```

**2. Publish config**

```bash
php artisan vendor:publish --tag=veribenim-config
```

**3. Configure .env**

```env
VERIBENIM_TOKEN=your_site_token_here
VERIBENIM_LANG=en
VERIBENIM_DEBUG=false
```

**4. Add script to main layout**

```blade
<!DOCTYPE html>
<html>
<head>
    @veribenimScript
</head>
<body>
    <!-- Content -->
</body>
</html>
```

**5. Run migrations (optional)**

```bash
php artisan vendor:publish --tag=veribenim-migrations
php artisan migrate
```

---

## Usage

### Blade Directives

#### @veribenimScript

```blade
<head>
    @veribenimScript
</head>
```

#### @ifConsented

```blade
@ifConsented('analytics')
    <script>gtag('config', 'GA_ID');</script>
@endIfConsented

@ifConsented('marketing')
    <!-- Marketing pixel -->
@endIfConsented
```

### Facade Usage

```php
use Veribenim\Laravel\VeribenimFacade as Veribenim;

// Log form consent
Veribenim::logFormConsent(
    formName: 'contact_form',
    consented: true,
    consentText: 'I agree to data processing',
    metadata: ['page_url' => url()->current()]
);

// Get user preferences
$preferences = Veribenim::getPreferences();

// Save preferences
Veribenim::savePreferences([
    'necessary' => true,
    'analytics' => true,
    'marketing' => false,
    'preferences' => true
]);

// Log page impression
Veribenim::logImpression();
```

### Middleware for Route Protection

```php
// Analytics consent required
Route::post('/track', function () {
    //...
})->middleware('veribenim.consent:analytics');

// Multiple categories
Route::post('/profile', function () {
    //...
})->middleware('veribenim.consent:analytics,preferences');
```

### DSAR Operations

```php
// Submit Data Subject Access Request
Veribenim::submitDsar(
    requestType: 'erasure',    // access, erasure, rectification, etc.
    fullName: 'John Doe',
    email: 'john@example.com',
    description: 'Delete all my data'
);
```

**DSAR Types:**

| Type | GDPR Article | Description |
|---|---|---|
| `access` | Article 15 | Right of access |
| `erasure` | Article 17 | Right to be forgotten |
| `rectification` | Article 16 | Correction of inaccurate data |
| `restriction` | Article 18 | Right to restrict processing |
| `portability` | Article 20 | Data portability |
| `objection` | Article 21 | Right to object |
| `automated` | Article 22 | Right against automated decisions |

---

## Security Standards

### GDPR Compliance

- **Article 6** — Lawful basis (Consent)
- **Article 7** — Conditions for consent
- **Articles 12-22** — Data subject rights
- **Article 25** — Privacy by Design
- **Article 32** — Security measures
- **EDPB Decisions** — Cookies and tracking technologies

### Data Encryption

| Component | Technology | Standard |
|---|---|---|
| Database | AES-256-GCM | FIPS 140-2 |
| Transmission | TLS 1.3+ | RFC 8446 |
| Keys | ECDH P-256 | NIST SP 800-56A |
| Signing | HMAC-SHA256 | RFC 2104 |

### IP Anonymization

```
Real IP:    192.168.1.42
Anonymized: 192.168.0.0

IPv6:       2001:db8::1
Anonymized: 2001:db8::0
```

### Audit Logs

All operations are recorded:
- Consent decisions
- Form submissions
- DSAR requests
- Data deletion
- API calls

Retained minimum **3 years**.

### Security Certifications

- ✓ ISO 27001
- ✓ ISO 27018
- ✓ SOC 2 Type II
- ✓ GDPR Compliant
- ✓ Annual Penetration Testing

---

## Requirements

### Software

```
PHP >= 8.1
Laravel >= 10.0
ext-json
ext-curl (recommended)
Composer
```

### Laravel Compatibility

| Version | Support | PHP Min |
|---|---|---|
| 12.x | ✅ Full | 8.2 |
| 11.x | ✅ Full | 8.1 |
| 10.x | ✅ Full | 8.1 |

### Environment Configuration

**Production:**
```env
VERIBENIM_TOKEN=prod_token_xxx
VERIBENIM_LANG=en
VERIBENIM_DEBUG=false
```

**Development:**
```env
VERIBENIM_TOKEN=dev_token_xxx
VERIBENIM_LANG=en
VERIBENIM_DEBUG=true
```

---

## License

MIT License

---

**Built with love by Veribenim**

Web: [https://veribenim.com](https://veribenim.com)
Email: [support@veribenim.com](mailto:support@veribenim.com)
