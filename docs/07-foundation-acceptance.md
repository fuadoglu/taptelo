# T01 · Foundation acceptance

**Status: ready for implementation.** This contract describes files and commands that T01 must create. It does not record a successful application build.

## Required changes

| Path | Acceptance |
| --- | --- |
| composer.json and composer.lock | Laravel 13, PHP ^8.4, pinned transitive install via the lockfile. Scripts: lint runs Pint in check mode, test runs the Laravel test suite, and check runs validate, lint and test. |
| .env.example | Contains safe local defaults and names for APP_ENV, APP_DEBUG, APP_URL, APP_KEY and DB_*. No live secrets. Use DB_CONNECTION=mysql for MariaDB, host 127.0.0.1 and a dedicated local/test database. |
| bootstrap/app.php and routes/web.php | Laravel boot probe at /up. Localized foundation entry at /az and /en demonstrates translation config. No catalog UI, public submission or Telegram integration is needed for T01. |
| lang/az and lang/en | Localized foundation labels and matching keys. Explicit locale URLs remain stable when Accept-Language changes. |
| tests/Feature and database/migrations | Smoke tests for boot, locale response, SQL access and safe production defaults. Run the database-backed tests against MariaDB, never fall back silently to SQLite. |
| .github/workflows/ci.yml | CI triggers on PRs and main. Ubuntu 24.04 runner, PHP 8.4 and MariaDB 10.11 Linux service. Apply conservative workflow permissions. Reviewably pin action revisions and record resolved tool/runtime versions. |
| README.md | Actual local prerequisites, commands and troubleshooting. Explain which behavior is available at T01. |

Use MariaDB 10.11 as the required CI target. PHP 8.5 and MySQL 8.4 are additional compatibility targets only after their own suites pass. Composer 2 is required during build. Do not require production Node, a worker daemon, a real Telegram credential or a real cPanel account for T01. Preserve the Laravel public/ boundary.

## Reproducible clean check

Run on a fresh checkout with an empty disposable MariaDB 10.11 database. Set DB_HOST, DB_PORT, DB_DATABASE, DB_USERNAME and DB_PASSWORD for that database in the copied .env before running migrations. Use a dedicated test database for commands that rebuild tables. Keep application staging and production out of these commands.

```sh
php -v
composer --version
composer validate --strict
composer install --no-interaction --prefer-dist
cp .env.example .env
# Set DB_* to the disposable MariaDB database here.
php artisan key:generate --no-interaction
php artisan migrate --no-interaction
php artisan migrate:status --no-interaction
composer check
php artisan config:cache
php artisan route:list --path=up
php artisan optimize:clear
```

All commands exit zero. Migration status shows all shipped migrations as ran. Capture the DB server version using SELECT VERSION() in the suite and assert MariaDB 10.11 in CI; fail before feature tests if it differs. Test app boot using Laravel's HTTP test layer and once over an actual local HTTP listener on 127.0.0.1:8000. /up returns 200 without secrets. /az and /en return 200 with appropriate html lang and localized text even when the browser language differs. /up checks process boot; database access is a separate test. A simulated production configuration must disable debug and prevent the local fixture path from running.

## Completion evidence

Record the exact tested revision, PHP/Composer/database versions, commands and outcomes, CI link, and any plan-specific limitations. Mark T01 complete in the delivery index only after the clean-check evidence passes. Leave live Mochahost validation, DNS and mailbox confirmation for T08.

References: [Laravel deployment](https://laravel.com/docs/13.x/deployment) · [GitHub Actions service configuration](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax#jobsjob_idservices)
