# Veribenim Laravel SDK

**منصة إدارة ملفات تعريف الارتباط والخصوصية المتوافقة مع GDPR و KVKK**

<div dir="rtl">

[![Packagist Version](https://img.shields.io/packagist/v/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)
[![PHP Version](https://img.shields.io/badge/php-%3E%3D%208.1-blue?style=flat-square)](https://www.php.net/)
[![Laravel Versions](https://img.shields.io/badge/laravel-10%20%7C%2011%20%7C%2012-red?style=flat-square)](https://laravel.com/)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)
[![Downloads](https://img.shields.io/packagist/dm/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)

Veribenim Laravel SDK هو **الحل الأكثر شمولاً لحماية البيانات وإدارة ملفات تعريف الارتباط لتطبيقات Laravel**. مصمم للمطورين، هذا SDK يجعل معالجة البيانات الشخصية قابلة للتحكم الكامل والشفافة تماماً. يوفر الامتثال الكامل مع GDPR و KVKK وجميع لوائح حماية البيانات الدولية.

**منصة إدارة الموافقة (CMP) على مستوى المؤسسات وإدارة حقوق موضوع البيانات (DSAR) بناءً على مبادئ الخصوصية بالتصميم، مدمجة مباشرة في تطبيقات Laravel.**

---

## جدول المحتويات

- [لماذا Veribenim؟](#لماذا-veribenim)
- [البدء السريع](#البدء-السريع)
- [التثبيت](#التثبيت)
- [الاستخدام](#الاستخدام)
- [معايير الأمان](#معايير-الأمان)
- [المتطلبات](#المتطلبات)
- [الترخيص](#الترخيص)

---

## لماذا Veribenim؟

### ✅ تطبيق حماية البيانات وفقاً لأفضل الممارسات

يقوم Veribenim بتطبيق المعايير التي تشير إليها سلطات حماية البيانات. المسؤولية عن معالجة البيانات تماماً تحت سيطرتك:

- **إدارة الموافقة الصريحة**: يحصل المستخدمون على تحكم دقيق عبر الفئات
- **سجلات التدقيق**: يتم تسجيل كل موافقة وسحب ومعالجة بشكل آمن
- **DSAR (حقوق موضوع البيانات)**: الوصول والحذف والتصحيح والتقييد والقابلية للنقل والاعتراض
- **حذف البيانات**: معالجة تلقائية عند سحب الموافقة

### 🌍 GDPR العالمي والمعايير الدولية

- **مواد GDPR 6، 7، 25، 32** — الخصوصية بالتصميم والموافقة الصريحة
- **إرشادات EDPB** — الامتثال الكامل مع متطلبات ملفات تعريف الارتباط والموافقة
- **متوافقة مع ISO 27001** — تشفير البيانات و TLS 1.3+ وهندسة معمارية بدون معرفة

### 🔒 أمان على مستوى المؤسسات

- **تشفير من طرف إلى طرف**: AES-256 (البيانات أثناء الراحة) و TLS 1.3+ (أثناء النقل)
- **هندسة معمارية بدون معرفة**: لا يمكن لخوادم Veribenim تخزين البيانات الشخصية
- **إخفاء الهوية عن IP**: متوافق مع GDPR، تتم إزالة أكتافات أخيرة

### ⚡ صديقة للمطورين

- **ServiceProvider مكتشف تلقائياً**: التثبيت والاستخدام على الفور
- **واجهة Facade**: استدعاءات نظيفة `Veribenim::`
- **توجيهات Blade**: التحكم على مستوى القالب
- **Middleware**: فرض الموافقة على مستوى الطريق

### 🚀 أداء عالي

- **~2KB Gzip**: بصمة JavaScript الحد الأدنى
- **CDN متعدد المناطق**: توفر 99.99%
- **التحميل غير المتزامن**: لا تأثير على سرعة الصفحة

---

## البدء السريع

```bash
composer require veribenim/laravel
php artisan vendor:publish --tag=veribenim-config
```

أضف إلى .env:
```env
VERIBENIM_TOKEN=your_api_token_here
VERIBENIM_LANG=ar
```

أضف إلى Blade:
```blade
@veribenimScript
```

---

## التثبيت

### المتطلبات الأساسية

- PHP 8.1+
- Laravel 10 أو 11 أو 12
- ext-curl أو ext-json
- حساب Veribenim

### خطوات التثبيت

**1. التثبيت عبر Composer**

```bash
composer require veribenim/laravel
```

**2. نشر التكوين**

```bash
php artisan vendor:publish --tag=veribenim-config
```

**3. تكوين .env**

```env
VERIBENIM_TOKEN=your_site_token_here
VERIBENIM_LANG=ar
VERIBENIM_DEBUG=false
```

**4. أضف البرنامج النصي إلى التخطيط الرئيسي**

```blade
<!DOCTYPE html>
<html>
<head>
    @veribenimScript
</head>
<body>
    <!-- المحتوى -->
</body>
</html>
```

**5. تشغيل الهجرات (اختياري)**

```bash
php artisan vendor:publish --tag=veribenim-migrations
php artisan migrate
```

---

## الاستخدام

### توجيهات Blade

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
    <!-- كود التسويق -->
@endIfConsented
```

### استخدام Facade

```php
use Veribenim\Laravel\VeribenimFacade as Veribenim;

// تسجيل موافقة النموذج
Veribenim::logFormConsent(
    formName: 'contact_form',
    consented: true,
    consentText: 'أوافق على شروط حماية البيانات',
    metadata: ['page_url' => url()->current()]
);

// الحصول على تفضيلات المستخدم
$preferences = Veribenim::getPreferences();

// حفظ التفضيلات
Veribenim::savePreferences([
    'necessary' => true,
    'analytics' => true,
    'marketing' => false,
    'preferences' => true
]);

// تسجيل انطباع الصفحة
Veribenim::logImpression();
```

### Middleware لحماية الطرق

```php
// الموافقة على التحليلات مطلوبة
Route::post('/track', function () {
    //...
})->middleware('veribenim.consent:analytics');

// فئات متعددة
Route::post('/profile', function () {
    //...
})->middleware('veribenim.consent:analytics,preferences');
```

### عمليات DSAR

```php
// تقديم طلب الوصول إلى البيانات
Veribenim::submitDsar(
    requestType: 'erasure',
    fullName: 'John Doe',
    email: 'john@example.com',
    description: 'حذف جميع بياناتي'
);
```

**أنواع DSAR:**

| النوع | مادة GDPR | الوصف |
|---|---|---|
| `access` | المادة 15 | حق الوصول |
| `erasure` | المادة 17 | الحق في النسيان |
| `rectification` | المادة 16 | حق التصحيح |
| `restriction` | المادة 18 | حق تقييد المعالجة |
| `portability` | المادة 20 | قابلية نقل البيانات |
| `objection` | المادة 21 | حق الاعتراض |
| `automated` | المادة 22 | الحقوق ضد القرارات الآلية |

---

## معايير الأمان

### الامتثال مع GDPR

- **المادة 6** — الأساس القانوني (الموافقة)
- **المادة 7** — شروط الموافقة
- **المادة 25** — الخصوصية بالتصميم
- **المادة 32** — تدابير الأمان
- **إرشادات EDPB** — ملفات تعريف الارتباط وتقنيات التتبع

### تشفير البيانات

| المكون | التكنولوجيا | المعيار |
|---|---|---|
| قاعدة البيانات | AES-256-GCM | FIPS 140-2 |
| النقل | TLS 1.3+ | RFC 8446 |
| المفاتيح | ECDH P-256 | NIST SP 800-56A |
| التوقيع | HMAC-SHA256 | RFC 2104 |

### إخفاء الهوية عن IP

```
IP الفعلي:      192.168.1.42
مخفي الهوية:    192.168.0.0

IPv6:           2001:db8::1
مخفي الهوية:    2001:db8::0
```

### سجلات التدقيق

يتم تسجيل جميع العمليات:
- قرارات الموافقة
- تقديمات النموذج
- طلبات DSAR
- حذف البيانات
- استدعاءات API

محفوظة لمدة **3 سنوات** على الأقل.

### شهادات الأمان

- ✓ ISO 27001
- ✓ ISO 27018
- ✓ SOC 2 Type II
- ✓ متوافق مع GDPR
- ✓ اختبارات الاختراق السنوية

---

## المتطلبات

### البرنامج

```
PHP >= 8.1
Laravel >= 10.0
ext-json
ext-curl (موصى به)
Composer
```

### توافق Laravel

| الإصدار | الدعم | PHP Min |
|---|---|---|
| 12.x | ✅ كامل | 8.2 |
| 11.x | ✅ كامل | 8.1 |
| 10.x | ✅ كامل | 8.1 |

### تكوين البيئة

**Production:**
```env
VERIBENIM_TOKEN=prod_token_xxx
VERIBENIM_LANG=ar
VERIBENIM_DEBUG=false
```

**Development:**
```env
VERIBENIM_TOKEN=dev_token_xxx
VERIBENIM_LANG=ar
VERIBENIM_DEBUG=true
```

---

## الترخيص

MIT License

---

**تم التطوير بحب بواسطة Veribenim**

Web: [https://veribenim.com](https://veribenim.com)
Email: [support@veribenim.com](mailto:support@veribenim.com)

</div>
