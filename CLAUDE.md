# FrobTADS (Asciiverse packaging)

FrobTADS, the TADS 2 and TADS 3 interpreter (`frob`), packaged for the
Asciiverse catalog (kind `program`, an interpreter for the Text adventures
category) and as system packages. Story titles such as I Am Prey bundle
this build with their story file. Not our code: keep changes small and to
build or platform fixes (the TADS licence allows nothing else), list every
one in PATCHES.md.

- Remotes: `upstream` is github.com/realnc/frobtads, `origin` is
  github.com/theasciiverse/frobtads.
- Branch `asciiverse` = upstream's v2.0 tag plus our commits.
- `asciiverse/build-frobtads.sh` is self-contained: it clones upstream's
  v2.0 and applies our code commits, which it carries as diffs. A new code
  commit here must also go into the script; the release workflow fails if
  the two differ. Story repos copy the script as is.
- Licence: free of charge only, never sold or bundled with anything sold
  (PATCHES.md, "Licence"). Keep FrobTADS out of paid titles.
- Release: tag `v<upstream version>-av.<n>` and push it.
- Local static build: `docker run --rm -v "$PWD":/src -w /src alpine:3.22
  sh asciiverse/build-frobtads.sh` leaves `build/frob`.
- Play: `frob <story file>`; `frob -d <story file>` keeps saves in the
  current directory (our fix for TADS 3). At a `[More]` prompt, Space
  shows the next page and Enter the next line.
