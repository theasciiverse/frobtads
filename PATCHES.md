# Changes from upstream

This is the Asciiverse packaging of [FrobTADS](https://github.com/realnc/frobtads),
the TADS 2 and TADS 3 interpreter for Unix terminals by Nikos Chantziaras,
on the TADS virtual machines by Michael J. Roberts. It is built from
upstream's v2.0 tag (commit 934b0e4, May 2021), its latest release.
Upstream is the `upstream` remote; the `asciiverse` branch carries the
changes below.

Upstream's `master` has five later commits (to June 2022: an input log
switch, relative paths for log and replay files, a longer `OSFNMAX`, a
THANKS update). None is needed here, so they are not included.

## Licence

FrobTADS is under the TADS 3 Freeware Source Code License (`doc/COPYING`,
`tads3/LICENSE.TXT`); the TADS 2 VM under the TADS 2 one
(`tads2/LICENSE.TXT`). They allow copying and giving the software away,
provided the licence and copyright notice go with every copy; they forbid
collecting a fee for it, or including it with any software that is sold;
and they allow changes only for porting TADS to a system, with each port
asked to carry its maintainer's contact details. Our changes are build and
platform fixes, and every archive and package carries the licences plus a
NOTICE naming this repository and packages@asciiverse.sh. No SPDX
identifier exists for these licences.

## Code

1. **Build without libcurl when networking is off** (`tads3/unix/osnetunix.h`):
   the header included `<curl/curl.h>` without using it, so
   `ENABLE_TADSNET=OFF` still failed for lack of the curl headers. The curl
   code lives in `osnetunix.cpp`, which includes the header itself and is
   only built with networking.
2. **Keep TADS 3 files in the current directory with `--no-chdir`**
   (`src/frobtadsapp.cc`): the T3VM resolves file names, saved games
   included, against the game's directory unless given a file directory,
   so `-d` changed nothing for TADS 3 games and they still saved next to
   the game file. With `-d`, frob now passes the current directory as the
   VM's file directory, which is also its file safety sandbox. TADS 2 games
   already worked this way. Story titles use this to keep saves outside
   the install folder, which av replaces on updates.

## Build

- No TADS networking (`ENABLE_TADSNET=OFF`), so no libcurl. That loses
  the HTTP client and server functions a TADS 3 game can import
  (`tads-net`), which only games built for the TADS Web UI use; those do
  not run in a terminal interpreter anyway. Console games, I Am Prey
  among them, import no `tads-net` functions.
- No compilers (`tadsc`, `t3make`): only the interpreter, `frob`.
- Charsets: frob defaults to `us-ascii` output; the VM's built-in ASCII
  map turns curly quotes, dashes and accented letters into plain
  equivalents, so no charmap files (`cmaplib.t3r`) are shipped. Non-ASCII
  input does not work (upstream notes Unicode output as unimplemented).

## Packaging (new files)

- `asciiverse/build-frobtads.sh`: clones upstream's v2.0 at its pinned
  commit, applies the two changes above (carried in the script as diffs,
  so story titles can copy the script alone), and builds a fully static
  Linux binary in Alpine, or a macOS binary that links only the system's
  ncurses (the script fails otherwise). Leaves `build/frob`, the licences
  and `build/NOTICE`. The release workflow checks that the patched clone
  is identical to this repository.
- `asciiverse/nfpm.yaml`: .deb, .rpm, .apk and Arch packages named
  `frobtads-asciiverse` (they conflict with a distribution's own
  frobtads). No menu entries.
- `asciiverse/frobtads.cast`: the catalog preview, I Am Prey's opening.
- `.github/workflows/release.yml`: builds all of it on a tag.

## Not built

- Windows: frob's curses interface uses Unix terminal calls (termios,
  signals).
- frobd (the debugger build), tadsc and t3make.

## Ours

- **How to play it, at the top of README.** A short note on installing and
  starting it with av, or from a release archive; the original text follows
  unchanged.
