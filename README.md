# 🚀 FlexyBOT Client ![Version](https://img.shields.io/badge/version-1.0.0a-blue) ![License](https://img.shields.io/badge/license-LGPL--3.0-green)

[![PHP](https://img.shields.io/badge/PHP-8.0+-blue?logo=php&logoColor=white)](https://www.php.net/)
[![JavaScript](https://img.shields.io/badge/JavaScript-vanilla-yellow?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

Client-side protection layer for FlexyBOT. It guards your site using server signals and optional JavaScript signals, communicates with the FlexyBOT Hub for scoring and decisions, and offers an admin dashboard.

---

![FlexyBOT Logo](assets/img/logo.png)  

---

## 🛡️ License
Copyright (C) 2025  flexcb.Fr  

This library (FlexyBOT Client) is free software; you can redistribute it and/or
modify it under the terms of the GNU Lesser General Public License (LGPL-3.0).  
See [Full License](./LICENSE) for details.

---

## ✨ Features
- Automatic or Enhanced protection (server-only vs JS+server)
- Central scoring via Hub: APPROVE / CHALLENGE / REJECT
- Basic math challenge for intermediate scores
- Whitelist management (IPs, User-Agents)
- Local caching and resilience if Hub is unavailable
- Admin dashboard: stats, cache tools, logs, config
- Optional “Protected by FlexyBOT” badge

---

## 📁 Project Structure

```
/your-site-root/
/your-protection-folder/
/Controllers
/Models
/Views
/admin
/assets/css
/cache
/config
/logs
config.php
node.php
node\_endpoint.php
process\_js\_signals.php
update-cache.php

````
**Note:** Replace `your-protection-folder` with your preferred name (e.g., `flexybot`, `security`, `guard`).

---

## ⚙️ Requirements
- PHP 7.4+
- cURL & JSON extensions
- Session support
- Web server (Nginx/Apache)
- Internet connection (to communicate with FlexyBOT Hub)

Optional (Enhanced mode):
- OpenSSL (advanced cryptography)
- Sodium (Ed25519 signatures)

---

## 🚀 Quick Start
1. Copy folder to your site root (`flexybot/`, `security/`, etc.)
2. Include the client in your front controller:
```php
require_once __DIR__ . '/../your-protection-folder/node.php';
\FlexyBOT\Client\Controllers\SimpleProtectionController::bootstrap()->protectPage();
````

3. Go to Admin: `/your-protection-folder/admin/` (set credentials)

---

## 🖥️ Admin Dashboard

* URL: `/your-protection-folder/admin/`
* Pages: Stats, Protection, Whitelist, Logs/Cache
* Shows blocked/allowed counts, false positives/negatives, cache stats

### Key Settings

* Protection type:

  * Automatic: enhanced for modern browsers, basic for bots/CLI
  * Enhanced: always collect JS signals
* Fallback mode:

  * Allow: lenient if no Hub/score; reduces false positives
  * Block: strict if no Hub/score
* Badge: toggle visibility of the bottom-right badge

---

## 🔐 Security

* No direct use of `$_REQUEST`
* Escape HTML to avoid XSS
* Sessions + cookies for challenge state
* Hub-side authentication for registered `site_id` only

---

## 🧪 Testing

* Use curl without JS for Basic mode
* Modern browser for Enhanced mode (JS + loading page)
* Verify admin stats & Hub connectivity

---

## ⚡ Nginx Example

```
location /your-protection-folder/ {
    try_files $uri /your-protection-folder/node.php?$args;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_pass php-fpm;
}
```

Ensure your main site routes still pass through your front controller and that `node.php` can be required.

---

## 🛠️ Development Notes

* MVC structure with manual class loading (no PSR-4 autoloader)
* Server-side scoring is on the Hub; the Client only applies the decision
* JS signals anonymized before storage for GDPR compliance

---

## 🗂️ Endpoints (Client)

* `/your-protection-folder/node_endpoint.php`     – Client → Hub bridge
* `/your-protection-folder/process_js_signals.php` – Receives JS metrics
* `/your-protection-folder/update-cache.php`       – Refresh local caches
* `/your-protection-folder/node.php`               – Bootstrap protection

---

## 🗄️ Caching & Logs

* Local cache under `/your-protection-folder/cache/` to minimize Hub calls
* Admin can view and clear cache
* Logs:

  * PHP errors: project root `php_errors.log` (if configured)
  * Optional app logs under `/your-protection-folder/logs/` (rotated)
