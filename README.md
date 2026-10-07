# seekpack (.skp) — the archive that seeks instead of dragging

Lossless multi-file archive for **backups and datasets**, for Windows and Linux.
One idea, carried all the way through: **never read what you don't need.**

Extracting the last file out of 600 archived: solid single-stream 7z takes
**8.88s**, `skp` **0.099s** — same machine, hashes verified on both
sides ([video](https://youtu.be/FrA3Ury5NHM)). Truncated archive: 7z dies,
seekpack recovers from the footer ([video](https://youtu.be/_COR5A38Ja0)).

![Tail-file extraction: 7z 8.88s vs seekpack 0.099s, both hash-verified](assets/seekpack-vs-7z.png)

* Seekable chunks + content-defined dedup: extraction cost stays
  flat as the archive grows, not linear.
* Index written twice (header + footer): survives truncated heads
  and interrupted downloads.
* Strong per-chunk verification (xxh3-64 + blake3): one flipped bit names
  the guilty chunk instead of silently poisoning the output.
* 100% lossless, byte-identical deterministic output.
* Speaks other formats too: opens `.zip` (native), `.tar.gz`/`.tar`
  (streaming) and `.rar` (via installed 7z or unrar) — and converts them
  to `.skp` with one command, bringing seek, dedup and verification to
  your legacy archives as well.

*Leggimi in italiano: [README.it.md](README.it.md).*

## Download and use (2 minutes)

Grab `skp.exe` (Windows) or `skp` (Linux) from the Releases section:

```
skp a backup.skp --level 3 document.pdf photos/
skp l backup.skp
skp x backup.skp document.pdf -o out.pdf
skp l old.zip
skp x old.zip inner.pdf -o out.pdf
skp c new.skp old.zip --level 3
skp --version
```

Full details in `USERGUIDE.md`. License in `EULA.txt`.

## Editions

* **Free** (this one): gratis, including commercial use. Creation limited to
  200 files / 2GB total and level ≤3. Listing and extraction always unlimited.
* **Trial**: every install starts with Pro features for 30 days,
  then decays to Free on its own. Archives stay readable forever.
* **Pro** (€39 one-time): no limits + MT, parity, encryption,
  scheduler, invoice and support. [Pro page — link]

## License and third parties

Use governed by `EULA.txt` (proprietary). Third-party components in
`THIRD_PARTY_NOTICES.txt` (zstd, xxHash, BLAKE3, zlib).
