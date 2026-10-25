# TLDR and plugin update guide

Applies to Zotero TLDR 2.4.7 and Supplement PDF 0.2.2.

## Install once

Download both XPI files from the [release page](https://github.com/oddish3/zotero-custom-plugins/releases/latest).
In Zotero, open **Tools → Plugins**.
Select the gear menu.
Select **Install Plugin From File…**.
Install each XPI file.
Keep **Update Add-ons Automatically** enabled in the gear menu.

This first installation replaces the old placeholder update addresses.
Future published updates use the working update source.
The release was tested with a separate Zotero 10.0.6 profile.

## Stop duplicate TLDR notes

The plugin shares overlapping requests for the same item in one Zotero session.
It reuses an existing Semantic Scholar note when it refreshes a summary.
It also removes exact duplicate notes during a fetch.
This does not guarantee that two computers cannot create duplicates before sync.

To clean existing duplicates, right-click a collection or library root.
Select **Remove duplicate TLDR notes**.
Collection cleanup includes subcollections.
Library cleanup includes that library.
Removed notes go to Trash.
Restore them there if necessary.

Cleanup compares the full note content, including its source link.
It keeps different versions and notes with extra personal text.
It also keeps personal summaries, quotations, tagged notes, notes with relations,
and notes that cannot be edited.
Unmarked notes from other plugins are outside this cleanup.
The TLDR panel shows only the newest Semantic Scholar summary.
Older, different notes can therefore remain in the item tree.

## Request missing summaries

| Option | Effect | Best use |
| --- | --- | --- |
| Collection: **Fetch missing Semantic Scholar TLDRs** | Requests summaries for items without a stored Semantic Scholar TLDR. Keeps existing summaries. | Normal backfill. |
| Items: **Research tools → Fetch or refresh Semantic Scholar TLDR** | Requests summaries again, including items with an existing summary. Reuses the note. | Refresh selected papers. |
| Automatic retrieval | Checks new or edited items. Retries unsuccessful requests later. | Routine use after backfill. |

For backfill, select one or more collections in the left pane.
Right-click the selection.
Select **Fetch missing Semantic Scholar TLDRs**.
Include each subcollection in the selection if it needs backfill.
This command checks direct collection members only.
It does not process a library root or saved search.

There is no dedicated whole-library, missing-only command in 2.4.7.
You can use a temporary collection for a whole-library backfill:

1. Create a collection named **TLDR backfill**.
2. Select the library root.
3. Clear any search or filter.
4. Select all items in the centre pane.
5. Drag the selected items onto **TLDR backfill**.
6. Right-click **TLDR backfill**.
7. Select **Fetch missing Semantic Scholar TLDRs**.

Adding items to a collection adds membership. It does not duplicate the records.
This includes unfiled items that were visible at the library root.
Keep the collection until the batch finishes.
You can remove the collection later while retaining its items.
Do not choose an option that deletes the items.

An alternative is to select all library items and use the item refresh command.
This also refreshes existing summaries and can cause many unnecessary requests.
A dedicated library command would remove the temporary-collection step.

Enable automatic retrieval in **Settings → Zotero TLDR**.
Set a Semantic Scholar API key there to reduce throttling.
The key stays in the profile and is sent to Semantic Scholar.
The plugin stores unsuccessful requests for later retries.
It checks due retries at startup and hourly while Zotero runs.
Some papers have no Semantic Scholar TLDR. Repeated requests cannot create one.

## Compatibility with later Zotero versions

Both packages and the public update file declare `strict_max_version: "10.0.*"`.
This includes 10.0.6, 10.0.7, and other 10.0.x versions.
It excludes 10.1 and 11.0 until compatibility is checked.
No package update is needed just for another 10.0.x patch release.

For a later release, test both plugins in a separate profile first.
If their code still works, change only the public update metadata.
Keep the existing plugin versions, package links, and hashes.
Zotero can receive that compatibility change without a new plugin package.
If code changes are needed, publish a new package version instead.

Do not use an unlimited maximum such as `99.*`.
Do not remove the maximum from the update file.
Zotero warns that these bypasses can be blocked.
Automatic updates distribute tested fixes. They do not test future Zotero APIs.
No scheduled compatibility monitor or automatic future-version testing is installed.

See [release checks and maximum-version instructions](zotero-compatibility.md).

Sources: [Zotero 10 developer notes](https://www.zotero.org/support/dev/zotero_10_for_developers),
[Zotero's version-limit guidance](https://forums.zotero.org/discussion/133268/is-zotero-still-enforcing-strict-max-version-for-plugins).
