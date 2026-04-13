# QDLS.io

> **Your links. Your brand. Your data.**

[![Live Site](https://img.shields.io/badge/Live%20Site-qdls.io-blue?style=flat-square)](https://qdls.io)
[![License](https://img.shields.io/badge/License-MIT%20%2B%20GemPixel-green?style=flat-square)](#license)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square)](#)
[![PHP](https://img.shields.io/badge/PHP-8.2-purple?style=flat-square)](#)

---

## What Is This?

QDLS.io is a **full-featured link management and URL shortening platform** — custom-built on top of the GemPixel Premium URL Shortener framework and tailored to deliver a branded, professional experience.

This is not a generic link shortener. Every link you create through QDLS.io is tracked, branded, and backed by real analytics. Whether you're a marketer managing campaigns, a creator sharing content, or a business protecting its brand — QDLS.io gives you total control over your links.

> **Live at:** [https://qdls.io](https://qdls.io)

---

## Why QDLS.io?

The internet is full of link shorteners. Most of them own your data, inject their branding, and give you nothing back.

QDLS.io is different:

- **Your brand, not ours** — Custom domains, branded links, white-label experience
- **Real analytics** — Click tracking, geo data, device breakdowns, UTM support
- **Built-in monetization** — Subscription tiers, Stripe, PayPal, and Mollie payment integration
- **Bio pages** — A full link-in-bio tool built right in
- **QR codes** — Auto-generated for every link, ready for print and digital
- **Open & extensible** — Built on open source, customized for real use cases

---

## Features

- 🔗 URL shortening with custom slugs and branded domains
- 📊 Deep analytics — clicks, geography, devices, referrers, UTM tracking
- 🪪 Bio pages — link-in-bio landing pages for creators and brands
- 📱 QR code generation for every link
- 👤 User management with roles, teams, and subscription plans
- 💳 Multi-gateway payments — Stripe, PayPal, Mollie
- 🔒 Two-factor authentication (2FA) via Google Authenticator
- 🌍 GeoIP-based link routing and country blocking
- 🔌 Plugin system — extend with custom functionality
- 🎨 Multi-theme support (Default + The23)
- 🤖 Bot detection and crawler filtering
- 📧 Transactional email via PHPMailer
- 🛡️ CAPTCHA (Altcha) on public forms

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | PHP 8.2 |
| Framework | GemFramework (MVC — custom) |
| Routing | nikic/fast-route |
| Database | MySQL |
| Payments | Stripe, PayPal, Mollie |
| Email | PHPMailer |
| QR Codes | endroid/qr-code |
| Caching | phpFastCache |
| Logging | Monolog |
| Encryption | defuse/php-encryption |
| GeoIP | MaxMind DB Reader |
| 2FA | Google Authenticator |
| Hosting | Apache on BlueHost/cPanel |
| Dependencies | Composer |

---

## Project Structure

```
qdls.io/
├── index.php                 # Bootstrap — loads config, initialises framework
├── config.php                # DB credentials & tokens (secrets — never commit)
├── app/
│   ├── config/               # App configuration
│   ├── controllers/          # Route handlers
│   │   ├── admin/            # Admin panel controllers
│   │   ├── api/              # API endpoints
│   │   └── user/             # User-facing controllers
│   ├── helpers/
│   │   ├── payments/         # Stripe, PayPal, Mollie integrations
│   │   └── qr/               # QR code generation
│   ├── middleware/           # Request middleware
│   ├── models/               # Database models
│   └── traits/               # Shared traits
├── core/                     # GemFramework core (routing, support, functions)
├── public/                   # Webroot — Apache/nginx document root
│   ├── content/              # User uploads (avatars, QR, profiles, images)
│   └── static/               # Frontend assets (CSS, JS, fonts)
│       ├── backend/          # Admin panel assets
│       └── frontend/         # Public-facing assets
├── storage/
│   ├── cache/                # Per-domain cache files
│   ├── languages/            # Translation files
│   ├── logs/                 # Application logs
│   ├── plugins/              # Installed plugins
│   └── themes/               # Theme templates (default, the23)
├── vendor/                   # Composer dependencies (not committed)
├── 01_md_plan_files/         # Internal build docs & plans
├── .gitattributes
├── .gitignore
├── .htaccess                 # Rewrites all requests to public/
├── composer.json
├── nginx.conf                # Sample nginx config
├── LICENSE
└── README.md
```

---

## Getting Started

### Prerequisites

- PHP 8.2+
- MySQL 5.7+ / MariaDB 10.3+
- Composer
- Apache with `mod_rewrite` (or nginx)

### Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/{GITHUB_USERNAME}/qdls.io.git
   ```
2. Install dependencies:
   ```bash
   composer install
   ```
3. Copy config and add credentials:
   ```bash
   cp config.sample.php config.php
   ```
   Edit `config.php` with your database credentials, `AuthToken`, `EncryptionToken`, and `PublicToken`.
4. Import the database schema via phpMyAdmin or CLI.
5. Point your web server document root to the `public/` directory.
6. Visit your domain — the installer will guide first-time setup.

### Deployment

See [`01_md_plan_files/DEPLOY.md`](01_md_plan_files/DEPLOY.md) for the full deployment checklist.

---

## Configuration

`config.php` is the heart of the application. Key constants:

| Constant | Purpose |
|----------|---------|
| `DBhost`, `DBname`, `DBuser`, `DBpassword` | Database connection |
| `AuthToken` | Session authentication secret |
| `EncryptionToken` | Data encryption key |
| `PublicToken` | Public-facing token |
| `FORCEURL` | Canonical URL enforcement |
| `CACHE` | Caching driver (file, redis, etc.) |

> ⚠️ **Never commit `config.php`.** It contains secrets. Use `config.sample.php` as a reference.

---

## Internal Documentation

All build logs, plans, and technical docs live in [`01_md_plan_files/`](01_md_plan_files/):

| File | Purpose |
|------|---------|
| `AGENTS.md` | Technical reference for AI-assisted development (Warp) |
| `BUILD_qdls_master.md` | Full build history and go-live checklist |
| `CHANGELOG.md` | Every meaningful change, dated and categorized |
| `DEPLOY.md` | Step-by-step deployment checklist |
| `ARCHITECTURE.md` | Architecture decisions and system design |
| `ROADMAP.md` | Where QDLS.io is going |
| `SECURITY.md` | Security policy and practices |
| `ongoing_plan.md` | Current sprint status |

---

## Social

- **Website:** [https://qdls.io](https://qdls.io)

---

## Contributing

This is an open source customization project. Contributions and forks are welcome under the MIT license terms covering the QDLS.io customizations. See [LICENSE](LICENSE) for details.

---

## License

QDLS.io customizations are MIT licensed. © 2026 Juan Jose / QDLS.io.
Built on GemPixel Premium URL Shortener — see [LICENSE](LICENSE) for full details.

---

*Short links. Big impact. Total control.*
