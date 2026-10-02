# Spese 2026 — regole per chi modifica l'app

L'app è un solo file, `index.html`, pubblicato su GitHub Pages dal branch `main`.
I dati dell'utente esistono SOLO nel localStorage del suo telefono: non c'è un server
e non c'è un'altra copia. Una modifica sbagliata li cancella per sempre.

## Regole sui dati (obbligatorie)

1. **Non cambiare mai la chiave `spese2026` né il formato salvato.** Si possono solo
   AGGIUNGERE campi. Rinominare, spostare o eliminare un campo è vietato.
   Se un nuovo formato è indispensabile, `loadState()` deve continuare a leggere tutti
   i formati precedenti (vedi la migrazione da `spese2026b`).
2. **Non scartare mai ciò che non si conosce.** `loadState()` conserva in `S.extra`, nei
   mesi e in `cats` tutti i campi e le categorie sconosciuti, e `persist()` li riscrive.
   Rimuovere una categoria da `CATEGORIES` non deve far sparire le sue voci.
3. **Non toccare le chiavi di backup** `spese2026-backup-*`, `spese2026b`,
   `spese2026-illeggibile`: si possono solo leggere.
4. **Il backup all'avvio (`makeBackup` prima di `loadState`) deve restare la prima cosa
   eseguita**, prima di qualunque scrittura.
5. **Prima di ogni modifica dei dati chiamare `syncFromStorage()`**: con più finestre
   aperte, una copia vecchia non deve mai sovrascrivere dati più nuovi.

## Prima di pubblicare su `main`

- Salvare con la versione ATTUALMENTE su `main` un insieme di dati realistico
  (saldo iniziale, entrate e voci in più mesi), poi caricare la nuova versione sulla
  stessa origine e verificare che tutti i valori siano ancora presenti e uguali.
- Verificare che nessun errore JavaScript compaia (pageerror) e che il file non contenga
  `<\/script>` o altri escape rotti.
- Chromium: `/opt/pw-browsers/chromium`; playwright-core:
  `/opt/node22/lib/node_modules/playwright/node_modules/playwright-core/index.mjs`.
