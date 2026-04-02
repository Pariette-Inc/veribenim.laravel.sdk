# Veribenim Laravel SDK

**Plataforma de Gestión de Consentimiento Conforme con RGPD y KVKK**

[![Packagist Version](https://img.shields.io/packagist/v/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)
[![PHP Version](https://img.shields.io/badge/php-%3E%3D%208.1-blue?style=flat-square)](https://www.php.net/)
[![Laravel Versions](https://img.shields.io/badge/laravel-10%20%7C%2011%20%7C%2012-red?style=flat-square)](https://laravel.com/)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)
[![Downloads](https://img.shields.io/packagist/dm/veribenim/laravel.svg?style=flat-square)](https://packagist.org/packages/veribenim/laravel)

Veribenim Laravel SDK es **la solución más completa de gestión de privacidad de datos y consentimiento de cookies para Laravel**. Diseñado para desarrolladores, este SDK hace que el procesamiento de datos personales sea completamente controlable y transparente. Cumplimiento total con RGPD, KVKK y todas las regulaciones internacionales de protección de datos.

**Plataforma de Gestión de Consentimiento (CMP) de nivel empresarial y gestión de Derechos de Sujetos de Datos (DSAR) basada en principios de Privacidad por Diseño, integrada directamente en aplicaciones Laravel.**

---

## Tabla de contenidos

- [¿Por qué Veribenim?](#por-qué-veribenim)
- [Inicio rápido](#inicio-rápido)
- [Instalación](#instalación)
- [Uso](#uso)
- [Estándares de seguridad](#estándares-de-seguridad)
- [Requisitos](#requisitos)
- [Licencia](#licencia)

---

## ¿Por qué Veribenim?

### ✅ Implementación de protección de datos según mejores prácticas

Veribenim implementa estándares referenciados por autoridades de protección de datos. La responsabilidad del procesamiento de datos está completamente bajo su control:

- **Gestión de consentimiento explícito**: Los usuarios tienen control granular sobre categorías
- **Pistas de auditoría**: Cada consentimiento, retiro y procesamiento se registra de forma segura
- **DSAR (Derechos de sujetos)**: Acceso, eliminación, rectificación, restricción, portabilidad, objeción
- **Eliminación de datos**: Procesamiento automático al retirar consentimiento

### 🌍 RGPD global y estándares internacionales

- **Artículos 6, 7, 25, 32 RGPD** — Privacidad por diseño, consentimiento explícito
- **Orientaciones del AEPD** — Conformidad total con requisitos de cookies y consentimiento
- **Conforme a ISO 27001** — Cifrado de datos, TLS 1.3+, arquitectura zero-knowledge

### 🔒 Seguridad de nivel empresarial

- **Cifrado de extremo a extremo**: AES-256 (datos en reposo), TLS 1.3+ (en tránsito)
- **Arquitectura zero-knowledge**: Los servidores de Veribenim no pueden almacenar datos personales
- **Anonimización de IP**: Conforme a RGPD, últimos octetos eliminados

### ⚡ Amigable para desarrolladores

- **ServiceProvider autodescubierto**: Instala y usa inmediatamente
- **API Facade**: Llamadas `Veribenim::` limpias y claras
- **Directivas Blade**: Control a nivel de plantilla
- **Middleware**: Aplicación de consentimiento a nivel de ruta

### 🚀 Alto rendimiento

- **~2KB Gzip**: Huella JavaScript mínima
- **CDN multi-región**: Disponibilidad 99,99%
- **Carga asincrónica**: Sin impacto en la velocidad de página

---

## Inicio rápido

```bash
composer require veribenim/laravel
php artisan vendor:publish --tag=veribenim-config
```

Agregar a .env:
```env
VERIBENIM_TOKEN=your_api_token_here
VERIBENIM_LANG=es
```

Agregar a Blade:
```blade
@veribenimScript
```

---

## Instalación

### Requisitos previos

- PHP 8.1+
- Laravel 10, 11 o 12
- ext-curl o ext-json
- Cuenta Veribenim

### Pasos de instalación

**1. Instalar vía Composer**

```bash
composer require veribenim/laravel
```

**2. Publicar configuración**

```bash
php artisan vendor:publish --tag=veribenim-config
```

**3. Configurar .env**

```env
VERIBENIM_TOKEN=your_site_token_here
VERIBENIM_LANG=es
VERIBENIM_DEBUG=false
```

**4. Agregar script al diseño principal**

```blade
<!DOCTYPE html>
<html>
<head>
    @veribenimScript
</head>
<body>
    <!-- Contenido -->
</body>
</html>
```

**5. Ejecutar migraciones (opcional)**

```bash
php artisan vendor:publish --tag=veribenim-migrations
php artisan migrate
```

---

## Uso

### Directivas Blade

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
    <!-- Código de marketing -->
@endIfConsented
```

### Uso de Facade

```php
use Veribenim\Laravel\VeribenimFacade as Veribenim;

// Registrar consentimiento del formulario
Veribenim::logFormConsent(
    formName: 'contact_form',
    consented: true,
    consentText: 'Acepto las condiciones de protección de datos',
    metadata: ['page_url' => url()->current()]
);

// Obtener preferencias del usuario
$preferences = Veribenim::getPreferences();

// Guardar preferencias
Veribenim::savePreferences([
    'necessary' => true,
    'analytics' => true,
    'marketing' => false,
    'preferences' => true
]);

// Registrar impresión de página
Veribenim::logImpression();
```

### Middleware para protección de rutas

```php
// Se requiere consentimiento analítico
Route::post('/track', function () {
    //...
})->middleware('veribenim.consent:analytics');

// Categorías múltiples
Route::post('/profile', function () {
    //...
})->middleware('veribenim.consent:analytics,preferences');
```

### Operaciones DSAR

```php
// Enviar solicitud de acceso a datos
Veribenim::submitDsar(
    requestType: 'erasure',
    fullName: 'John Doe',
    email: 'john@example.com',
    description: 'Eliminar todos mis datos'
);
```

**Tipos DSAR:**

| Tipo | Artículo RGPD | Descripción |
|---|---|---|
| `access` | Artículo 15 | Derecho de acceso |
| `erasure` | Artículo 17 | Derecho al olvido |
| `rectification` | Artículo 16 | Derecho a la rectificación |
| `restriction` | Artículo 18 | Derecho a limitar el procesamiento |
| `portability` | Artículo 20 | Portabilidad de datos |
| `objection` | Artículo 21 | Derecho de objeción |
| `automated` | Artículo 22 | Derechos contra decisiones automatizadas |

---

## Estándares de seguridad

### Cumplimiento RGPD

- **Artículo 6** — Base legal (Consentimiento)
- **Artículo 7** — Condiciones del consentimiento
- **Artículo 25** — Privacidad por diseño
- **Artículo 32** — Medidas de seguridad
- **Directrices AEPD** — Cookies y tecnologías de seguimiento

### Cifrado de datos

| Componente | Tecnología | Estándar |
|---|---|---|
| Base de datos | AES-256-GCM | FIPS 140-2 |
| Transmisión | TLS 1.3+ | RFC 8446 |
| Claves | ECDH P-256 | NIST SP 800-56A |
| Firma | HMAC-SHA256 | RFC 2104 |

### Anonimización de IP

```
IP real:        192.168.1.42
Anonimizada:    192.168.0.0

IPv6:           2001:db8::1
Anonimizada:    2001:db8::0
```

### Registros de auditoría

Todas las operaciones se registran:
- Decisiones de consentimiento
- Envíos de formularios
- Solicitudes DSAR
- Eliminación de datos
- Llamadas API

Retenidas mínimo **3 años**.

### Certificaciones de seguridad

- ✓ ISO 27001
- ✓ ISO 27018
- ✓ SOC 2 Type II
- ✓ Conforme a RGPD
- ✓ Pruebas de penetración anuales

---

## Requisitos

### Software

```
PHP >= 8.1
Laravel >= 10.0
ext-json
ext-curl (recomendado)
Composer
```

### Compatibilidad Laravel

| Versión | Soporte | PHP Min |
|---|---|---|
| 12.x | ✅ Completo | 8.2 |
| 11.x | ✅ Completo | 8.1 |
| 10.x | ✅ Completo | 8.1 |

### Configuración de entorno

**Production:**
```env
VERIBENIM_TOKEN=prod_token_xxx
VERIBENIM_LANG=es
VERIBENIM_DEBUG=false
```

**Development:**
```env
VERIBENIM_TOKEN=dev_token_xxx
VERIBENIM_LANG=es
VERIBENIM_DEBUG=true
```

---

## Licencia

MIT License

---

**Desarrollado con pasión por Veribenim**

Web: [https://veribenim.com](https://veribenim.com)
Email: [support@veribenim.com](mailto:support@veribenim.com)
