# Enterprise SAP Laravel

> A Laravel-based enterprise operations platform inspired by the structure, discipline, and integrated processes of SAP business systems.

[![PHP](https://img.shields.io/badge/PHP-8.5-777BB4?logo=php&logoColor=white)](https://www.php.net/) [![Laravel](https://img.shields.io/badge/Laravel-13-FF2D20?logo=laravel&logoColor=white)](https://laravel.com/) [![Filament](https://img.shields.io/badge/Filament-5-FDAE4B)](https://filamentphp.com/) [![Livewire](https://img.shields.io/badge/Livewire-4-FB70A9)](https://livewire.laravel.com/)

[![Install](https://github.com/liberusoftware/project-enterprise-sap-laravel-filament/actions/workflows/install.yml/badge.svg?branch=main)](https://github.com/liberusoftware/project-enterprise-sap-laravel-filament/actions/workflows/install.yml) [![Tests](https://github.com/liberusoftware/project-enterprise-sap-laravel-filament/actions/workflows/tests.yml/badge.svg?branch=main)](https://github.com/liberusoftware/project-enterprise-sap-laravel-filament/actions/workflows/tests.yml) [![Docker](https://github.com/liberusoftware/project-enterprise-sap-laravel-filament/actions/workflows/docker.yml/badge.svg?branch=main)](https://github.com/liberusoftware/project-enterprise-sap-laravel-filament/actions/workflows/docker.yml) [![Codecov](https://codecov.io/gh/liberusoftware/project-enterprise-sap-laravel-filament/branch/main/graph/badge.svg)](https://codecov.io/gh/liberusoftware/project-enterprise-sap-laravel-filament) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE.md)

Enterprise SAP Laravel is a modular business application foundation for organisations that need one dependable place to coordinate people, processes, data, and operational decisions. It brings together identity, organisations, administration, audit, integrations, analytics, search, notifications, and background processing in a Laravel application that can be shaped around a company’s operating model.

The project is SAP-inspired, not an SAP product and is not affiliated with SAP SE. SAP terminology is used as design inspiration for integrated enterprise workflows, clear ownership, and traceable business records.

## Why this foundation

Enterprise software is most useful when the parts agree with each other. This application treats the business as a connected system rather than a collection of isolated screens:

- **Organisational structure** — organisations, teams, roles, permissions, profiles, and sessions
- **Governance and control** — audit trails, settings, feature flags, access policies, and two-factor authentication
- **Process enablement** — queues, scheduled work, notifications, webhooks, integrations, and import/export flows
- **Operational insight** — analytics, search, observability, Pulse, Telescope, and application diagnostics
- **Composable delivery** — independently versioned modules and themes installed through Composer
- **Presentation flexibility** — Filament administration, Livewire experiences, APIs, and switchable themes

The result is a practical starting point for ERP-adjacent applications, shared services, internal operations portals, and domain products that need enterprise-grade foundations without locking every business into the same process model.

## Architecture

```text
Enterprise SAP Laravel
├── app/              # Host composition and application-specific integration
├── modules/          # Composer-installed business and platform capabilities
├── themes/           # Composer-installed visual systems and theme assets
├── config/           # Runtime policy, module enablement, and application configuration
├── database/         # Migrations, factories, and seeders
├── resources/        # Host views and frontend entry points
└── tests/            # Host behaviour and cross-module architecture tests
```

Modules own cohesive capabilities and expose stable contracts, actions, events, registries, or identifiers. Presentation adapters remain optional: domain modules do not need to depend on Filament or a particular theme. Themes provide their own metadata, compatibility information, assets, and fallback behaviour.

Composer and `composer.lock` are the source of installation and version truth. The tracked `modules/` and `themes/` directories make the exact deployed package contents visible in code review; do not patch an installed package only in this host repository.

## Technology baseline

| Concern | Version or choice |
| --- | --- |
| PHP | 8.5 |
| Laravel | 13.x |
| Filament | 5.x |
| Livewire | 4.x |
| Database | Any Laravel-supported SQL database |
| Frontend | Vite, Tailwind CSS 4, and the project’s selected theme |
| Runtime services | Horizon, Pulse, Telescope, Octane, and Reverb where enabled |

## Quick start

```bash
git clone https://github.com/liberusoftware/project-enterprise-sap-laravel-filament.git
cd enterprise-sap-laravel

composer install
cp .env.example .env
php artisan key:generate

npm install
npm run build

php artisan migrate
php artisan serve
```

Review `.env` before running migrations. Use `php artisan migrate --seed` when example data is required. For containerised or Kubernetes-oriented environments, see [`install.sh`](install.sh), [`docker-compose.yml`](docker-compose.yml), and [`k8s/`](k8s/).

## Capability areas

The host assembles a broad platform from independently released packages. The enabled graph can be inspected and queried without manually scanning application classes:

```bash
php artisan module:features
php artisan module:features health
php artisan module:status search
```

Typical capability areas include:

| Enterprise concern | Foundation capabilities |
| --- | --- |
| Identity and access | Authentication, social login, passkeys, sessions, devices, two-factor authentication, roles, and permissions |
| Organisation management | Organisations, teams, profiles, settings, and tenancy-aware application structure |
| Governance | Audit records, activity comments, feature flags, module diagnostics, and controlled configuration |
| Integration | APIs, webhooks, external integrations, localisation, analytics providers, and import/export |
| Operations | Queues, Horizon, scheduler, notifications, search, files/media, observability, and backups |
| Experience | Filament panels, Livewire adapters, theme inheritance, dark mode, and accessibility-minded fallbacks |

## Module and theme development

Every module should own one cohesive capability and keep its public boundary intentional. A typical module contains:

```text
composer.json
module.json
README.md
LICENSE.md
CHANGELOG.md
src/
tests/
```

Themes follow the same release discipline and add `theme.json`, source assets, compatibility metadata, accessibility expectations, and safe fallback behaviour. Start with the [module development guide](docs/MODULE_DEVELOPMENT.md) and [theme architecture](docs/THEME_ARCHITECTURE.md).

To update a package, change the package repository, release it, and then update this host:

```bash
composer update --with-all-dependencies
composer update liberusoftware/search --with-dependencies
```

## Quality gates

Run the same checks used by continuous integration before opening a pull request:

```bash
composer validate --strict
composer audit --locked
vendor/bin/pest
vendor/bin/pint --test
vendor/bin/phpstan analyse --no-progress
npm run build
```

The test suite covers application behaviour and cross-package contracts. Architecture tests protect dependency direction, package metadata, host isolation, presentation boundaries, and Composer ownership.

## Documentation

- [Enterprise architecture](docs/ENTERPRISE_ARCHITECTURE.md)
- [Module development](docs/MODULE_DEVELOPMENT.md)
- [Foundation compliance](docs/FOUNDATION_COMPLIANCE.md)
- [Foundation module matrix](docs/FOUNDATION_MODULE_MATRIX.md)
- [Theme architecture](docs/THEME_ARCHITECTURE.md)
- [Theme system](docs/THEME_SYSTEM.md)
- [Messaging architecture](docs/MESSAGING_ARCHITECTURE.md)
- [Search architecture](docs/SEARCH_ARCHITECTURE.md)
- [Localisation](docs/MULTI_LANGUAGE.md)

## Security

Please do not report security vulnerabilities in public GitHub issues. Send reproduction details and the affected version to `security@liberusoftware.com` so the report can be handled privately.

## Contributing

Contributions are welcome. Keep pull requests focused, explain the business or technical problem being solved, include tests for behaviour changes, and document user-visible or breaking changes. When a change belongs to a reusable module or theme, contribute it to that package repository instead of modifying its installed copy here.

## Licence

Enterprise SAP Laravel is open-source software available under the [MIT Licence](LICENSE.md). SAP and related marks are the property of their respective owners; this project is an independent, SAP-inspired implementation.


### Automatic module and theme publishing

`.github/workflows/sync-packages.yml` publishes packages changed under `modules/`
or `themes/` on pushes to `main`. A manual run reconciles all tracked packages.
Each package needs a `composer.json` with a `liberusoftware/*` name. Files are
copied from Git into the root of `liberusoftware/module-<directory>` or
`liberusoftware/theme-<directory>`. Where `.liberu-meta.json` defines a
`repositoryPrefix`, that design is authoritative (for example,
`modules/module-manager` becomes `module-module-manager`). Without a configured
prefix, an existing `module-` or `theme-` directory prefix is kept. Destination
history is preserved, deleted files are removed, and unchanged packages do not
receive another commit or release. Removing an entire package leaves its public
repository intact. This workflow makes tracked package edits publishable; install
checks still require the project's Composer dependencies to resolve to those
published package contents.

Configure these secrets in the `packagist` GitHub environment (or as repository
or organization secrets available to this repository):

- `MODULE_SYNC_TOKEN`: organization-authorized GitHub token able to create public
  repositories and read/write package contents and releases across the target
  repositories. Because package snapshots include `.github/workflows`, allow
  workflow writes too. A classic PAT needs `repo` and `workflow` scopes and an
  organization role permitted to create repositories. The default `GITHUB_TOKEN`
  cannot publish across these repositories.
- `PACKAGIST_USERNAME`: Packagist account that owns or maintains these packages.
- `PACKAGIST_API_TOKEN`: that account's **main** API token, required to submit
  new packages as well as update existing packages.

New packages start at their declared stable Composer version, or `v1.0.0`.
Existing packages use the highest stable repository tag: a higher declared
Composer version wins, `BREAKING CHANGE:` or `type!:` commits bump major,
`feat:` commits bump minor, and other changes bump patch. Only commits touching
the package since the last successful sync affect this decision. The first
automated run and manual runs reconcile all packages, so canceled or failed
runs cannot silently drop package changes. Tags and commits are
pushed atomically, then a GitHub release is created and the package is submitted
to or refreshed on Packagist. Rerunning a failed job completes publication
without issuing another release for the same contents. Concurrent pushes from
other projects are rejected instead of force-pushed; rerun the failed job.

The source project and its lockfile are not rewritten by publication. Different
projects publishing the same directory name share a destination repository;
coordinate their package contents. Run the publisher's local tests with
`python3 -m unittest discover -s .github/scripts -p 'test_sync_packages.py'`.

<!-- presentation-family:begin -->
## Project family and presentation options

This repository hosts **Enterprise SAP with Laravel and Filament**. Enterprise workspace and cross-product orchestration. Its existing installation instructions and manifests govern this application. The other presentations are independent skeletons derived from their matching Boilerplate foundation; repository availability does not establish product feature parity.

### Other Laravel and Filament projects

| Project | Scope |
| --- | --- |
| [Boilerplate](https://github.com/liberusoftware/project-boilerplate-laravel-filament) | Shared application foundation and module composition |
| [Accounting ERP](https://github.com/liberusoftware/project-accounting-erp-laravel-filament) | Ledgers, tax, reporting and finance workflows |
| [Automation](https://github.com/liberusoftware/project-automation-laravel-filament) | AI gateways, approvals, rules and integrations |
| [Billing](https://github.com/liberusoftware/project-billing-laravel-filament) | Subscriptions, pricing, payments and provisioning |
| [Browser Game](https://github.com/liberusoftware/project-browser-game-laravel-filament) | Game worlds, players and community workflows |
| [CMS](https://github.com/liberusoftware/project-cms-laravel-filament) | Content authoring, publishing and delivery |
| [Control Panel](https://github.com/liberusoftware/project-control-panel-laravel-filament) | Infrastructure and hosting operations |
| [CRM](https://github.com/liberusoftware/project-crm-laravel-filament) | Relationships, sales pipelines and customer service |
| [Ecommerce](https://github.com/liberusoftware/project-ecommerce-laravel-filament) | Catalogs, checkout, orders and fulfillment |
| [Genealogy](https://github.com/liberusoftware/project-genealogy-laravel-filament) | Family trees, research, evidence and collaboration |
| [Liberu Platform](https://github.com/liberusoftware/project-liberu-official-laravel-filament) | Official sites, staff applications and platform composition |
| [Maintenance](https://github.com/liberusoftware/project-maintenance-laravel-filament) | Assets, work orders, schedules and field operations |
| [Music DAW](https://github.com/liberusoftware/project-music-daw-laravel-filament) | Composition, recording, sequencing and audio production |
| [Real Estate](https://github.com/liberusoftware/project-real-estate-laravel-filament) | Listings, sales, lettings and agency operations |
| [Social Network](https://github.com/liberusoftware/project-social-network-laravel-filament) | Profiles, feeds, communities and moderation |

### Other presentations for Enterprise SAP

- [Laravel · Inertia · Vue](https://github.com/liberusoftware/project-enterprise-sap-laravel-inertia-vue): Laravel routes and sessions with Vue pages over the Inertia protocol.
- [Laravel · Inertia · React](https://github.com/liberusoftware/project-enterprise-sap-laravel-inertia-react): Laravel routes and sessions with React pages over the Inertia protocol.
- [Nuxt · Vue](https://github.com/liberusoftware/project-enterprise-sap-nuxt-vue): Independent Vue application with Nuxt server rendering and reviewed API integration.
- [Next.js · React](https://github.com/liberusoftware/project-enterprise-sap-next-react): Independent React application with explicit Next.js server/client boundaries.
- [React Native · Expo](https://github.com/liberusoftware/project-enterprise-sap-mobile-react): Native iOS/Android application using React Native and Expo.
- [Flutter · Mobile](https://github.com/liberusoftware/project-enterprise-sap-mobile-flutter): Flutter iOS/Android application using Dart and mobile widgets.
- [Dart · Desktop](https://github.com/liberusoftware/project-enterprise-sap-dart-desktop): Dedicated Dart desktop application with a Flutter host for Linux, macOS and Windows.
- [Dart · Mobile](https://github.com/liberusoftware/project-enterprise-sap-dart-mobile): Separate Dart mobile application with a Flutter host for iOS and Android.

Use the [presentation implementation guides](https://github.com/liberusoftware/documentation/blob/main/guides/presentation/README.md) and [product documentation](https://github.com/liberusoftware/documentation/blob/main/projects/sap/README.md) to select compatible modules, reviewed contracts and the correct deployment boundary. Laravel remains authoritative for domain rules and tenant authorization across all clients.
<!-- presentation-family:end -->
