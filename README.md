\# Rentpoa



Rentpoa is an open-source property management platform for boarding houses (kos), rental homes, and apartments. It covers the full rental lifecycle: listing properties and units, onboarding tenants, managing leases and rent invoices, recording payments, handling maintenance and inspections, tracking expenses and utilities, and sending reminders. It also includes a public listing portal for prospective tenants and a self-service portal for existing tenants.



Licensed under the \[Apache License 2.0](LICENSE).



\---



\## Features



\### Back office (staff)



\- \*\*Dashboard\*\*: overview, rent collection status, and financial summary, each gated by permission.

\- \*\*Properties\*\*: property records with photo galleries, documents, amenities, publication status, and per-property rental mode (whole-property pricing or unit inventory).

\- \*\*Unit types and units\*\*: unit types with their own gallery, publication status, and rates; individual units with rates, status, and lease history.

\- \*\*Utilities\*\*: meters and readings per unit, with corrections and audit trail.

\- \*\*Tenants\*\*: tenant profiles, identity documents, unit assignment, invitations, and portal access control.

\- \*\*Leases\*\*: creation, renewal, move-in, move-out, deposit settlement, rent schedules, and per-lease document and inspection history.

\- \*\*Invoices and payments\*\*: rent invoices with print and PDF download, payment recording, payment proof uploads, verification, and payment attempt rechecks.

\- \*\*Payment gateways\*\*: configurable gateway integration with signed payment links for tenants.

\- \*\*Maintenance tickets\*\*: reporting, assignment to staff, status updates, and history per unit.

\- \*\*Inspections\*\*: reusable inspection templates, property, unit, and lease inspections, checklist items, photo evidence, and completion locking.

\- \*\*Expenses\*\*: operating expenses, recurring expenses (pause and resume), and receipts.

\- \*\*Reminders\*\*: scheduled rent reminders before and after due dates, with configurable channels and message templates.

\- \*\*Data transfer\*\*: CSV templates, import previews, committed imports, and exports for properties, units, tenants, leases, expenses, and unit rates.

\- \*\*Users and roles\*\*: staff invitations, custom roles, role cloning, and fine-grained permissions.

\- \*\*Settings\*\*: general and branding options, currency and locale, mail, WhatsApp, reminders, payment gateway, public portal SEO, and plugin management.



\### Public portal



\- Public listing pages for properties and unit types, with galleries and SEO metadata.

\- Applications from prospective tenants, tracked through review and conversion to a lease.



\### Tenant portal



\- Dashboard, lease details, invoices and payment history, and online payment.

\- Notifications and read tracking.

\- Maintenance ticket submission and status tracking.



\### Platform



\- \*\*Plugin system\*\*: plugins register settings pages, dashboard sub-pages, sidebar items, permissions, and notification drivers through the `openkos/platform` SDK.

\- \*\*Built-in plugins\*\*: Mail and WhatsApp notification drivers.

\- \*\*Localization\*\*: multi-language interface and multi-currency support.

\- \*\*Appearance\*\*: light and dark themes.



\---



\## Technology stack



| Layer | Technology |

|---|---|

| Backend | PHP 8.3+, Laravel 13 |

| Frontend | React, TypeScript, Inertia.js, Tailwind CSS, shadcn/ui |

| Routing helpers | Laravel Wayfinder (typed route functions for TypeScript) |

| Authentication | Laravel Fortify (including two-factor authentication) |

| Authorization | spatie/laravel-permission |

| Query building | spatie/laravel-query-builder |

| PDF generation | dompdf |

| Platform SDK | openkos/platform |

| Testing | Pest 4 |

| Code style | Laravel Pint |

| Database | MySQL (or MariaDB), SQLite supported for development |



\---



\## Requirements



\- PHP 8.3 or newer, with the extensions Laravel requires (`pdo\_mysql`, `mbstring`, `openssl`, `tokenizer`, `xml`, `ctype`, `json`, `fileinfo`, `gd` or `imagick` for image handling)

\- Composer 2

\- Node.js 20 or newer, with npm

\- MySQL 8 or MariaDB 10.6 or newer (or SQLite for local development)

\- A local stack such as XAMPP, Laragon, or Docker



\---



\## Installation



\### 1. Clone the repository



```bash

git clone https://github.com/<owner>/rentpoa.git

cd rentpoa

```



\### 2. Install dependencies



```bash

composer install

npm install

```



Use `composer install` rather than `composer update` so the exact versions in `composer.lock` are installed.



\### 3. Configure the environment



```bash

cp .env.example .env

php artisan key:generate

```



On Windows PowerShell, use `copy` instead of `cp`.



Set the database connection in `.env`:



```env

APP\_NAME=Rentpoa

APP\_URL=http://localhost:8000



DB\_CONNECTION=mysql

DB\_HOST=127.0.0.1

DB\_PORT=3306

DB\_DATABASE=rentpoa

DB\_USERNAME=root

DB\_PASSWORD=

```



Create the database before migrating. For example, in phpMyAdmin, create a database named `rentpoa` with the `utf8mb4\_unicode\_ci` collation.



\### 4. Run migrations and seeders



```bash

php artisan migrate

php artisan db:seed

```



If the project provides an install command, use it instead of seeding manually:



```bash

php artisan list | grep -i install

```



\### 5. Build the frontend



```bash

npm run build

```



For development, run `npm run dev` in a separate terminal, or use the combined development command:



```bash

composer run dev

```



This starts the web server, queue listener, log viewer, and Vite together.



\### 6. Open the application



Visit `http://localhost:8000`. Sign in with the owner account created by the seeder or the install command, then change its password immediately.



\---



\## Configuration



Common settings in `.env`:



| Variable | Purpose |

|---|---|

| `APP\_URL` | Public base URL, used in emails and signed links |

| `DB\_\*` | Database connection |

| `MAIL\_\*` | Outgoing mail (or configure it later under Settings → Mail) |

| `RENTPOA\_PLUGIN\_PATH` | Storage path for runtime-installed plugins |

| `QUEUE\_CONNECTION` | Queue backend for reminders and background jobs (`database` by default) |



Most operational settings, including mail, WhatsApp, reminders, payment gateway, currency, and branding, are managed in the application under \*\*Settings\*\* and stored in the `settings` table. Keep secrets out of version control.



\---



\## Permissions and roles



Access is controlled by permissions in the format `module.action`, for example `properties.view`, `leases.create`, or `payments.verify`. Each permission maps to a route or an action in the UI.



Default roles:



| Role | Summary |

|---|---|

| Owner | Full access, including roles and settings |

| Manager | Day-to-day operations for assigned properties |

| Accountant | Payments, invoices, expenses, and financial reports |

| Caretaker | Maintenance tickets and inspections on site |

| Viewer | Read-only access to properties, units, tenants, and leases |

| Tenant | Access to the tenant portal only |



Owners can create custom roles and assign permissions from \*\*Settings → Roles\*\*.



\---



\## Project structure



```

app/

&#x20; Actions/            Single-purpose business operations (e.g. reminders, settings updates)

&#x20; Business/           Domain logic such as reminder scheduling

&#x20; Console/Commands/   Artisan commands (install, maintenance)

&#x20; Enums/              Permission and role definitions

&#x20; Events/             Domain events

&#x20; Http/Controllers/   Back office, settings, public portal, and tenant portal controllers

&#x20; Http/Middleware/    Locale, appearance, Inertia sharing, active-user checks

&#x20; Models/             Eloquent models

&#x20; Notifications/      Rent reminders and invitations

&#x20; Providers/          Service providers, including platform bindings

&#x20; Services/           Integrations: payments, mail, WhatsApp, invoices, settings, platform

config/               Application configuration, including platform.php and settings.php

database/             Migrations, factories, and seeders

resources/js/

&#x20; pages/              Inertia page components, one per route

&#x20; components/         Shared and UI components

&#x20; layouts/            Application and auth layouts

&#x20; routes/             Wayfinder-generated typed route helpers

&#x20; lib/                Formatters, i18n, and utilities

routes/

&#x20; web.php             Application routes, grouped by access level

&#x20; settings.php        Settings routes

src/

&#x20; Plugins/            First-party plugins (Mail, WhatsApp, Example)

tests/                Pest feature and unit tests

```



\---



\## Plugins



Plugins extend Rentpoa through the `openkos/platform` SDK. Each plugin extends `Plugin`, declares a manifest with its ID, version, and `coreVersion` constraint, and registers its contributions in `register()`.



Plugins are listed in `config/platform.php` under `plugins`. Composer-discovered plugins are loaded when discovery is enabled. Use `src/Plugins/Example` as a reference implementation. It demonstrates a sidebar item, a dashboard sub-page, a settings page, and a header badge.



\---



\## Development



Run the test suite:



```bash

composer test

```



This clears the config cache, checks code style with Pint, and runs the Pest tests. Other useful commands:



```bash

composer lint          # Fix code style with Pint

composer lint:check    # Check code style without changing files

php artisan route:list # List all routes and their middleware

php artisan wayfinder:generate # Regenerate typed route helpers after route changes

```



\### Adding a page



1\. Add a permission case in `app/Enums/Permission.php`, with a label and description.

2\. Add the route in `routes/web.php` with the matching `permission:` middleware.

3\. Create a controller that returns `Inertia::render('folder/page')`.

4\. Create the React page in `resources/js/pages/folder/page.tsx`.

5\. Regenerate the route helpers and add a navigation link if the page belongs in the sidebar.



Sync permissions to the database with the seeder, then assign them to roles from the Roles screen.



\---



\## Security



\- Passwords are hashed by Laravel. Two-factor authentication is available through Fortify.

\- Routes are protected by authentication, email verification, and permission middleware.

\- Signed payment links are used for tenant payments that don't require a login.

\- Uploaded files (photos, documents, payment proofs) are stored on private disks and served through authorized controllers.

\- Sensitive tenant identifiers are exported only with the `tenants.export\_sensitive` permission.



To report a security issue, don't open a public issue. Contact the maintainers privately at `<security contact email>`.



\---



\## Contributing



1\. Fork the repository and create a feature branch.

2\. Run `composer test` and make sure it passes.

3\. Keep changes focused and include tests for new behavior.

4\. Open a pull request describing the change and any migrations or configuration updates.



\---



\## License



Rentpoa is open-source software licensed under the \[Apache License 2.0](LICENSE).

