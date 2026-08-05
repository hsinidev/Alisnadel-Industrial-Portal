# Alisnad Industrial Portal (`alisnadel.hsini.dev`)

Alisnad is a multilingual (FR / EN / AR / ES / IT) industrial company website for **شركة الإسناد للصناعة والإصلاح** (Alisnad Sinaa Wa Islah), a Moroccan marine & industrial repair company. The platform features a B2B lead capture system, a multilingual project portfolio gallery, a full CMS for pages and blog articles, and a secure admin panel with optional 2FA login.

---

## 🔑 Access URLs & Credentials

### 1. Admin Control Panel

- **URL:** https://alisnadel.hsini.dev/fr/admin-login
  *(Replace `fr` with any supported language: `en`, `ar`, `es`, `it`)*
- **Credentials:**
  - **Username:** `admin`
  - **Password:** `admin123`
- **After Login:** Redirects to `/{lang}/admin-dashboard`
- **Purpose:** Managing the project gallery, blog articles, leads/inquiries, static CMS pages, site settings (colors, contact info, domain), and optional 2FA configuration.

> **Note:** 2FA is **disabled** by default (`admin_2fa_enabled = 0` in settings table). If enabled, OTP is sent to `hsini.web@gmail.com`.

---

## 🌐 Public Website

- **URL:** https://alisnadel.hsini.dev
- **Default Language:** French (`fr`)
- **Homepage:** https://alisnadel.hsini.dev/fr/accueil
- **Supported Languages:** `fr`, `en`, `ar`, `es`, `it`

---

## 🛠️ Technology Stack & Architecture

- **Backend Core:** Vanilla PHP (Custom MVC, PSR-4 autoloading via `App\` namespace)
- **Database:** MySQL / MariaDB — Database name: `alisnad`
- **Multilingual Engine:** Language prefix routing (`/{lang}/route`) with per-language DB columns (`_fr`, `_en`, `_ar`, `_es`, `_it`)
- **Email Service:** Gmail SMTP via App Password (PHPMailer)
- **Security:** Rate-limited login (5 attempts / 5 min), Audit log trail (`audit_logs` table), optional email OTP 2FA

---

## 💾 Environment Configuration (`.env`)

```env
DB_HOST=127.0.0.1
DB_PORT=3306
DB_NAME=alisnad
DB_USER=hsini
DB_PASS="1Wqy2xPtPRA9gPodUPK32Q8SY"
APP_NAME="شركة الإسناد للصناعة والإصلاح"
APP_DOMAIN=alisnad.ma
SUPPORTED_LANGUAGES=fr,en,ar,es,it
DEFAULT_LANGUAGE=fr

# SMTP Configuration
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=hsini.web@gmail.com
SMTP_PASS="revo ukwp yjfz naaa"
SMTP_FROM_EMAIL=hsini.web@gmail.com
SMTP_FROM_NAME="Alisnad Industrial"

# Webhooks & Integrations
CRM_WEBHOOK_URL=https://hooks.zapier.com/hooks/catch/12345/abcde
```

---

## 📁 VPS Deployment Directory Layout

- 📂 **Web Root (`public_html/`)** -> `/home/hsini/domains/alisnadel.hsini.dev/public_html/`
  - `index.php` (Main router and language dispatcher)
  - `.htaccess` (Apache rewrite rules for clean URLs)
  - `assets/` (CSS, JS, images)
  - `uploads/` (Project photos and blueprint files)
  - `favicon.ico`, `HSINI.jfif`, `robots.txt`, `sitemap.xml`
- 📁 **Core Logic (one level above `public_html/`)** -> `/home/hsini/domains/alisnadel.hsini.dev/`
  - `app/` (MVC: `controllers/`, `views/`, `lang/`, `services/`, `security/`, `config/`, `storage/`)
  - `vendor/` (Composer dependencies)
  - `.env` (Environment variables)
  - `schema.sql` (Full MySQL database schema)
  - `composer.json` & `composer.lock`

---

## ⚙️ Initial VPS Provisioning & Database Setup

1. Extract deployment files into `/home/hsini/domains/alisnadel.hsini.dev/`.
2. Import the database schema:
   ```bash
   mysql -u hsini -p"1Wqy2xPtPRA9gPodUPK32Q8SY" alisnad < schema.sql
   ```
3. Set folder write permissions:
   ```bash
   chmod -R 775 /home/hsini/domains/alisnadel.hsini.dev/app/storage
   chmod -R 775 /home/hsini/domains/alisnadel.hsini.dev/public_html/uploads
   ```
4. Verify the site: https://alisnadel.hsini.dev/fr/accueil

---

## 🗄️ Key Database Tables

| Table          | Purpose                                                        |
|----------------|----------------------------------------------------------------|
| `admins`       | Admin accounts (username + bcrypt password hash)               |
| `settings`     | Key-value site config (colors, email, domain, 2FA)             |
| `leads`        | B2B quote/contact form submissions                             |
| `projects`     | Industrial project portfolio entries (5 languages)             |
| `project_media`| Photos and blueprints linked to projects                       |
| `pages`        | CMS static pages (multilingual content)                        |
| `blog_posts`   | Blog articles (multilingual)                                   |
| `admin_2fa`    | Temporary OTP codes for 2FA login flow                         |
| `audit_logs`   | Admin action audit trail with IP & user agent                  |
