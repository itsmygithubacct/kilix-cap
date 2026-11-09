# kilix-cap

kilix-cap 3.0.0 is a full graphical desktop provider, alongside Kilix 95, Kilix
TUI, and Kilix Land, for terminals that support the Kitty graphics protocol.
It renders a true-RGB 480×320 logical canvas and presents it at an integer scale
so the room art, physical objects, and bitmap type remain crisp.

**Status:** an optional Kilix desktop, installed on first use by Kilix.
Kilix pins this repository (`KILIX_CAP_DEFAULT_REF=84906fa6` in its
`scripts/install-kilix-cap.sh`), clones it to
`~/.local/gpu_terminal/sources/kilix-desktops/kilix-cap` the first time
`kilix cap` runs, and builds it there. Plebian-OS 0.2.3 inherits that pin
through Kilix rather than pinning it separately. The pinned commit is on the
`release/0.2.2-coordinated` branch; `main`, the default branch documented here,
does not contain its three laptop-lifecycle commits and has one later test
commit.

The mansion has eight connected, full-color rooms:

- **Study** — thirteen physical desk props launch their mapped desktop apps
  immediately. No prop opens a built-in textual list first. An open laptop
  on the desk's front-left corner opens configured Kilix terminal sessions
  and other desktop providers from user-editable laptop profiles
  (`docs/LAPTOP.md`).
- **Grand Gallery** — an elegant perspective corridor with six side doors and
  a glazed Balcony entrance at the far end.
- **Storeroom** — movable boxes and containers can be dragged between two
  shelves.
- **Server Room** — two live monitors and four administration consoles sit
  among racks, copper pipes, patch cables, and status lights.
- **Game Room** — the live Kilix 95 catalog appears as physical CDs, floppies,
  and manuals; selecting one starts that title in a new Kilix tab.
- **Library** — five physical volumes open the real project documents in the
  system viewer.
- **Cleaning Room** — the basin, copper bin, cache drawers, and maintenance
  terminal open a bounded Housekeeping TUI.
- **Balcony** — the Gallery's end door opens onto a panoramic exterior with
  direct Weather and stargazing launchers.

Every room begins with a committed generated background image. Doors are
generated as edits of their respective rooms and remain native to those
plates. Dynamic Study props, Storeroom items, and Game Room media stay in
separate layers where their state needs to change. Baked physical consoles,
books, instruments, and doors use explicit object-sized hit regions and
identify themselves in the name bar without painting button boxes over the
room.

## What it does

- Draws a mansion of eight generated rooms on a 480×320 true-RGB canvas and
  presents it at an integer scale through the Kitty graphics protocol.
- Launches real desktop apps from physical objects: Study props, Server Room
  consoles, Library books and Balcony instruments, with fixed argument
  vectors and no shell.
- Opens configured Kilix terminal sessions and other desktop providers from
  the Study laptop's profiles ([docs/LAPTOP.md](docs/LAPTOP.md)).
- Shows the Kilix 95 games catalog as CDs, floppies and manuals in the Game
  Room and starts each title in a new Kilix tab.
- Runs read-only system monitors and a confirm-twice Housekeeping TUI in fresh
  Kilix tabs.
- Plays optional audio cues, and makes no network request of its own.

## Watch the episode

https://github.com/user-attachments/assets/473b28b3-2e7a-4062-83b0-06c3a349112e

**[Desktop Two: Kilix Cap](https://github.com/itsmygithubacct/kilix-cap/releases/download/media-v1/06-kilix-cap.mp4)**
— part six of *Kilix, Pleb, and Plebian-OS: A Desktop Built Inside a Terminal*, the ten-part
stack series. The inline player is a 720p preview; the link is the full-quality file (1920×1080,
3m09s, 15 MB; published as a
[media release](https://github.com/itsmygithubacct/kilix-cap/releases/tag/media-v1) so a clone
stays small). The [full series](https://github.com/itsmygithubacct/plebian-os#watch-the-series) (31m22s)
lives on `plebian-os` and plays at [plebian-os.com](https://plebian-os.com/#watch).

## Build and run

Kilix Cap currently targets Linux desktop systems. The renderer and input core
use C11 and POSIX interfaces; direct app launching, authenticated Kilix tabs,
system monitors, and housekeeping additionally integrate with Linux facilities
such as `/proc`, XDG desktop tools, `pkexec`, and optional APT/systemd
utilities. Missing optional desktop utilities disable only their mapped
interaction and produce a concise status message.

The build requires a C11 compiler, POSIX threads, zlib, libm, Python 3, and
Python's standard `curses` module. It downloads nothing.

Clone it with its submodule (`git clone --recursive
https://github.com/itsmygithubacct/kilix-cap.git`, or
`git submodule update --init --recursive` in an existing clone), then:

~~~sh
make
./bin/kilix-cap
~~~

Run it in Kitty, Kilix, Ghostty, WezTerm, or another terminal implementing the
Kitty graphics protocol. With no modal surface open, `q` or Escape exits.

Inside Kilix, any of these selects the desktop:

~~~sh
kilix cap
kilix desktop kilix-cap
KILIX_DESKTOP_PROVIDER=cap kilix desktop
~~~

Kilix owns the immutable first-install source pin, builds a missing checkout,
and launches the resulting executable through its native `cap` provider path.
Kilix Cap is therefore part of the selected Kilix dependency closure, but it is
not one of the four independently tagged Plebian-OS release-core repositories.

The bottom strip is a direct **house map**: Study, Gallery, Store, Server,
Games, and Clean navigate immediately; Lamp toggles the room-light accent.
The Gallery doors provide the spatial mansion route, including Library and
Balcony.

Optional audio uses `pacat`, `pw-play`, `aplay`, or SoX `play` when available:

~~~sh
KILIX_CAP_AUDIO=0 ./bin/kilix-cap
~~~

External launches can likewise be disabled:

~~~sh
KILIX_CAP_EXTERNAL_APPS=0 ./bin/kilix-cap
~~~

When launching is disabled or an app is unavailable, the room remains in
place and the concise reason appears temporarily in the name bar.

## Usage

| Input or command | Effect |
| --- | --- |
| click an object | launch its mapped app, open a door, or pick up a Storeroom item |
| house map strip | Study, Gallery, Store, Server, Games and Clean navigate directly; Lamp toggles the light accent |
| `q` or Escape | exit, when no modal surface is open |
| `KILIX_CAP_AUDIO=0` | disable audio |
| `KILIX_CAP_EXTERNAL_APPS=0` | disable external launches |
| `KILIX_CAP_CONFIG_HOME=/abs/dir` | relocate the private config (Mail target, `web_home=`) |
| `KILIX95_PROJECT_HOME=/abs/dir` | read the Game Room catalog from another Kilix 95 checkout |
| `./bin/kilix-cap --help` | list the headless self-tests (`--interaction-test`, `--render-test DIR`, ...) |
| `make test` / `make sanitize` | the test gate / sanitizer build |

[docs/rooms.md](docs/rooms.md) lists what each room's objects do, and
[docs/INTERACTIONS.md](docs/INTERACTIONS.md) is the full interaction guide.

## How it works

```text
 kilix cap ──▶ Kilix clones the pinned kilix-cap, runs make, starts bin/kilix-cap
                    │
 bin/kilix-cap (C11): rooms + objects + scenes ──▶ 480x320 RGB canvas
     │  terminal lifecycle, Kitty graphics and keyboard, raster, PCM mixing
     │  (kilix-game-sdk submodule, built from source)
     ├──▶ Kitty graphics frames ──▶ terminal pane
     ├──▶ posix_spawn fixed argv ──▶ desktop apps, authenticated Kilix tabs
     └──▶ tools/mansion_tui.py ──▶ monitor and Housekeeping TUIs in Kilix tabs
```

The executable is a C11 program with POSIX threads, zlib and libm. Each room is
a committed background plate with object-sized hit regions; dynamic props,
Storeroom items and Game Room media are separate layers. Input arrives as
Kitty keyboard and SGR mouse events; output is presented as Kitty graphics
frames with 30 Hz pacing. The terminal, graphics, rasterizer and audio layers come from
the [kilix-game-sdk](https://github.com/itsmygithubacct/kilix-game-sdk#readme)
submodule. Launches go through one launcher that passes fixed argument
vectors to `posix_spawn`; anything privileged stays behind `pkexec`.
[docs/ENGINE.md](docs/ENGINE.md) records the engine invariants and
[docs/provenance.md](docs/provenance.md) the runtime boundary and art
provenance.

## Documentation

| Document | Contents |
| --- | --- |
| [docs/rooms.md](docs/rooms.md) | direct app behavior, Housekeeping safety, the Game Room |
| [docs/INTERACTIONS.md](docs/INTERACTIONS.md) | mansion flow, props, breaker, monitors, pointer and keyboard |
| [docs/APPS.md](docs/APPS.md) | app and system-tool launch vectors and candidate order |
| [docs/LAPTOP.md](docs/LAPTOP.md) | the Study laptop and its profiles |
| [docs/ENGINE.md](docs/ENGINE.md) | engine contract: canvas, objects, scenes, launcher, audio, gates |
| [docs/testing.md](docs/testing.md) | test targets and headless checks |
| [docs/provenance.md](docs/provenance.md) | runtime boundary and visual provenance |
| [docs/THIRD_PARTY.md](docs/THIRD_PARTY.md) | vendored code, generation-time tooling, optional external programs |
| [COPYING-ASSETS.md](COPYING-ASSETS.md) | asset licence scope and exceptions |

## Related repositories

- [Kilix](https://github.com/itsmygithubacct/kilix#readme): the host that pins,
  builds and launches this desktop (`kilix cap`).
- [kilix-game-sdk](https://github.com/itsmygithubacct/kilix-game-sdk#readme):
  the runtime, rendering and audio libraries, as the `third_party/kilix-game-sdk`
  submodule.
- [Kilix 95](https://github.com/itsmygithubacct/kilix-95#readme): the games
  registry the Game Room reads.
- [kilix-desktop-sdk](https://github.com/itsmygithubacct/kilix-desktop-sdk#readme):
  the desktop parent repository that carries a mirrored copy of this provider.
- Other Kilix desktops: [Kilix Land](https://github.com/itsmygithubacct/kilix-land-desktop#readme),
  [Kilix IceWM](https://github.com/itsmygithubacct/kilix-icewm#readme) and the
  Kilix TUI desktop in [kilix-tui-utils](https://github.com/itsmygithubacct/kilix-tui-utils#readme).

## Part of Plebian-OS

Kilix Cap is one of the repositories behind
[Plebian-OS](https://github.com/itsmygithubacct/plebian-os#readme), a Debian
install whose desktop is [Kilix](https://github.com/itsmygithubacct/kilix#readme)
running the [Kilix 95](https://github.com/itsmygithubacct/kilix-95#readme)
desktop through [Pleb](https://github.com/itsmygithubacct/pleb#readme).
Kilix installs and builds it on first use of `kilix cap`, at a commit pinned in
Kilix. The map of every repository and how it ships is on the
[components page](https://github.com/itsmygithubacct/plebian-os/blob/main/docs/components.md).

## License

Kilix Cap's code, documentation, generated visual pixels, project-authored
visual transformations, and rendered audio outputs are available under the
[MIT License](LICENSE) to the extent contributors hold licensable rights in
them. Identified third-party code, CC0/public-domain source recordings, and
provider-issued C2PA metadata retain their own notices or legal status. The
precise asset scope and exceptions are documented in
[COPYING-ASSETS.md](COPYING-ASSETS.md), and vendored notices are indexed in
[docs/THIRD_PARTY.md](docs/THIRD_PARTY.md).
