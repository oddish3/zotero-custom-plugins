# Zotero release checks

Use a separate Zotero profile and data directory. Never test a beta against
the normal library. Disable sync and automatic TLDR retrieval in the test profile.

1. Read the developer migration notes for the target Zotero release.
2. Wait for the release freeze before declaring support for a new major version.
3. Run `npm run check` in Zotero-TLDR.
4. Run `node test.cjs` and `node --check bootstrap.js` in zotero-supplement-pdf.
5. Install both built packages in the test profile.
6. Check startup, menus, settings, disable/re-enable, and restart.
7. Test collection commands with no selection, one collection, and several collections.
8. Test a collection and its child selected together. Each item must appear once.
9. Test a saved search. It must not expand cleanup or reports to the entire library.
10. Test PDF reports, summary storage, and normal PDF reindexing with sample records.
11. Convert a synthetic DOCX and PPTX. Check the PDF and retained originals.
12. Repeat conversion. It must return the existing PDF.
13. Inspect the Zotero error log for plugin failures.
14. Update both package manifests and the update file to the tested version range.
15. Publish packages and `updates.json` in the same GitHub release.
16. Check that Zotero can find the update from an older test installation.

Keep plugin IDs unchanged. Publish only packaged extension files and release
information. Never publish Zotero profiles, libraries, credentials, or local
configuration files. Both plugins use this update address:

`https://github.com/oddish3/zotero-custom-plugins/releases/latest/download/updates.json`

The update file must include both IDs, exact package versions, version limits,
release-specific download links, and SHA-256 package hashes. SHA-256 is the
checksum used to verify package contents. Do not replace a package after
publishing its hash. Make a new release instead.

Monitor the zotero-dev release announcements. Repeat these checks before each
major release. Automatic updates distribute tested fixes; they cannot test a
future Zotero interface. Keep compatibility limits in place.

## Extend compatibility without a package update

Use this procedure only after both plugins pass the checks for the target
Zotero release. `11.0.*` below is an example, not a tested version.

1. Download `updates.json` from the latest release of
   `oddish3/zotero-custom-plugins`.
2. For each tested plugin, find
   `addons[plugin ID].updates[0].applications.zotero.strict_max_version`.
3. Change that value to the tested range, for example `"11.0.*"`.
4. Keep `version`, `update_link`, `update_hash`, and `strict_min_version` unchanged.
5. Upload the edited `updates.json` to the same release. Replace only that asset.
   For the current release, use:

   ```sh
   gh release upload plugins-2026-10-25 updates.json \
     --repo oddish3/zotero-custom-plugins --clobber
   ```

6. Verify the JSON at the public update address above.
7. In an isolated profile, check that an existing installation receives the
   compatibility change. Use the Plugins gear menu's update check.
8. Update `addon/manifest.json` and Supplement PDF's `manifest.json` locally so
   the next package build declares the same tested range.

If only one plugin passes, extend only its entry. Leave the other entry intact.
Do not rebuild or replace the existing XPI assets during this procedure.
An older XPI can retain its original limit for direct installation. When a new
package is needed for fresh installations, publish a new version and hash.

The update metadata can change compatibility for the same plugin version.
This removes routine manual reinstalls. It still requires a maintainer to test
and publish each new compatibility range. No scheduled monitor is configured.

Sources:
- https://www.zotero.org/support/dev/zotero_10_for_developers
- https://www.zotero.org/support/dev/zotero_7_for_developers
