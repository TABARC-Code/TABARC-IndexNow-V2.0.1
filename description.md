# Project description

## GitHub repository description

A lightweight, secure IndexNow plugin for WordPress, rebuilt by TABARC-Code, Inc. with queued submissions, migration support, noindex checks, history, retries and a Media Library PNG admin icon.

## Short release description

TABARC IndexNow notifies IndexNow-compatible search engines when public WordPress content is added, changed or removed. It uses native WordPress administration screens, a deduplicated background queue, strict same-site URL validation and bounded retry handling.

## Longer project description

TABARC IndexNow is a clean-room rebuild of two older plugin packages which had become divided between source files and a compiled administration application. The new version replaces that arrangement with a smaller native WordPress implementation.

The plugin can automatically queue changed public URLs, submit a local URL manually, serve the required public verification key, respect exclusions and `noindex` directives, retry temporary failures and retain a controlled amount of recent history. It also supports migration from recognised legacy options and tables.

Administrators can select a genuine PNG from the WordPress Media Library for the plugin's menu and page icon. It is a small feature, but staring at another generic cog icon for the next six years seemed unnecessary.

The live plugin has no Node, React, Composer or third-party PHP dependency. Security-sensitive operations use WordPress capabilities, nonces, validation, escaping and the safe HTTP API. The IndexNow endpoint is fixed in code, and submitted URLs must belong to the current site.

# rebuild of a previous project.

#TABARC-Code
