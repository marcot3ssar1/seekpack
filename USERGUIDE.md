# seekpack — guida utente (v0.2.1)

## Comandi

```
skp a out.skp [--level N|--store] [--tmin B] [--tmax B] in...
skp l archive.{skp,zip,tar.gz,tar,rar}
skp x archive.{skp,zip,tar.gz,tar,rar} <nome> [-o out]
skp c out.skp in.{zip,tar.gz,tar,rar} [--level N|--store] [--tmin B] [--tmax B]
skp --version
```

* Input: `nome-interno=percorso` oppure percorso (nome = basename).
  Oltre ~32k caratteri di riga comando: file lista `@lista.lst`
  (una riga `nome=percorso` per file).
* `--level 0`/`--store`: nessuna compressione (comunque con dedup e hash).
  `--level 1..19`: zstd (default 3). Free: max 3.
* `--tmin/--tmax`: finestra chunking (default 256K/4M).

## Esempi

```
skp a backup.skp --level 3 report.pdf foto/
skp l backup.skp
skp x backup.skp report.pdf -o recuperato.pdf
skp x backup.skp report.pdf > recuperato.pdf   # via stdout
```

## Archivi stranieri: leggere e convertire

`skp` apre direttamente i formati più diffusi (lettura sempre libera,
nessun limite Free):

```
skp l vecchio.zip                    # zip nativo: lista veloce
skp x vecchio.zip interno.pdf -o out.pdf
skp l logs.tar.gz                    # tar.gz/tar: scorre in streaming
skp x logs.tar.gz giorno.log -o giorno.log
skp l cliente.rar                    # rar: serve 7z o unrar installati
skp x cliente.rar documento.pdf -o doc.pdf
```

Con `skp c` li converti in `.skp` — da lì in poi hai seek O(1), dedup
a chunk e verifica forte anche sullo storico (la scrittura `.skp`
segue i limiti Free come `skp a`):

```
skp c nuovo.skp vecchio.zip --level 3
skp c logs.skp logs.tar.gz --level 3
```

Note oneste: su `.tar.gz` l'estrazione resta sequenziale (fisica del
gzip) finché non converti; i `.rar` richiedono `7z` o `unrar` nel PATH
(override esplicito con la variabile `SKP_RAR_TOOL`); gli zip criptati,
multi-disco e zip64 non sono supportati.

## Recovery

Header danneggiato? `skp` ricostruisce l'indice dal footer e avvisa:
`warning: header corrupt, recovered from footer`. Chunk danneggiato?
l'estrazione nomina il chunk e fallisce pulito (exit 1); `l` resta
funzionante per salvare il resto.

## Limiti Free (solo creazione; lettura sempre libera)

200 file / 2GB totali / level≤3 per archivio. Oltre: messaggio chiaro con
rimando a Pro. Trial 30 giorni con tutto sbloccato, poi decadimento
automatico — niente chiavi, niente account, niente blocchi dei dati.

## FAQ

* **Su quali OS gira?** Windows x64 e Linux x64. Mac in arrivo.
* **Legge zip/tar.gz/rar?** Sì: zip nativo, tar.gz/tar in streaming,
  rar via 7z o unrar. E li converte in `.skp` con `skp c`.
* **SmartScreen avvisa al primo avvio?** Sì, il binario non è ancora
  firmato con certificato commerciale: verifica lo SHA256 pubblicato
  nella Release e procedi. La firma arriva con le prime vendite.
* **I miei archivi restano leggibili se non rinnovo?** Sì, per sempre:
  il formato non dipende dalla licenza.
