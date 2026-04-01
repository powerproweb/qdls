# AGENTS.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

QDLS.io is a URL shortener platform built on the GemPixel Premium URL Shortener / GemFramework. It provides link shortening, custom branded links, bio pages, QR code generation, analytics, and user management with subscription tiers and payment integration.

## Architecture

### Application Structure (MVC-style PHP)

```
app/
  config/         — Application configuration
  controllers/    — Route handlers
    admin/        — Admin panel controllers
    api/          — API endpoints
    user/         — User-facing controllers
  helpers/        — Utility classes
    payments/     — Payment gateway integrations
    qr/           — QR code generation
  middleware/     — Request middleware
  models/         — Database models
  traits/         — Shared traits

core/             — GemFramework core (routing, support, functions)

public/           — Webroot (document root for Apache/nginx)
  content/        — User-uploaded files (avatars, blog, images, QR codes, profiles)
  static/         — Frontend assets (CSS, JS, fonts, images)
    backend/      — Admin panel assets
    frontend/     — Public-facing assets

storage/
  app/tmp/        — Temporary files
  cache/          — Cached data (per-domain)
  languages/      — Translation files
  logs/           — Application logs
  plugins/        — Installed plugins
  themes/         — Theme templates (default, the23)
```

### Routing & Entry Point
- `index.php` (root) — Bootstraps the framework, loads `config.php`
- `.htaccess` (root) — Rewrites all requests to `public/` subdirectory
- Routing via nikic/fast-route

### Key Dependencies (Composer)
- `stripe/stripe-php` — Stripe payment integration
- `paypal/rest-api-sdk-php` — PayPal payments
- `mollie/mollie-api-php` — Mollie payments
- `endroid/qr-code` — QR code generation
- `phpmailer/phpmailer` — Email delivery
- `phpfastcache/phpfastcache` — Application caching
- `monolog/monolog` — Logging
- `defuse/php-encryption` — Encryption utilities
- `maxmind-db/reader` — GeoIP lookups
- `sonata-project/google-authenticator` — 2FA support
- `abraham/twitteroauth` — Twitter OAuth
- `altcha-org/altcha` — CAPTCHA alternative
- `jaybizzle/crawler-detect` — Bot detection
- `setasign/fpdf` — PDF generation
- `nikic/fast-route` — URL routing

### Database
- MySQL (configured via `config.php` — credentials in `DBhost`, `DBname`, `DBuser`, `DBpassword`)
- Table prefix configurable via `DBprefix`

### Plugins
Plugins live in `storage/plugins/` and include: allowemail, banip, biodirectory, blockcountry, blockemail, blogfeed, coinpayments, helloworld, iyzico, lemonsqueezy, mercado, midtrans, noindex, urlhaus, userwebhook, verifyemail

### Themes
- `storage/themes/default/` — Default theme
- `storage/themes/the23/` — Alternate theme

## Hosting & Deployment
- Apache on shared hosting (BlueHost/cPanel) with PHP 8.2
- `.htaccess` rewrites all requests to `public/` (the actual document root)
- `nginx.conf` sample provided for nginx deployments
- `config.php` holds database credentials and security tokens — **never commit this file** (use `config.sample.php` as template)
- Dependencies managed via Composer (`composer install`)

## Conventions
- GemFramework MVC patterns — controllers, models, helpers, middleware, traits
- Configuration via PHP `define()` constants in `config.php`
- Security tokens: `AuthToken`, `EncryptionToken`, `PublicToken` — changing these will break existing sessions and logins
- Caching: configurable via `CACHE` constant; cache files stored per-domain in `storage/cache/`
- Multi-domain support: `FORCEURL` constant controls whether the app enforces a single canonical URL

## Important Notes
- **`config.php` contains database credentials and security tokens — never commit it.** Use `config.sample.php` as reference.
- The `vendor/` directory is managed by Composer — do not modify files inside it
- `storage/plugins/` and `storage/cache/` are runtime directories — excluded from version control
- User-uploaded content in `public/content/` should not be committed to git
- This is a licensed commercial product (GemPixel) — the core framework code has distribution restrictions
