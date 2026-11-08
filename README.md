# Codici di attivazione di LostTV

`licenses.json` è l'elenco dei codici che possono usare LostTV (TV, telefono e Mac).
Le app lo leggono all'avvio e ogni 6 ore. Qui c'è solo l'**impronta** (SHA-256) di ogni codice,
mai il codice vero: chi legge questo file non può ricavarne un codice.

```json
{ "name": "Marco", "hash": "…", "expires": "2026-12-31", "active": true }
```

- `name`: chi lo usa (lo vedi solo tu).
- `expires`: ultimo giorno valido (`AAAA-MM-GG`), oppure `null` per nessuna scadenza.
- `active`: `false` per spegnerlo subito.

## Dare l'app a un amico

1. Sul Mac, nel Terminale, crea un codice nuovo:
   ```
   C="LTV-$(LC_ALL=C tr -dc 'ABCDEFGHJKLMNPQRSTUVWXYZ23456789' </dev/urandom | head -c 12 | sed 's/..../&-/g;s/-$//')"; echo "Codice: $C"; printf %s "$C" | shasum -a 256 | cut -d' ' -f1
   ```
   La prima riga è il **codice** da dare all'amico, la seconda è l'**impronta** da mettere qui.
2. Su GitHub apri questo file (`licenses.json`), premi la matita ✏️, aggiungi una riga
   con il nome, l'impronta e la scadenza, poi **Commit changes**.
3. Manda all'amico l'app (APK o DMG) e il codice.

## Spegnere o prolungare un codice

Apri `licenses.json` su GitHub, cambia `"active": false` (spento) oppure la data di `expires`,
e salva. Entro qualche minuto vale per tutti i dispositivi già collegati a internet; un
dispositivo senza internet si blocca al massimo dopo 3 giorni.

## Importante

- Non cancellare questo repository né `licenses.json`, e lascialo **pubblico**: le app leggono l'elenco da qui.
  Per spostarlo altrove imposta su GitHub la variabile `LICENSE_URL` (Settings → Secrets and
  variables → Actions → Variables) e ricompila le app.
- I codici proteggono solo l'app già pronta. Chi ha il codice sorgente può togliere il controllo.
