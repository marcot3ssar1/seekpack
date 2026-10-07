# seekpack — guida utente (v0.2)

## Comandi

```
skp a out.skp [--level N|--store] [--tmin B] [--tmax B] in...
skp l archive.skp
skp x archive.skp <nome> [-o out]
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
* **Legge zip/7z/rar?** No: importa i file, non gli archivi altrui.
* **SmartScreen avvisa al primo avvio?** Sì, il binario non è ancora
  firmato con certificato commerciale: verifica lo SHA256 pubblicato
  nella Release e procedi. La firma arriva con le prime vendite.
* **I miei archivi restano leggibili se non rinnovo?** Sì, per sempre:
  il formato non dipende dalla licenza.
