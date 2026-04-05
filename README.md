# Veribenim Laravel SDK

**KVKK & GDPR Uyumlu Çerez Yönetimi ve Veri Koruma Platformu**

[![Packagist Version](https://img.shields.io/packagist/v/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)
[![PHP Version](https://img.shields.io/badge/php-%3E%3D%208.1-blue?style=flat-square)](https://www.php.net/)
[![Laravel Versions](https://img.shields.io/badge/laravel-10%20%7C%2011%20%7C%2012-red?style=flat-square)](https://laravel.com/)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)
[![Downloads](https://img.shields.io/packagist/dm/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)

Veribenim Laravel SDK, **Türkiye'nin en kapsamlı KVKK uyumlu veri güvenliği ve çerez yönetimi çözümüdür**. Geliştiriciler için tasarlanan bu SDK, kişisel verilerin işlenmesini tamamen kontrol edilebilir ve şeffaf hale getirir. GDPR, KVKK, EDPB ve diğer tüm veri koruma yönetmeliklerine tam uyumluluk sağlar.

**Privacy by Design prensiplerine dayanan, kurumsal seviye Consent Management Platform (CMP) ve Data Subject Rights (DSR) yönetimini Laravel uygulamalarında doğrudan kullanın.**

---

## İçindekiler

- [Neden Veribenim?](#neden-veribenim)
- [Hızlı Başlangıç](#hızlı-başlangıç)
- [Nasıl Kurulur?](#nasıl-kurulur)
- [Kullanım](#kullanım)
- [Güvenlik Standartları](#güvenlik-standartları)
- [Gereksinimler](#gereksinimler)
- [Lisans](#lisans)

---

## Neden Veribenim?

### ✅ Türkiye'de KVKK Uyumluluğunun En İyi Uygulaması

Veribenim, Veri Koruma Kurulu tarafından referans alınan standartları içerir. Kişisel verilerin işlenmesinde yasal sorumluluk tamamen sizin kontrolünüzde:

- **Açık Rıza Yönetimi**: Kullanıcılar tam kontrol altında, kategoriye göre (Gerekli, Analitik, Pazarlama, Tercihler) rıza verir
- **Denetim İzleri**: Her rıza, ret ve veri işleme işlemi güvenli şekilde kaydedilir
- **DSAR (Veri Sahibi Hakları)**: Erişim, silinme, düzeltme, kısıtlama, taşınabilirlik, itiraz, otomatikleştirilmiş karar alma — tümü SDK ile entegre
- **Veri Kalıntısı Yönetimi**: Rıza geri çekildiğinde ilgili veri otomatik işlenmesi

### 🌍 Global GDPR & Uluslararası Standartlar

- **GDPR Artikel 6, 7, 25, 32** — Privacy by Design, Explicit Consent, Data Protection by Default
- **EDPB Rehberliği** — Consent ve çerez politikalarına tam uyum
- **ISO 27001 Uyumlu** — Veri şifreleme, TLS 1.3+ iletim, zero-knowledge mimarisi

### 🔒 Kurumsal Seviye Güvenlik

- **End-to-End Şifreleme**: AES-256 (veriler sunucuda), TLS 1.3+ (transit)
- **Sıfır Bilgi Mimarisi**: Veribenim sunucuları kişisel veri depolayamaz
- **İp Anonimleştirme**: GDPR'a uygun olarak son 1-4 oktet silinir

### ⚡ Geliştirici Dostu

- **ServiceProvider Otomatik Keşif**: Yükle ve kullan
- **Facade API**: Temiz `Veribenim::` çağrıları
- **Blade Directives**: Şablon kontrolü
- **Middleware**: Route-level rıza kontrolü

### 🚀 Yüksek Performans

- **~2KB Gzip**: Minimal JavaScript bundle
- **Çok Bölgeli CDN**: 99.99% uptime garantisi
- **Async Loading**: Sayfa hızını hiç etkilemez

### 🏢 Uçtan Uca KVKK Yönetim Platformu

Veribenim sadece bir çerez SDK'sı değil, **tam kapsamlı bir KVKK/GDPR uyum yönetim platformu**dur:

| Modül | Açıklama |
|-------|----------|
| **Veri Hakkı Talepleri (DSAR)** | KVKK Md.11 / GDPR Md.15-22: 7 talep tipi, otomatik 30 gün deadline |
| **Veri İhlali Yönetimi** | GDPR Md.33: 72 saat countdown, risk seviyesi, durum akışı, otorite bildirim kaydı |
| **VERBİS / RoPA Export** | KVKK VERBİS kaydı ve GDPR Md.30 RoPA: CSV/JSON export, 17 alan |
| **Politika Yönetimi** | Gizlilik politikası, çerez politikası, KVKK aydınlatma — çoklu dil, PDF/HTML export |
| **Uyumluluk Skoru** | 17 kural, 5 kategori, A-F notlandırma, düzeltme önerileri |
| **Form Rızası Takibi** | İletişim, üyelik, bülten formlarındaki KVKK onayını kayıt altına alma |
| **Webhook Sistemi** | 7 olay tipi, HMAC-SHA256, Slack/Teams/n8n entegrasyonu |
| **Çerez Tarayıcı** | 50+ bilinen tracker otomatik tespiti |
| **Tercih Merkezi** | Kalıcı panel + DSAR entegrasyonu |
| **Veri Envanteri** | KVKK Md.16 / GDPR Md.30: Departman ve süreç bazlı veri haritalama, 20 veri kategorisi, VERBİS uyumlu export |
| **Saklama-İmha Otomasyonu** | KVKK Md.7 / GDPR Md.17: Saklama politikaları, otomatik imha, imha tutanakları, 5 imha yöntemi |
| **Risk Yönetimi** | KVKK Md.12 / GDPR Md.35: 5x5 risk matrisi, 7 risk kategorisi, aksiyon takibi, risk raporu export |
| **İç Denetim & Aksiyon Takibi** | 6 denetim tipi, 0-100 puanlama, aksiyon atama ve gecikme takibi |
| **Doküman Şablonları** | 10 hazır KVKK/GDPR şablonu, değişken sistemi, çoklu dil, versiyon takibi |
| **Rıza Versiyonlama** | Onay metni versiyon takibi, yeniden onay mekanizması, versiyon karşılaştırma |
| **AI Asistan** | RAG tabanlı KVKK/GDPR bilgi asistanı |

---

## Hızlı Başlangıç

```bash
composer require veribenim/laravel
php artisan vendor:publish --tag=veribenim-config
```

.env dosyasına:
```env
VERIBENIM_TOKEN=your_api_token_here
VERIBENIM_LANG=tr
```

Blade dosyanıza:
```blade
@veribenimScript
```

---

## Nasıl Kurulur?

### Ön Koşullar

- PHP 8.1+
- Laravel 10, 11, 12
- ext-curl veya ext-json
- Veribenim hesabı

### Kurulum Adımları

**1. Composer ile kütüphane yükle**

```bash
composer require veribenim/laravel
```

**2. Config dosyasını yayınla**

```bash
php artisan vendor:publish --tag=veribenim-config
```

**3. .env dosyasını yapılandır**

```env
VERIBENIM_TOKEN=your_site_token_here
VERIBENIM_LANG=tr
VERIBENIM_DEBUG=false
```

**4. Ana layout dosyanıza script ekle**

```blade
<!DOCTYPE html>
<html>
<head>
    @veribenimScript
</head>
<body>
    <!-- İçerik -->
</body>
</html>
```

**5. Migrasyonları çalıştır (opsiyonel)**

```bash
php artisan vendor:publish --tag=veribenim-migrations
php artisan migrate
```

---

## Kullanım

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
    <!-- Marketing code -->
@endIfConsented
```

### Facade Kullanımı

```php
use Veribenim\Laravel\VeribenimFacade as Veribenim;

// Form rızası kaydet
Veribenim::logFormConsent(
    formName: 'contact_form',
    consented: true,
    consentText: 'KVKK şartlarını kabul ediyorum',
    metadata: ['page_url' => url()->current()]
);

// Kullanıcı tercihlerini al
$preferences = Veribenim::getPreferences();

// Tercihler kaydet
Veribenim::savePreferences([
    'necessary' => true,
    'analytics' => true,
    'marketing' => false,
    'preferences' => true
]);

// Sayfa ziyaretini logla
Veribenim::logImpression();
```

### Middleware ile Rota Koruması

```php
// Analytics rızası gerekli
Route::post('/track', function () {
    //...
})->middleware('veribenim.consent:analytics');

// Birden fazla kategori
Route::post('/profile', function () {
    //...
})->middleware('veribenim.consent:analytics,preferences');
```

### DSAR İşlemleri

```php
// Veri sahibi hakları isteği gönder
Veribenim::submitDsar(
    requestType: 'erasure',    // access, erasure, rectification, etc.
    fullName: 'John Doe',
    email: 'john@example.com',
    description: 'Tüm verilerimi silin'
);
```

**DSAR Türleri:**

| Tür | GDPR Madde | Açıklama |
|---|---|---|
| `access` | Artikel 15 | Kişisel verilere erişim |
| `erasure` | Artikel 17 | Unutulma Hakkı |
| `rectification` | Artikel 16 | Yanlış veri düzeltme |
| `restriction` | Artikel 18 | Veri işlemesini kısıtlama |
| `portability` | Artikel 20 | Taşınabilir formatta veri alma |
| `objection` | Artikel 21 | İtiraz etme hakkı |
| `automated` | Artikel 22 | Otomatik kararlar karşı itiraz |

---

## Güvenlik Standartları

### KVKK Uyumluluğu (Türkiye Veri Koruma Yasası)

Veribenim, Veri Koruma Kurulu'nun rehberliklerini tam olarak takip eder:

- **Açık Rıza**: Kullanıcılar her kategori için ayrı ayrı rıza verirler
- **Rıza Kaydı**: Her rıza, ret ve geri çekilme güvenli şekilde loglanır
- **Veri İşleme Kaydı**: Tüm işlemler denetim izinde tutulur
- **Veri Silme**: Rıza geri çekildiğinde, 30 gün içinde silinir
- **Veri Sorumlusu**: Tam sorumluluk yönetimi sağlanır

### GDPR Uyumluluğu (Avrupa Birliği)

- **Artikel 6** — Yasal dayanak (Rıza)
- **Artikel 7** — Rıza koşulları (Açık, bilgili, somut)
- **Artikel 12-22** — Veri sahibi hakları
- **Artikel 25** — Privacy by Design
- **Artikel 32** — Uygun güvenlik önlemleri
- **EDPB Kararları** — Çerez ve tracking teknolojileri

### Veri Şifreleme

| Bileşen | Teknoloji | Standart |
|---|---|---|
| Veritabanı | AES-256-GCM | FIPS 140-2 |
| İletim | TLS 1.3+ | RFC 8446 |
| Anahtarlar | ECDH P-256 | NIST SP 800-56A |
| İmza | HMAC-SHA256 | RFC 2104 |

### IP Anonimleştirme

```
Gerçek IP:  192.168.1.42
Anonimleştirilmiş: 192.168.0.0

IPv6:       2001:db8::1
Anonimleştirilmiş: 2001:db8::0
```

### Denetim İzleri

Tüm işlemler kaydedilir:
- Rıza verme/geri çekilme
- Form gönderişleri
- DSAR istekleri
- Veri silme işlemleri
- API çağrıları

Minimum **3 yıl** saklanır.

### Güvenlik Sertifikaları

- ✓ ISO 27001
- ✓ ISO 27018
- ✓ SOC 2 Type II
- ✓ GDPR Uyumlu
- ✓ KVKK Uyumlu
- ✓ Yıllık Penetrasyon Testi

---

## Gereksinimler

### Yazılım

```
PHP >= 8.1
Laravel >= 10.0
ext-json
ext-curl (önerilen)
Composer
```

### Laravel Uyumluluğu

| Sürüm | Destek | PHP Min |
|---|---|---|
| 12.x | ✅ Tam | 8.2 |
| 11.x | ✅ Tam | 8.1 |
| 10.x | ✅ Tam | 8.1 |

### Ortam Konfigürasyonu

**Production:**
```env
VERIBENIM_TOKEN=prod_token_xxx
VERIBENIM_LANG=tr
VERIBENIM_DEBUG=false
```

**Development:**
```env
VERIBENIM_TOKEN=dev_token_xxx
VERIBENIM_LANG=tr
VERIBENIM_DEBUG=true
```

---

## Lisans

MIT License

---

**Veribenim tarafından geliştirildi**

Web: [https://veribenim.com](https://veribenim.com)
Email: [support@veribenim.com](mailto:support@veribenim.com)
