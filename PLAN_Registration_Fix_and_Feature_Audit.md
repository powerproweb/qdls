# QDLS.io Registration Fix & Feature Audit

## Problem
User registration is currently broken — visitors cannot sign up for an account. Based on code analysis of the GemPixel Premium URL Shortener (PHP 8.2 / GemFramework MVC / MySQL), there are **four settings** that gate registration plus **two prerequisites** that must be in place.

## How Registration Works
The registration flow in `UsersController.php (289)` has this gate:
```php
if(!config("user") || config("private") || config("maintenance"))
    return redirect->to(route('login'))->with("danger", "We are not accepting users at this time.");
```
Then `registerValidate()` at line 318-320 adds a 4th check:
```php
if(!config('system_registration'))
    return redirect->to(route('login'))->with("danger", "Please use a social media platform to login or register.");
```
The register template (`storage/themes/default/auth/register.php`) also conditionally hides the entire form if these settings are wrong.

## Full Feature Audit

### Admin Side (17 modules)
- **Dashboard** — overview stats, search, cron jobs, PHP info, updates
- **Statistics** — links, users, clicks, geo map, memberships, subscriptions, payments
- **Plans** — create/edit/toggle subscription plans, sync with payment processors, free plan flag
- **Subscriptions & Payments** — manage active subs, invoices, mark-as actions
- **Coupons & Vouchers** — discount codes, bulk generation
- **Tax** — tax rate rules by region
- **Links** — view/edit/delete/disable/approve, pending, reported, bad, archived, expired, anonymous, import
- **Users** — CRUD, ban, verify email, verify identity, import, login-as, testimonials, change plan, teams, login logs, activity
- **Roles & Permissions** — custom admin roles
- **Bio Pages & Themes** — manage all bios, custom themes
- **QR Codes & Templates** — manage all QRs, templates
- **CMS** — pages (including terms page), blog + categories, FAQ + categories
- **Domains** — add/approve/disable branded domains
- **Affiliates** — referrals, withdrawal requests, payment history, settings
- **Ads** — ad placements, adblock detection
- **Themes** — activate, upload, editor, custom CSS, menu builder
- **Settings** — 14 sub-pages: General, Application, Link, QR, Bio, Advanced, Upload, Theme, Security, Membership, Payments, Users, Mail, Integrations + OAuth + Languages + Plugins + Email Templates + Notifications + Tools

### Public Side (registered users — 15 modules)
- **Dashboard** — overview, recent links, click stats
- **Links** — shorten, edit, delete, archive, bulk ops, campaigns, A/B testing, export/import
- **Campaigns** — group links, campaign-level stats
- **Splash Pages** — custom interstitial pages
- **CTA Overlays** — message/contact/newsletter/image/poll overlays
- **Tracking Pixels** — Facebook, Google, GTM, LinkedIn, Twitter, Quora, Pinterest, etc.
- **Custom Domains** — branded short domains
- **Teams** — invite members, assign permissions, switch accounts
- **QR Codes** — create/edit/bulk, multiple types, downloadable
- **Bio Pages** — link-in-bio with blocks, themes, custom settings
- **Statistics** — clicks, countries, platforms, browsers, languages, referrers
- **Channels** — organize links/bio/QR into channels
- **Affiliate** — referral dashboard, withdrawal requests
- **Billing** — view plan, manage subscription, invoices, cancel
- **Account** — profile, 2FA, API keys, security, integrations, verification

### Public Side (unauthenticated)
- Homepage with URL shortener (if not private)
- Pricing page
- Blog, FAQ, Help Center
- Contact & Report pages
- Developer API docs
- Login (email + social: Facebook, Google, Twitter)
- Registration, Password Reset, 2FA, SSO, Email Verification

## Settings to Fix (in order of priority)

### 1. CRITICAL — Admin > Settings > Application Settings
Path: `/admin/settings/app`
- **Maintenance Mode** → must be **OFF** (unchecked). When ON, blocks all non-admin access including registration.
- **Private Service** → must be **OFF** (unchecked). When ON, prevents all registration and public shortening. The `CheckPrivate` middleware redirects everyone.

### 2. CRITICAL — Admin > Settings > Users Settings
Path: `/admin/settings/users`
- **User Registration** → must be **ON** (checked). This is the `user` config key — the primary gate for registration.
- **System Registration** → must be **ON** (checked). This is the `system_registration` config key — when OFF, the email/password form is hidden and only social logins work (which likely aren't configured either).
- **User Activation** → decide ON or OFF. If ON, activation emails are sent (requires working mail — see step 5). If OFF, users are active immediately after registering.

### 3. CRITICAL — Admin > Plans
Path: `/admin/plans`
A **Free plan** must exist with `free = 1` and `status = 1`. The registration code at `UsersController.php (379)` does:
```php
if($plan = DB::plans()->where('free', 1)->where('status', 1)->first())
```
Without this, new users get no plan assignment. Verify a free plan exists and is enabled. If not, create one via Admin > Plans > New Plan with "Free" checked.

### 4. IMPORTANT — Admin > Settings > Security (Captcha)
Path: `/admin/settings/security`
The registration form calls `\Helpers\Captcha::display('register')` and the route uses `ValidateCaptcha` middleware. If captcha is enabled but keys are wrong or missing, registration submissions will silently fail.
- If using reCaptcha/hCaptcha/Turnstile: verify Public Key + Private Key are correct.
- If using Altcha (option 6): no keys needed but requires PHP 8.2+.
- Safest short-term fix: set Captcha to **None** (0) until keys are properly configured.

### 5. IMPORTANT — Admin > Settings > Mail Settings
Path: `/admin/settings/mail`
If **User Activation** is ON (step 2), email must work for activation links. Configure one of:
- SMTP (host, port, security, user, pass)
- Mailgun, Sendgrid, Postmark, or Mailchimp API
Also set the **From Email** field. Test with the "Send Test Email" button on that page.

### 6. RECOMMENDED — Terms of Service Page
The register form displays a terms checkbox. The code looks for: `DB::page()->where('category', 'terms')`. If no terms page exists, the form falls back to a generic "I agree to the terms and conditions" label. Create a proper Terms page via Admin > Pages > New Page with category set to `terms`.

## Verification Steps
After making the changes above:
1. Open an incognito/private browser window
2. Navigate to `https://qdls.io/user/register`
3. Confirm the registration form is visible (not redirected to login)
4. Fill in a test username, email, password, confirm password, check terms
5. Submit — should either create account directly or send activation email
6. If activation email is required, check it arrives and the link works
