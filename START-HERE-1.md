# Start here

This page is for the person who has a WordPress site, a ZIP file and no interest in pretending that plugin installation should be a personality test.

## The file you need

Upload this file to WordPress:

```text
release/tabarc-indexnow-2.0.1.zip
```

Do **not** upload the whole repository ZIP downloaded from GitHub's **Code** button.

## Before installing

You need:

- Administrator access to WordPress.
- WordPress 6.4 or later.
- PHP 7.4 or later.
- A current backup of the site and database.

Most hosts provide backups. Check before changing plugins, particularly on a live shop or membership site.

## Install it

1. Sign in to WordPress.
2. Select **Plugins** in the left menu.
3. Select **Add New Plugin**.
4. Select **Upload Plugin** near the top of the page.
5. Select **Choose File**.
6. Choose `tabarc-indexnow-2.0.1.zip`.
7. Select **Install Now**.
8. When installation finishes, select **Activate Plugin**.

A new **IndexNow** item should appear in the left-hand admin menu.

## Set it up

1. Open **IndexNow**.
2. Leave **Automatic submission** enabled unless you have a specific reason not to.
3. Leave the generated API key unchanged.
4. Tick the public content types you want to notify, usually **Posts** and **Pages**.
5. Add any private or irrelevant URL paths to the exclusions box.
6. Leave **Respect noindex directives** enabled if the site uses SEO controls to hide pages from search engines.
7. Choose how much history to keep. The default is sensible.
8. Optionally choose a square PNG for the admin icon.
9. Select **Save settings**.

## Test it

### Test the key file

The settings page shows a public key-file link. Open it in a new browser tab.

A working result is a plain page containing only a long string of letters and numbers.

### Test a submission

1. Copy the public URL of the site home page.
2. Paste it into **Manual submission**.
3. Select **Submit URL**.
4. Look at **Recent submission history**.

A successful or accepted result normally uses HTTP status `200` or `202`.

## Add an admin icon

1. Prepare a square PNG, ideally around 64 × 64 pixels.
2. In the plugin settings, select **Choose PNG**.
3. Upload the file or choose it from the Media Library.
4. Select **Use this image**.
5. Save the settings.

The picker accepts PNG images. A file merely renamed to `.png` may be rejected because its real MIME type is different.

## Finished

Once the key file loads and one manual submission appears in history, the plugin is set up.

The automatic queue will handle future public content changes. WordPress cron normally processes that queue during ordinary site visits.

For migrations, error messages and less ordinary hosting arrangements, read [`docs/COMPLETE-BEGINNERS-GUIDE.md`](docs/COMPLETE-BEGINNERS-GUIDE.md).
