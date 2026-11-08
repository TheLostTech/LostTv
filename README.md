# Codici di attivazione di LostTV

`licenses.json` è l'elenco dei codici che possono usare LostTV (TV, telefono e Mac).
Le app lo leggono all'avvio e ogni 6 ore. Qui c'è solo l'**impronta** (SHA-256) di ogni codice,
mai il codice vero: chi legge questo file non può ricavarne un codice, e solo tu puoi modificarlo.

```json
{ "name": "Marco", "hash": "…", "expires": "2026-12-31", "active": true, "device": "DEV-K3F9-QX2M" }
```

- `name`: chi lo usa (lo vedi solo tu).
- `expires`: ultimo giorno valido (`AAAA-MM-GG`), oppure `null` per nessuna scadenza.
- `active`: `false` per spegnerlo subito.
- `device`: il **codice dispositivo** su cui funziona (ogni TV, telefono o Mac ha il suo, lo mostra l'app).
  `null` = non ancora associato (l'app chiede di mandarti il codice dispositivo); `"*"` = qualsiasi dispositivo.
- Per più dispositivi sullo stesso codice usa `"devices": ["DEV-…", "DEV-…"]` al posto di `device`.

## Dare l'app a un amico

1. Sul Mac, nel Terminale, crea un codice nuovo:
   ```
   C="LTV-$(LC_ALL=C tr -dc 'ABCDEFGHJKLMNPQRSTUVWXYZ23456789' </dev/urandom | head -c 12 | sed 's/..../&-/g;s/-$//')"; echo "Codice: $C"; printf %s "$C" | shasum -a 256 | cut -d' ' -f1
   ```
   La prima riga è il **codice** da dare all'amico, la seconda è l'**impronta** da mettere qui.
2. Su GitHub apri `licenses.json`, premi la matita ✏️, aggiungi una riga con nome, impronta,
   scadenza e `"device": null`, poi **Commit changes**.
3. Manda all'amico l'app (APK o DMG) e il codice. Quando lo inserisce, l'app gli mostra il suo
   **codice dispositivo** (tipo `DEV-K3F9-QX2M`): fattelo mandare e scrivilo in `"device"`.
   L'amico preme **Riprova** e l'app si apre. Da quel momento funziona solo su quel dispositivo.

## Cambiare dispositivo

Sul dispositivo nuovo l'app mostra il nuovo codice dispositivo. Mettilo in `"device"` al posto
del vecchio: il vecchio dispositivo smette di funzionare al controllo successivo.

## Spegnere o prolungare un codice

Cambia `"active": false` (spento) oppure la data di `expires`, e salva. Entro qualche minuto vale
per tutti i dispositivi collegati a internet; un dispositivo senza internet si blocca al massimo dopo 3 giorni.

## Importante

- Lascia questo repository **pubblico** e non cancellare `licenses.json`: le app leggono l'elenco da qui.
- Non accettare *pull request* su questo repository: solo tu devi poterlo modificare.
- Attiva la verifica in due passaggi sul tuo account GitHub.
