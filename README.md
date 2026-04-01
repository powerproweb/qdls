# QDLS.io

URL shortener platform built on GemPixel Premium URL Shortener / GemFramework.

Features: link shortening, custom branded links, bio pages, QR code generation, analytics, user management, subscription tiers, and multi-payment-gateway integration (Stripe, PayPal, Mollie).

## Setup

1. Copy `config.sample.php` to `config.php` and fill in database credentials and security tokens
2. Run `composer install` to install PHP dependencies
3. Point your web server document root to the `public/` directory (or use the root `.htaccess` rewrite)

## Tech Stack

- PHP 8.2 on GemFramework (MVC)
- MySQL database
- Composer for dependency management
- Apache (`.htaccess`) or nginx (`nginx.conf`)
- Hosted on BlueHost/cPanel

## Deployment

Upload files to the server. Run `composer install` if dependencies are missing. Ensure `config.php` exists with valid credentials.
