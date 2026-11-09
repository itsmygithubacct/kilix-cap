# Kilix Cap rooms: direct apps, Housekeeping and the Game Room

What the Study props, Server Room objects, Library and Balcony launch, how the Cleaning Room's Housekeeping stays safe, and how the Game Room reads the Kilix 95 catalog. Moved from the [README](../README.md); the text is unchanged.

## Direct app behavior

The Study's Clock, Inbox, Outbox, Mail, Profile, Notepad, Dates, Contacts,
Files, Phone, Writer, Calculator, and Web props all queue their real mapping
from the object click itself. The launcher uses fixed argument vectors passed
directly to `posix_spawn`; it never invokes a shell.

Mail uses the saved target when one exists. Without one, it opens the first
installed desktop mail program (or `xdg-email`) directly. The optional saved
target and Web home remain a private file at
`~/.local/gpu_terminal/kilix-cap/config`; set `KILIX_CAP_CONFIG_HOME` to an
absolute directory to relocate it. A target is one executable name/absolute
path or one `http(s)` webmail URL, never a command line.

The Computer defaults to `https://news.ycombinator.com/`. Its click creates a
Firefox ESR tab in the background with an isolated temporary profile and an
explicit `--new-window` start page. A compact green boot sequence runs on the
physical Study monitor while Kilix captures the browser. The glass zoom begins
only after Kilix reports capture readiness: either a changed capture after the
startup snapshot or, with Kilix 0.1.4 or newer, an emitted initial capture
after three seconds without a change. The grace path is a handoff heuristic,
not proof that the requested page finished loading. Only then does the exact
captured tab receive focus. Override the start page by adding a single strict
URL line to the private config:

~~~text
web_home=https://example.org/
~~~

Only an `http://` or `https://` URL without whitespace is accepted. A missing
or invalid value falls back to Hacker News.

The Server Room maps:

| Physical object | Direct result |
|---|---|
| Left monitor | Live green-on-black TUI with system logs, warning-or-higher alerts, and local system mail. |
| Right monitor | Live TUI with user processes and active TCP/UDP network connections. |
| Brass console | Kilix Settings Center in a fresh tab; outside Kilix, the installed system settings application. |
| Service rack | Installed disk/storage administration application. |
| Patch panel | Installed network settings application. |
| Right cabinet, upper door | Kilix Software Center in a fresh tab; outside Kilix, the installed software administration application. |
| Right cabinet, lower door | PDF Conversion in a fresh Kilix tab, installed and launched through the shared catalog application verb. |

The two monitor TUIs are read-only, refresh every two seconds, and open in
fresh authenticated Kilix tabs. Press `r` to refresh or `q` to close.

Library books use `xdg-open` on actual files under this checkout. Balcony
instruments prefer installed Weather and astronomy applications. Exact
candidate order is documented in [docs/APPS.md](APPS.md).

## Housekeeping safety

Cleaning Room stations open `tools/mansion_tui.py cleanup` in a fresh Kilix
tab. It previews every target and requires a second explicit `y`
confirmation before changing anything.

The fixed actions are:

- user-owned `/tmp` entries older than seven days;
- the current user's thumbnail cache;
- the APT download cache via `pkexec apt-get clean`;
- system journals older than fourteen days via
  `pkexec journalctl --vacuum-time=14d`; and
- desktop Trash via `gio trash --empty`, with a bounded user-Trash fallback.

Symlinks are unlinked rather than followed. The helper never accepts a path or
command from room text, and privileged work remains behind the desktop's
`pkexec` policy and authentication prompt. Merely entering the Cleaning Room
or opening Housekeeping removes nothing.

## Game Room

The Game Room reads Kilix 95's own `games.GAMES` registry without modifying
its storage, prepends Minesweeper and Solitaire, and polls its source once per
second. Each current title becomes one generated CD case, floppy disk, or
manual carrying that title's original Kilix 95 pixel icon. Hover reveals the
full title; click launches it in a fresh authenticated Kilix terminal tab.

By default the checkout is the `kilix-95` sibling of this project inside
`kilix-desktops`.
`KILIX95_PROJECT_HOME` can select another absolute checkout. Catalog discovery
is read-only. A selected game's normal Kilix 95 installer may download and
persist its pinned content after that explicit launch.
