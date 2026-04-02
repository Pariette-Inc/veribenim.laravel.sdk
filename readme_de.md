# Veribenim Laravel SDK

**DSGVO & KVKK-konforme Cookie-Verwaltung und Datenschutzplattform**

[![Packagist Version](https://img.shields.io/packagist/v/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)
[![PHP Version](https://img.shields.io/badge/php-%3E%3D%208.1-blue?style=flat-square)](https://www.php.net/)
[![Laravel Versions](https://img.shields.io/badge/laravel-10%20%7C%2011%20%7C%2012-red?style=flat-square)](https://laravel.com/)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)
[![Downloads](https://img.shields.io/packagist/dm/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)

Veribenim Laravel SDK ist **die umfassendste Datenschutz- und Cookie-Verwaltungslösung für Laravel-Anwendungen**. Entwickler können die Verarbeitung personenbezogener Daten vollständig kontrollieren und transparent gestalten. Vollständige Einhaltung der DSGVO, KVKK und aller internationalen Datenschutzbestimmungen.

**Enterprise-Grade Consent Management Platform (CMP) und Data Subject Rights (DSR) Management nach Privacy-by-Design-Prinzipien, direkt in Laravel-Anwendungen integriert.**

---

## Inhaltsverzeichnis

- [Warum Veribenim?](#warum-veribenim)
- [Schnellstart](#schnellstart)
- [Installation](#installation)
- [Verwendung](#verwendung)
- [Sicherheitsstandards](#sicherheitsstandards)
- [Anforderungen](#anforderungen)
- [Lizenz](#lizenz)

---

## Warum Veribenim?

### ✅ Best-Practice-Datenschutzumsetzung

Veribenim implementiert Standards, auf die sich Datenschutzbehörden beziehen. Die Verantwortung für die Datenverarbeitung liegt vollständig in Ihrer Kontrolle:

- **Explizite Einwilligungsverwaltung**: Nutzer haben granulare Kontrolle über Kategorien
- **Audit-Trails**: Jede Einwilligung, jeder Widerruf und jede Verarbeitung wird sicher protokolliert
- **DSAR (Betroffenenrechte)**: Auskunft, Löschung, Berichtigung, Einschränkung, Portabilität, Widerspruch
- **Datenlöschung**: Automatische Verarbeitung bei Widerruf

### 🌍 Globale DSGVO & internationale Standards

- **DSGVO Artikel 6, 7, 25, 32** — Privacy by Design, explizite Einwilligung
- **EDSA-Richtlinien** — Vollständige Konformität mit Cookie- und Datenschutzrichtlinien
- **ISO 27001 konform** — Datenverschlüsselung, TLS 1.3+, Zero-Knowledge-Architektur

### 🔒 Enterprise-Sicherheit

- **End-to-End-Verschlüsselung**: AES-256 (Ruhedaten), TLS 1.3+ (Transit)
- **Zero-Knowledge-Architektur**: Veribenim-Server können keine Daten speichern
- **IP-Anonymisierung**: DSGVO-konform, letzte Oktets entfernt

### ⚡ Entwicklerfreundlich

- **Auto-Discovery**: Einfach installieren und verwenden
- **Facade API**: Saubere `Veribenim::` Aufrufe
- **Blade Directives**: Template-Kontrolle auf Vorlagenbene
- **Middleware**: Route-Level-Consent-Durchsetzung

### 🚀 Hohe Leistung

- **~2KB Gzip**: Minimale JavaScript-Größe
- **Multi-Region-CDN**: 99,99% Verfügbarkeit
- **Asynchrones Laden**: Kein Einfluss auf Seitengeschwindigkeit

---

## Schnellstart

```bash
composer require veribenim/laravel
php artisan vendor:publish --tag=veribenim-config
```

In .env hinzufügen:
```env
VERIBENIM_TOKEN=your_api_token_here
VERIBENIM_LANG=de
```

In Blade hinzufügen:
```blade
@veribenimScript
```

---

## Installation

### Voraussetzungen

- PHP 8.1+
- Laravel 10, 11 oder 12
- ext-curl oder ext-json
- Veribenim-Konto

### Installationsschritte

**1. Installation via Composer**

```bash
composer require veribenim/laravel
```

**2. Konfiguration veröffentlichen**

```bash
php artisan vendor:publish --tag=veribenim-config
```

**3. .env konfigurieren**

```env
VERIBENIM_TOKEN=your_site_token_here
VERIBENIM_LANG=de
VERIBENIM_DEBUG=false
```

**4. Script zum Hauptlayout hinzufügen**

```blade
<!DOCTYPE html>
<html>
<head>
    @veribenimScript
</head>
<body>
    <!-- Inhalt -->
</body>
</html>
```

**5. Migrationen ausführen (optional)**

```bash
php artisan vendor:publish --tag=veribenim-migrations
php artisan migrate
```

---

## Verwendung

### Blade-Direktiven

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
    <!-- Marketing-Code -->
@endIfConsented
```

### Facade-Verwendung

```php
use Veribenim\Laravel\VeribenimFacade as Veribenim;

// Formulareinwilligung protokollieren
Veribenim::logFormConsent(
    formName: 'contact_form',
    consented: true,
    consentText: 'Ich akzeptiere Datenschutzbestimmungen',
    metadata: ['page_url' => url()->current()]
);

// Benutzereinstellungen abrufen
$preferences = Veribenim::getPreferences();

// Einstellungen speichern
Veribenim::savePreferences([
    'necessary' => true,
    'analytics' => true,
    'marketing' => false,
    'preferences' => true
]);

// Seitenzugriff protokollieren
Veribenim::logImpression();
```

### Middleware für Routenschutz

```php
// Analytics-Einwilligung erforderlich
Route::post('/track', function () {
    //...
})->middleware('veribenim.consent:analytics');

// Mehrere Kategorien
Route::post('/profile', function () {
    //...
})->middleware('veribenim.consent:analytics,preferences');
```

### DSAR-Operationen

```php
// Anfrage zu Betroffenenrechten einreichen
Veribenim::submitDsar(
    requestType: 'erasure',
    fullName: 'John Doe',
    email: 'john@example.com',
    description: 'Alle meine Daten löschen'
);
```

**DSAR-Typen:**

| Typ | DSGVO-Artikel | Beschreibung |
|---|---|---|
| `access` | Artikel 15 | Auskunftsrecht |
| `erasure` | Artikel 17 | Recht auf Vergessenwerden |
| `rectification` | Artikel 16 | Berichtigungsrecht |
| `restriction` | Artikel 18 | Einschränkungsrecht |
| `portability` | Artikel 20 | Datenportabilität |
| `objection` | Artikel 21 | Widerspruchsrecht |
| `automated` | Artikel 22 | Recht gegen automatisierte Entscheidungen |

---

## Sicherheitsstandards

### DSGVO-Konformität

- **Artikel 6** — Rechtmäßige Grundlage (Einwilligung)
- **Artikel 7** — Einwilligungsbedingungen
- **Artikel 25** — Privacy by Design
- **Artikel 32** — Sicherheitsmaßnahmen
- **EDSA-Richtlinien** — Cookies und Tracking

### Datenverschlüsselung

| Komponente | Technologie | Standard |
|---|---|---|
| Datenbank | AES-256-GCM | FIPS 140-2 |
| Übertragung | TLS 1.3+ | RFC 8446 |
| Schlüssel | ECDH P-256 | NIST SP 800-56A |
| Signatur | HMAC-SHA256 | RFC 2104 |

### IP-Anonymisierung

```
Echte IP:    192.168.1.42
Anonymisiert: 192.168.0.0

IPv6:        2001:db8::1
Anonymisiert: 2001:db8::0
```

### Audit-Logs

Alle Operationen werden protokolliert:
- Einwilligungsentscheidungen
- Formulareinreichungen
- DSAR-Anfragen
- Datenlöschung
- API-Aufrufe

Mindestens **3 Jahre** aufbewahrt.

### Sicherheitszertifikate

- ✓ ISO 27001
- ✓ ISO 27018
- ✓ SOC 2 Type II
- ✓ DSGVO-konform
- ✓ Jährliche Penetrationstests

---

## Anforderungen

### Software

```
PHP >= 8.1
Laravel >= 10.0
ext-json
ext-curl (empfohlen)
Composer
```

### Laravel-Kompatibilität

| Version | Support | PHP Min |
|---|---|---|
| 12.x | ✅ Vollständig | 8.2 |
| 11.x | ✅ Vollständig | 8.1 |
| 10.x | ✅ Vollständig | 8.1 |

### Umgebungskonfiguration

**Production:**
```env
VERIBENIM_TOKEN=prod_token_xxx
VERIBENIM_LANG=de
VERIBENIM_DEBUG=false
```

**Development:**
```env
VERIBENIM_TOKEN=dev_token_xxx
VERIBENIM_LANG=de
VERIBENIM_DEBUG=true
```

---

## Lizenz

MIT License

---

**Mit Liebe von Veribenim entwickelt**

Web: [https://veribenim.com](https://veribenim.com)
Email: [support@veribenim.com](mailto:support@veribenim.com)
