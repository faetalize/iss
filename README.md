# Iss

A primitive issue tracker for OSS work.

It's not meant to be a long term issue log, though git gives you that for free.

## Supported Trackers

Paste one of these as the `url` and `tools/iss` shows its live status:

| Tracker | Example `url` | Status |
| --- | --- | --- |
| GitHub issue or PR | `https://github.com/owner/repo/issues/12`<br>`https://github.com/owner/repo/pull/34` | open, closed, merged (through `gh`) |
| GitLab issue or MR, on any instance | `https://gitlab.freedesktop.org/mesa/mesa/-/issues/12`<br>`https://gitlab.gnome.org/GNOME/gtk/-/merge_requests/34` | open, closed, merged |
| Forgejo or Gitea issue or PR, on any instance | `https://codeberg.org/owner/repo/issues/12`<br>`https://codeberg.org/owner/repo/pulls/34` | open, closed, merged |
| Bugzilla, on any instance | `https://bugzilla.mozilla.org/show_bug.cgi?id=123456` | the bug's status, or its resolution once resolved |
| Launchpad | `https://bugs.launchpad.net/ubuntu/+source/mesa/+bug/123456` | the bug's status in that project |
| Debian BTS | `https://bugs.debian.org/123456` | open, forwarded, fix pending, fixed, done |
| SourceHut todo | `https://todo.sr.ht/~owner/tracker/12` | the ticket's status, or its resolution once resolved |
| Patch on Patchwork, on any instance | `https://patchwork.kernel.org/project/linux-input/patch/20261103091500.12345-1-you@example.com/` | the patch's state, like new, changes requested, accepted |
| Patch on lore | `https://lore.kernel.org/all/20261103091500.12345-1-you@example.com/` | sent, or replied once someone else answers it |
| Patch on another mailing list (lists.freedesktop.org, lists.sr.ht) | `https://lists.freedesktop.org/archives/wayland-devel/2026-November/012345.html` | sent |

Replies on lore count anywhere under your message in its thread, so a reply to one patch of a series counts for its cover letter. The other mailing lists have no state to ask for, so a patch there stays `sent`. Trackers are asked without logging in, except GitHub through `gh`, so private projects show `?`, like any other tracker.

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

`tools/iss` starts a local web page at http://127.0.0.1:8765/ (the next free port if that's taken) and opens it in the browser. New (or `n`) starts a draft, or paste a link there to fill in the title and folder name. Drop files on an issue, pick them with Add files, or paste a screenshot to add them to its folder. While you're editing the README, they're also linked where the cursor is. On an issue, the link button next to its url (or `l`) adds a link, and the × next to a linked one removes it.

`tools/iss list` prints the issues in the terminal, `tools/iss new <project>-<slug>` starts a draft, `tools/iss new <url>` starts one that's already filed, with its title and folder name taken from the tracker (add a name after the url to pick your own), `tools/iss delete <folder>` deletes one, and `tools/iss link <folder> <url>` and `tools/iss unlink <folder> <url>` add and remove links. Statuses aren't stored: every tracker is asked at once, and each one fills in as it answers.

## License

GPL-3.0, see [LICENSE](LICENSE).
