# seekpack beta qualification test (v0.2.1)

## The deal

Pass this test → you are beta tester #N/10 → complete the full beta
report on YOUR archives → PRO free, lifetime (EULA clause 2(d)).

The test proves you actually ran the tool. Everything below is checkable.

## Step 0 — get the tool

https://github.com/marcot3ssar1/seekpack/releases/tag/v0.2.1 —
download `skp.exe` (Windows) or `skp` (Linux), verify SHA256SUMS.txt
(binary unsigned yet — always verify first).

## Step 1 — prove the version

```
skp --version
```

Must print `seekpack 0.2.1`. (0.2.0 does NOT qualify — foreign formats
are new in 0.2.1.)

## Step 2 — the fixture

Download `beta-fixture.zip` from the same release page, verify it:

```
sha256: f3f8b835cd8a78e9692a76b0d6ff1768fdd816cc2f4e67850ce2eb752cb827d2
```

Then convert and post your exact output line:

```
skp c fixture.skp beta-fixture.zip --level 3
```

Expected (files + chunks must match exactly):

```
packed 3 files, 2 chunks (2 unique), 78028 data bytes (codec zstd, level 3)
```

## Step 3 — prove the round-trip

```
skp x fixture.skp alpha.txt -o alpha.out
sha256 of alpha.out must be:
ae0c736c787ed82c07a4873ec9726b4defd92b0f5ad84ed3b5153b71915bfdbd
```

## Step 4 — your turn (seeds the beta report)

Run it on YOUR archive and post: format, size, file count, disk type,
plus one before/after number (e.g. old tool time vs `skp x` time for
one file). Honest numbers only — mismatches are welcome, that's the beta.

## Pass = comment with: version line + packed line + alpha sha + your numbers.

First 10 complete passes get tester numbers. Full beta report afterwards
unlocks the lifetime PRO.
