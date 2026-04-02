# Veribenim Laravel SDK

**GDPR и KVKK Съответна Платформа за Управление на Cookies и Защита на Данните**

[![Packagist Version](https://img.shields.io/packagist/v/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)
[![PHP Version](https://img.shields.io/badge/php-%3E%3D%208.1-blue?style=flat-square)](https://www.php.net/)
[![Laravel Versions](https://img.shields.io/badge/laravel-10%20%7C%2011%20%7C%2012-red?style=flat-square)](https://laravel.com/)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)
[![Downloads](https://img.shields.io/packagist/dm/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)

Veribenim Laravel SDK е **най-комплексното решение за защита на данните и управление на бисквитки за Laravel приложения**. Проектиран за разработчици, този SDK прави обработката на личните данни напълно управляема и прозрачна. Пълно съответствие с GDPR, KVKK и всички международни разпоредби за защита на данните.

**Платформа за управление на съгласието (CMP) на ниво предприятие и управление на правата на субектите на данни (DSAR), базирана на принципите на Privacy by Design, интегрирана директно в Laravel приложения.**

---

## Съдържание

- [Защо Veribenim?](#защо-veribenim)
- [Бърз старт](#бърз-старт)
- [Инсталация](#инсталация)
- [Употреба](#употреба)
- [Стандарти за сигурност](#стандарти-за-сигурност)
- [Изисквания](#изисквания)
- [Лиценз](#лиценз)

---

## Защо Veribenim?

### ✅ Имплементация на защита на данните според най-добрите практики

Veribenim имплементира стандарти, референцирани от органи за защита на данните. Отговорността за обработката на данните е напълно под ваш контрол:

- **Управление на явно съгласие**: Потребителите имат гранулиран контрол върху категориите
- **Следи за одит**: Всяко съгласие, оттегляне и обработка се записва по безопасен начин
- **DSAR (Права на субектите)**: Достъп, изтриване, коригиране, ограничение, преносимост, възражение
- **Изтриване на данни**: Автоматична обработка при оттегляне на съгласието

### 🌍 Глобален GDPR и международни стандарти

- **GDPR Членове 6, 7, 25, 32** — Privacy by Design, явно съгласие
- **Указания на EDPB** — Пълно съответствие с изисквания за бисквитки и съгласие
- **Съответно с ISO 27001** — Криптиране на данни, TLS 1.3+, архитектура без нулеви познания

### 🔒 Сигурност на ниво предприятие

- **Криптиране от край до край**: AES-256 (данни в покой), TLS 1.3+ (при предаване)
- **Архитектура без нулеви познания**: Сървърите на Veribenim не могат да съхранят лични данни
- **Анонимизиране на IP**: Съответно на GDPR, последните октети се премахват

### ⚡ Удобно за разработчици

- **Автоматично открит ServiceProvider**: Инсталирайте и използвайте незабавно
- **Facade API**: Чисти `Veribenim::` повиквания
- **Blade директиви**: Контрол на ниво шаблон
- **Middleware**: Прилагане на съгласието на ниво маршрут

### 🚀 Висока производителност

- **~2KB Gzip**: Минимален размер на JavaScript
- **CDN в множество региони**: 99,99% достъпност
- **Асинхронно зареждане**: Без влияние върху скоростта на страницата

---

## Бърз старт

```bash
composer require veribenim/laravel
php artisan vendor:publish --tag=veribenim-config
```

Добавете към .env:
```env
VERIBENIM_TOKEN=your_api_token_here
VERIBENIM_LANG=bg
```

Добавете към Blade:
```blade
@veribenimScript
```

---

## Инсталация

### Предварителни изисквания

- PHP 8.1+
- Laravel 10, 11 или 12
- ext-curl или ext-json
- Акаунт на Veribenim

### Стъпки на инсталация

**1. Инсталирайте чрез Composer**

```bash
composer require veribenim/laravel
```

**2. Публикувайте конфигурацията**

```bash
php artisan vendor:publish --tag=veribenim-config
```

**3. Конфигурирайте .env**

```env
VERIBENIM_TOKEN=your_site_token_here
VERIBENIM_LANG=bg
VERIBENIM_DEBUG=false
```

**4. Добавете скрипт към основния макет**

```blade
<!DOCTYPE html>
<html>
<head>
    @veribenimScript
</head>
<body>
    <!-- Съдържание -->
</body>
</html>
```

**5. Изпълнете миграции (по желание)**

```bash
php artisan vendor:publish --tag=veribenim-migrations
php artisan migrate
```

---

## Употреба

### Blade директиви

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
    <!-- Маркетингов код -->
@endIfConsented
```

### Употреба на Facade

```php
use Veribenim\Laravel\VeribenimFacade as Veribenim;

// Запишете съгласието на формуляра
Veribenim::logFormConsent(
    formName: 'contact_form',
    consented: true,
    consentText: 'Съгласявам се с условията за защита на данните',
    metadata: ['page_url' => url()->current()]
);

// Получете предпочитанията на потребителя
$preferences = Veribenim::getPreferences();

// Запишете предпочитанията
Veribenim::savePreferences([
    'necessary' => true,
    'analytics' => true,
    'marketing' => false,
    'preferences' => true
]);

// Запишете впечатление на страницата
Veribenim::logImpression();
```

### Middleware за защита на маршрутите

```php
// Изисква се аналитичко съгласие
Route::post('/track', function () {
    //...
})->middleware('veribenim.consent:analytics');

// Множество категории
Route::post('/profile', function () {
    //...
})->middleware('veribenim.consent:analytics,preferences');
```

### DSAR операции

```php
// Подайте искане за достъп до данни
Veribenim::submitDsar(
    requestType: 'erasure',
    fullName: 'John Doe',
    email: 'john@example.com',
    description: 'Изтрийте всичките ми данни'
);
```

**DSAR типове:**

| Тип | GDPR член | Описание |
|---|---|---|
| `access` | Член 15 | Право на достъп |
| `erasure` | Член 17 | Право да бъдат забравени |
| `rectification` | Член 16 | Право на коригиране |
| `restriction` | Член 18 | Право на ограничаване на обработката |
| `portability` | Член 20 | Преносимост на данни |
| `objection` | Член 21 | Право на възражение |
| `automated` | Член 22 | Права срещу автоматизирани решения |

---

## Стандарти за сигурност

### Съответствие с GDPR

- **Член 6** — Законен основание (Съгласие)
- **Член 7** — Условия за съгласие
- **Член 25** — Privacy by Design
- **Член 32** — Мерки за сигурност
- **Указания на EDPB** — Бисквитки и технологии за проследяване

### Криптиране на данни

| Компонент | Технология | Стандарт |
|---|---|---|
| База данни | AES-256-GCM | FIPS 140-2 |
| Предаване | TLS 1.3+ | RFC 8446 |
| Ключове | ECDH P-256 | NIST SP 800-56A |
| Подпис | HMAC-SHA256 | RFC 2104 |

### Анонимизиране на IP

```
Истински IP:    192.168.1.42
Анонимизиран:   192.168.0.0

IPv6:           2001:db8::1
Анонимизиран:   2001:db8::0
```

### Следи за одит

Всички операции се записват:
- Решения за съгласие
- Предавания на формуляри
- DSAR искания
- Изтриване на данни
- API повиквания

Съхранявани минимум **3 години**.

### Сертификати за сигурност

- ✓ ISO 27001
- ✓ ISO 27018
- ✓ SOC 2 Type II
- ✓ Съответно с GDPR
- ✓ Годишни тестове за пенетрация

---

## Изисквания

### Софтуер

```
PHP >= 8.1
Laravel >= 10.0
ext-json
ext-curl (препоръчана)
Composer
```

### Съвместимост с Laravel

| Версия | Поддръжка | PHP мин |
|---|---|---|
| 12.x | ✅ Пълна | 8.2 |
| 11.x | ✅ Пълна | 8.1 |
| 10.x | ✅ Пълна | 8.1 |

### Конфигурация на среда

**Production:**
```env
VERIBENIM_TOKEN=prod_token_xxx
VERIBENIM_LANG=bg
VERIBENIM_DEBUG=false
```

**Development:**
```env
VERIBENIM_TOKEN=dev_token_xxx
VERIBENIM_LANG=bg
VERIBENIM_DEBUG=true
```

---

## Лиценз

MIT License

---

**Разработено с любов от Veribenim**

Web: [https://veribenim.com](https://veribenim.com)
Email: [support@veribenim.com](mailto:support@veribenim.com)
