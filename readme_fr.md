# Veribenim Laravel SDK

**Plateforme de Gestion des Cookies Conforme au RGPD et à la KVKK**

[![Packagist Version](https://img.shields.io/packagist/v/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)
[![PHP Version](https://img.shields.io/badge/php-%3E%3D%208.1-blue?style=flat-square)](https://www.php.net/)
[![Laravel Versions](https://img.shields.io/badge/laravel-10%20%7C%2011%20%7C%2012-red?style=flat-square)](https://laravel.com/)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)
[![Downloads](https://img.shields.io/packagist/dm/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)

Veribenim Laravel SDK est **la solution la plus complète de gestion de la protection des données et des cookies pour Laravel**. Conçu pour les développeurs, ce SDK rend le traitement des données personnelles entièrement contrôlable et transparent. Conformité complète avec le RGPD, la KVKK et tous les règlements internationaux de protection des données.

**Plateforme de gestion du consentement (CMP) de niveau entreprise et gestion des droits des personnes (DSAR) basée sur les principes Privacy by Design, intégrée directement dans les applications Laravel.**

---

## Table des matières

- [Pourquoi Veribenim?](#pourquoi-veribenim)
- [Démarrage rapide](#démarrage-rapide)
- [Installation](#installation)
- [Utilisation](#utilisation)
- [Normes de sécurité](#normes-de-sécurité)
- [Exigences](#exigences)
- [Licence](#licence)

---

## Pourquoi Veribenim?

### ✅ Mise en œuvre de la protection des données conforme aux meilleures pratiques

Veribenim met en œuvre des normes référencées par les autorités de protection des données. La responsabilité du traitement des données est entièrement sous votre contrôle:

- **Gestion des consentements explicites**: Les utilisateurs ont un contrôle granulaire sur les catégories
- **Pistes d'audit**: Chaque consentement, retrait et traitement est enregistré de façon sécurisée
- **DSAR (Droits des personnes)**: Accès, suppression, rectification, restriction, portabilité, opposition
- **Suppression de données**: Traitement automatique lors du retrait du consentement

### 🌍 RGPD mondial et normes internationales

- **Articles 6, 7, 25, 32 du RGPD** — Privacy by Design, consentement explicite
- **Directives du CEPD** — Conformité complète avec les exigences en matière de cookies et de consentement
- **Conforme à ISO 27001** — Chiffrement des données, TLS 1.3+, architecture zero-knowledge

### 🔒 Sécurité de niveau entreprise

- **Chiffrement de bout en bout**: AES-256 (données au repos), TLS 1.3+ (en transit)
- **Architecture zero-knowledge**: Les serveurs Veribenim ne peuvent pas stocker les données personnelles
- **Anonymisation des adresses IP**: Conforme au RGPD, derniers octets supprimés

### ⚡ Convivial pour les développeurs

- **ServiceProvider auto-découvert**: Installez et utilisez immédiatement
- **API Facade**: Appels `Veribenim::` propres et clairs
- **Directives Blade**: Contrôle au niveau du modèle
- **Middleware**: Application du consentement au niveau des routes

### 🚀 Haute performance

- **~2KB Gzip**: Empreinte JavaScript minimale
- **CDN multi-région**: Disponibilité 99,99%
- **Chargement asynchrone**: Aucun impact sur la vitesse des pages

---

## Démarrage rapide

```bash
composer require veribenim/laravel
php artisan vendor:publish --tag=veribenim-config
```

Ajouter à .env:
```env
VERIBENIM_TOKEN=your_api_token_here
VERIBENIM_LANG=fr
```

Ajouter à Blade:
```blade
@veribenimScript
```

---

## Installation

### Prérequis

- PHP 8.1+
- Laravel 10, 11 ou 12
- ext-curl ou ext-json
- Compte Veribenim

### Étapes d'installation

**1. Installation via Composer**

```bash
composer require veribenim/laravel
```

**2. Publier la configuration**

```bash
php artisan vendor:publish --tag=veribenim-config
```

**3. Configurer .env**

```env
VERIBENIM_TOKEN=your_site_token_here
VERIBENIM_LANG=fr
VERIBENIM_DEBUG=false
```

**4. Ajouter le script au layout principal**

```blade
<!DOCTYPE html>
<html>
<head>
    @veribenimScript
</head>
<body>
    <!-- Contenu -->
</body>
</html>
```

**5. Exécuter les migrations (optionnel)**

```bash
php artisan vendor:publish --tag=veribenim-migrations
php artisan migrate
```

---

## Utilisation

### Directives Blade

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
    <!-- Code marketing -->
@endIfConsented
```

### Utilisation de Facade

```php
use Veribenim\Laravel\VeribenimFacade as Veribenim;

// Enregistrer le consentement du formulaire
Veribenim::logFormConsent(
    formName: 'contact_form',
    consented: true,
    consentText: 'J\'accepte la politique de protection des données',
    metadata: ['page_url' => url()->current()]
);

// Obtenir les préférences de l'utilisateur
$preferences = Veribenim::getPreferences();

// Sauvegarder les préférences
Veribenim::savePreferences([
    'necessary' => true,
    'analytics' => true,
    'marketing' => false,
    'preferences' => true
]);

// Enregistrer l'impression de page
Veribenim::logImpression();
```

### Middleware pour la protection des routes

```php
// Consentement analytique requis
Route::post('/track', function () {
    //...
})->middleware('veribenim.consent:analytics');

// Catégories multiples
Route::post('/profile', function () {
    //...
})->middleware('veribenim.consent:analytics,preferences');
```

### Opérations DSAR

```php
// Soumettre une demande d'accès aux données
Veribenim::submitDsar(
    requestType: 'erasure',
    fullName: 'John Doe',
    email: 'john@example.com',
    description: 'Supprimer toutes mes données'
);
```

**Types DSAR:**

| Type | Article RGPD | Description |
|---|---|---|
| `access` | Article 15 | Droit d'accès |
| `erasure` | Article 17 | Droit à l'oubli |
| `rectification` | Article 16 | Droit à la rectification |
| `restriction` | Article 18 | Droit à la limitation du traitement |
| `portability` | Article 20 | Droit à la portabilité des données |
| `objection` | Article 21 | Droit d'opposition |
| `automated` | Article 22 | Droit contre les décisions automatisées |

---

## Normes de sécurité

### Conformité RGPD

- **Article 6** — Base juridique (Consentement)
- **Article 7** — Conditions du consentement
- **Article 25** — Privacy by Design
- **Article 32** — Mesures de sécurité
- **Directives du CEPD** — Cookies et technologies de suivi

### Chiffrement des données

| Composant | Technologie | Norme |
|---|---|---|
| Base de données | AES-256-GCM | FIPS 140-2 |
| Transmission | TLS 1.3+ | RFC 8446 |
| Clés | ECDH P-256 | NIST SP 800-56A |
| Signature | HMAC-SHA256 | RFC 2104 |

### Anonymisation des adresses IP

```
Adresse réelle:     192.168.1.42
Anonymisée:         192.168.0.0

IPv6:               2001:db8::1
Anonymisée:         2001:db8::0
```

### Journaux d'audit

Toutes les opérations sont enregistrées:
- Décisions de consentement
- Soumissions de formulaires
- Demandes DSAR
- Suppression de données
- Appels API

Conservés minimum **3 ans**.

### Certifications de sécurité

- ✓ ISO 27001
- ✓ ISO 27018
- ✓ SOC 2 Type II
- ✓ Conforme RGPD
- ✓ Tests de pénétration annuels

---

## Exigences

### Logiciel

```
PHP >= 8.1
Laravel >= 10.0
ext-json
ext-curl (recommandé)
Composer
```

### Compatibilité Laravel

| Version | Support | PHP Min |
|---|---|---|
| 12.x | ✅ Complet | 8.2 |
| 11.x | ✅ Complet | 8.1 |
| 10.x | ✅ Complet | 8.1 |

### Configuration d'environnement

**Production:**
```env
VERIBENIM_TOKEN=prod_token_xxx
VERIBENIM_LANG=fr
VERIBENIM_DEBUG=false
```

**Development:**
```env
VERIBENIM_TOKEN=dev_token_xxx
VERIBENIM_LANG=fr
VERIBENIM_DEBUG=true
```

---

## Licence

MIT License

---

**Développé avec passion par Veribenim**

Web: [https://veribenim.com](https://veribenim.com)
Email: [support@veribenim.com](mailto:support@veribenim.com)
