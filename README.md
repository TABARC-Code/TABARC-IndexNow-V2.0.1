# TABARC IndexNow

**A compact, security-conscious IndexNow plugin for WordPress.**

TABARC IndexNow tells participating search engines when public content on your WordPress site has been added, changed or removed. It does not promise instant rankings, divine favour or a queue of search-engine engineers outside your house. It simply sends the right notification, records what happened and gets out of the way.

**Author:** TABARC-Code, Inc.  
**Licence:** GPL-2.0-or-later  
**Requires:** WordPress 6.4 or later, PHP 7.4 or later

## Download the correct file

For an ordinary WordPress installation, upload only:

```text
tabarc-indexnow-2.0.1.zip
```

Do not upload the entire GitHub repository ZIP from the green **Code** button. That archive is for developers and contains repository paperwork as well as the plugin. WordPress is many things, but clairvoyant is not one of them.

A ready-to-upload release is included in the repository under `release/`.

## What the plugin does

- Queues public URLs when selected content is published, updated, moved or deleted.
- Sends URL batches to the fixed global IndexNow endpoint.
- Allows one-off manual submission of a URL
- Serves the public `{key}.txt` verification file required by IndexNow.
- Supports public post-type selection and wildcard path exclusions.
- Can inspect common SEO metadata and rendered pages for `noindex` directives.
- Retries temporary failures using bounded exponential backoff.
- Understands `Retry-After` responses when a service asks it to slow down.
- Records recent successes, failures and skipped URLs.
- Shows submission totals for the previous 48 hours.
- Migrates useful settings and recent history from the older plugin packages.
- Lets an administrator choose a PNG from the WordPress Media Library for the admin menu and page heading.

There is no Node build, Composer install, React bundle or external PHP library on the live site. The administration screen uses native WordPress forms, permissions, nonces, the HTTP API and the Media Library.

## Five-minute installation

1. Download `tabarc-indexnow-2.0.1.zip` from the release folder or release page.
2. Sign in to WordPress as an administrator.
3. Open **Plugins → Add New Plugin → Upload Plugin**.
4. Choose the ZIP file and select **Install Now**.
5. Select **Activate Plugin**.
6. Open **IndexNow** in the left-hand admin menu.
7. Review the generated key, enabled content types and exclusions.
8. Open the key-file link shown on the settings page. It should display one plain-text key.
9. Use **Manual submission** to submit your home page.
10. Check **Recent submission history** for the result.

That is the basic installation. The full no-assumptions walkthrough is in [`START-HERE.md`](START-HERE.md) and [`docs/COMPLETE-BEGINNERS-GUIDE.md`](docs/COMPLETE-BEGINNERS-GUIDE.md).

## Upgrading from an older version

### From TABARC IndexNow 2.0.0

Upload the new ZIP. If WordPress reports that the destination folder already exists, use **Replace current with uploaded**. Your settings and history remain in place.

### From the older IndexNow packages

1. Back up the site and database.
2. Deactivate the older plugin, but do not delete it yet.
3. Install and activate TABARC IndexNow.
4. Open the new **IndexNow** screen and confirm that the key, automatic-submission choice and exclusions have migrated.
5. Test the public key file and one manual submission.
6. Once satisfied, remove the old plugin.

Do not leave two IndexNow plugins active. Duplicate notifications are not catastrophic, but neither are they clever.

The migration routine checks these legacy options:

- `indexnow-admin_api_key`
- `indexnow-auto_submission_enabled`
- `indexnow-excluded_paths`

It also imports up to 100 recent rows from each recognised legacy success or failure table. Old data is not deleted automatically.

## Settings explained

### Automatic submission

When enabled, the plugin watches selected public content types and queues changed URLs for background submission.

Turning this off pauses automatic work. It does not throw queued URLs away.

### API key

The plugin creates a valid key automatically. In normal use, leave it alone.

The key is not a password. IndexNow requires the same value to be publicly visible in a verification text file, so disguising it with base64 or similar theatre would add confusion rather than security.

### Content types

Choose which public WordPress post types should trigger automatic submission. Posts and pages are the usual choices. Public custom post types may also appear.

Attachments are intentionally excluded.

### Excluded URL paths

Enter one path per line. Use `*` for any number of characters and `?` for one character.

Examples:

```text
/private/*
/preview/?
/member-area/*
```

Patterns are matched against URL paths, not arbitrary external domains.

### Respect noindex directives

When enabled, the plugin checks common SEO metadata and, where necessary, the rendered public page before submitting additions or updates.

This is more cautious but can be slower on hosting with poor loopback performance. The worker therefore handles smaller batches when deep checks are enabled.

### History records to retain

Choose how many log rows to keep. The permitted range is 100 to 5,000.

A larger history is useful for diagnosis. It is not a substitute for analytics, nor should it become one by accident.

### Admin menu PNG icon

Select a genuine PNG from the Media Library. A square image around **64 × 64 pixels** works well.

The plugin validates the attachment MIME type on the server. Renaming a JPEG or WebP file to `.png` does not make it a PNG, despite decades of optimistic file management suggesting otherwise.

### Delete data on uninstall

Ordinary deactivation never removes settings or history.

Enable this option only when you want uninstalling the plugin to remove its database tables, options and saved metadata. Back up first if the information matters.

## Security approach

- Administrative writes require the `manage_options` capability.
- Every state-changing form uses a WordPress nonce.
- Inputs are validated and sanitised at the boundary.
- Rendered values are escaped for their output context.
- Manual URLs must use HTTP or HTTPS and belong to the current WordPress host and public installation path.
- The remote submission endpoint is fixed in code, reducing server-side request forgery risk.
- Loopback checks use WordPress's safe HTTP client with strict limits.
- Response bodies are not retained; only short sanitised result notes are logged.
- Deactivation preserves data. Destructive uninstall behaviour is explicit and opt-in.

See [`SECURITY.md`](SECURITY.md) for reporting and design details. or just DM me.

## Repository map

```text
.
├── tabarc-indexnow.php          Main plugin bootstrap
├── includes/                    Plugin classes and application logic
├── assets/                      Admin CSS and JavaScript
├── languages/                   Translation-ready placeholder
├── docs/                        Architecture, audit, GitHub and beginner guides
├── release/                     Ready-to-upload WordPress ZIP and checksum
├── README.md                    Main project documentation
├── description.md               Repository and project description copy
├── START-HERE.md                Fast beginner route
├── RELEASE-NOTES.md             Ready-to-paste release copy
├── CONTRIBUTING.md              Contribution expectations
├── readme.txt                   WordPress plugin directory format
├── CHANGELOG.md                 Release history
├── SECURITY.md                  Security policy
└── LICENSE.txt                  GPL licence text
```

## Maintainer notes

Repeated saves of the same URL collapse into one queue entry, with the newest event taking precedence. WordPress can fire several hooks during one edit and forwarding each twitch of the machinery would be noisy, not thorough.

The verification route deliberately sends plain text, no-cache headers, `nosniff` and `noindex`. It also forces a successful HTTP status so that a server's earlier 404 decision does not leak through after WordPress has correctly matched the route.

Permalink changes notify both the former URL and the current URL. The old address may need to disappear from an index while the new one needs attention.

Automatic submission uses WordPress cron. This means work runs when the site receives traffic unless the host provides a real cron runner. That is normal WordPress behaviour, though “normal” is doing a little work in that sentence.

## Future ideas

- WP-CLI commands for queue inspection, retries and bulk submission.
- WordPress Site Health checks for key-file reachability and blocked loopback requests.
- Optional multisite network settings.
- Exportable CSV submission history.
- Focused integrations for product stock or structured-content changes that do not save the entire post.
- Better diagnostics for hosts which disable or badly imitate WordPress cron.

## Development and release checks

The release process should include:

```bash
find . -name '*.php' -print0 | xargs -0 -n1 php -l
node --check assets/admin.js
```

Then create an installable ZIP with exactly one top-level folder named `tabarc-indexnow`.

The repository package already includes a prepared release and checksum under `release/`.

## Support boundaries

The plugin notifies IndexNow-compatible services that a URL changed. It does not guarantee crawling, indexing, ranking or traffic.

Failures caused by outbound HTTP restrictions, disabled WordPress cron, invalid SSL configuration, aggressive security middleware or an unreachable local loopback need to be fixed at the site or hosting level. The history screen should at least make those problems visible rather than leaving the administrator to read tea leaves.

## Licence

TABARC IndexNow is released under **GPL-2.0-or-later**. See [`LICENSE.txt`](LICENSE.txt).
