# Iss

A primitive issue tracker for OSS work.

It's not meant to be a long term issue log, though git gives you that for free.

## Supported Trackers

Paste one of these as the `url` and `tools/iss` shows its live status:

| Tracker | Example `url` | Status |
| --- | --- | --- |
| GitHub issue or PR | `https://github.com/owner/repo/issues/12`<br>`https://github.com/owner/repo/pull/34` | open, closed, merged (through `gh`) |
| GitLab issue or MR, on any instance | `https://gitlab.freedesktop.org/mesa/mesa/-/issues/12`<br>`https://gitlab.gnome.org/GNOME/gtk/-/merge_requests/34` | open, closed, merged |
| Bugzilla, on any instance | `https://bugzilla.mozilla.org/show_bug.cgi?id=123456` | the bug's status, or its resolution once resolved |
| Patch sent to a mailing list (lore, patchwork, lists.freedesktop.org) | `https://lore.kernel.org/all/20261103091500.12345-1-you@example.com/` | sent |

A mailing list has no state to ask for, so a patch stays `sent`. GitLab and Bugzilla are asked without logging in, so private projects show `?`, like any other tracker.

## Getting started

```sh
git clone https://github.com/faetalize/iss
iss/tools/iss
```

The first run asks where your issues should live and whether to sync them to a private GitHub repo. Your issues stay in their own repo, so `git -C iss pull` updates the tool without touching them.

You need Python 3 and git.
For GitHub specifically, you need the [`gh`](https://cli.github.com/) CLI. Log in with `gh auth login` if you track GitHub issues or PRs.

## Structure

One folder per issue.

`<project>-<slug>/`

Each folder has one `README.md`, plus whatever files the issue needs (repros, patches, logs, screenshots). The README starts with:

```
---
url:
---

# Title
```

* url: the issue, MR or PR once it's filed, or the lore link for a patch sent by email. An MR or PR doesn't need an issue first. Empty while it's a draft. Filing means pasting the link here, nothing else.
* more `url:` lines: link the MR that fixes the issue, or the issue behind an MR. The first `url:` sets the status, the others show under it with their own.
* hold: add `hold: true` for something parked on purpose.

Below that, write whatever is useful: the draft write-up before filing, notes the issue doesn't have after. Don't record the status or the title's tracker state. `tools/iss` fetches them.

To point at another issue, use its URL, or `../<folder>/` for a draft.

## Tools

`tools/iss` starts a local web page at http://127.0.0.1:8765/ (the next free port if that's taken) and opens it in the browser. On an issue, the link button next to its url (or `l`) adds a link, and the × next to a linked one removes it.

`tools/iss list` prints the issues in the terminal, `tools/iss new <project>-<slug>` starts a draft, `tools/iss delete <folder>` deletes one, and `tools/iss link <folder> <url>` and `tools/iss unlink <folder> <url>` add and remove links. Statuses aren't stored: every tracker is asked at once, and each one fills in as it answers.

## License

GPL-3.0, see [LICENSE](LICENSE).
