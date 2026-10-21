# Resume Lab: CRUD + Authentication (PHP / MySQL)

Continues the PHP/MySQL Resume Lab. Week 2 covers managing the `skill` table with INSERT / UPDATE / DELETE. Week 3 adds user registration, login and logout.

## Project structure
| File | Purpose |
|------|---------|
| `index.php` | Resume page |
| `skills.php` | List skills |
| `skill_create.php` | INSERT a skill |
| `skill_edit.php` | SELECT + UPDATE a skill |
| `skill_delete.php` | DELETE a skill |
| `register.php`, `login.php`, `logout.php` | Authentication pages |
| `auth.php` | Session, login check and auth CSRF helpers |
| `crud_bootstrap.php` | DB connection, CSRF, escaping and page layout helpers |
| `config/db.php` | Database settings |
| `database/this_week_migration.sql` | Week 3 migration (creates the `users` table) |
| `assets/` | CSS and images |

## Setup
1. Use a separate practice database that already contains the Week 2 tables (`profile`, `skill`, ...). The original `setup.sql` is **not** included in this repository, and it DROPS tables when run, so never run it against a database with important data.
2. Select your existing resume database in phpMyAdmin and import `database/this_week_migration.sql`. It is non-destructive (`CREATE TABLE IF NOT EXISTS`) and adds the `users` table linked to `profile`.
3. Configure the database connection. `config/db.php` reads `DB_HOST`, `DB_USER`, `DB_PASS` and `DB_NAME` from environment variables and falls back to the defaults in the file. **Never commit real credentials.** Replace the fallback values with local-only placeholders (for example `localhost` / `root` / empty password), or keep your real settings in an untracked file such as `config/db.local.php` (already in `.gitignore`).
4. Start Apache + MySQL, place this folder in your local web root, and open `http://localhost/<folder>/skills.php`.

## Student activities

### Week 2: CRUD
- Task 1 (INSERT): `skill_create.php` TODO 1. Save a new skill and confirm the row appears.
- Task 2 (SELECT + UPDATE): `skill_edit.php` TODO 2a / 2b. Change the level and verify the new value.
- Task 3 (DELETE): `skill_delete.php` TODO 3. Delete one practice record and verify it disappears.
- Task 4: Explain why UPDATE and DELETE require `WHERE`. Demonstrate parameter binding.

### Week 3: Authentication
- Task 5 (Login): `login.php` TODO 1-5. Query the user by email with a prepared statement, check the password with `password_verify()`, call `session_regenerate_id(true)`, set the session values, then redirect to `skills.php`.
- Task 6 (Logout): `logout.php` TODO 6. After CSRF verification, clear `$_SESSION`, expire the session cookie and destroy the session.
- Task 7: Review `register.php` and explain how passwords are stored (hashing, never plaintext).

### Submission
- Task 8: Git workflow. Create a feature branch, commit your SQL and PHP completion, push to GitHub (NO secrets), then submit the repository URL and screenshots.

## SQL mapping hints
- INSERT: `INSERT INTO skill (profile_id, skill_name, skill_level) VALUES (?, ?, ?)`; bind order: `profile_id`, `skill_name`, `skill_level`.
- SELECT: filter using `id = ? AND profile_id = ?`.
- UPDATE: `SET skill_name = ?, skill_level = ?` with both WHERE conditions.
- DELETE: never omit `WHERE id = ? AND profile_id = ?`.
- Login: select `id`, `profile_id` and `password_hash` from `users` where `email = ?`.

## Security / scope
The pages use prepared statements, input validation, output escaping, POST for changes and CSRF tokens. Passwords are stored with `password_hash()` and sessions use HttpOnly, SameSite and strict-mode settings.

The skill pages (`skills.php`, `skill_create.php`, `skill_edit.php`, `skill_delete.php`) still operate on the first profile in the database and do not yet call `require_login()` from `auth.php`. They are a classroom example and are not ready for public deployment on Angsila without authorization and production hardening. Prefer local-only access for exercises.

## Grading (100 points)
INSERT 25, SELECT+UPDATE 30, DELETE 25, screenshots/testing 10, Git commits 10.

**Starter:** fill in every TODO before submitting.
