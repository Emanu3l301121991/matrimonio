# 🎬 Saluti per gli sposi — Alica & Emanuele (28.06.2026)

Booth video per il ricevimento: gli ospiti toccano un pulsante e registrano un saluto
di **massimo 60 secondi**, in **italiano e tedesco**. I video restano salvati **sul dispositivo**.
È una **PWA installabile** che funziona anche **offline**.

## File
```
index.html              ← l'app
manifest.webmanifest    ← per l'installazione
sw.js                   ← cache offline
icons/                  ← icone app
```

## 1) Pubblicazione (serve HTTPS)
La fotocamera e l'installazione funzionano **solo su HTTPS** (o su `localhost`).
Carica la cartella su un host statico — per esempio **GitHub Pages**:

```bash
git init && git add . && git commit -m "booth saluti"
git branch -M main
git remote add origin https://github.com/<utente>/<repo>.git
git push -u origin main
# Settings → Pages → Branch: main /(root) → Save
```

L'app sarà su `https://<utente>.github.io/<repo>/`.
(Va bene qualsiasi hosting statico: Netlify, Vercel, Cloudflare Pages, o un tuo server.)

## 2) Installazione sul dispositivo del booth
Apri l'URL nel browser del tablet/telefono e **aggiungi alla schermata Home**:
- **iPhone/iPad (Safari)**: Condividi → *Aggiungi a Home* → apri dall'icona.
- **Android (Chrome)**: menu ⋮ → *Installa app* / *Aggiungi a Home*.

Aperta dall'icona parte **a tutto schermo, senza barre del browser**.

## 3) Bloccare il dispositivo (modalità chiosco vera)
Una pagina web **non può** impedire da sola di uscire. L'app fa la sua parte
(schermo intero, schermo sempre acceso, niente menu, avviso in uscita), ma per
**bloccare davvero** il tablet su questa app usa la funzione del sistema operativo:

- **iPad/iPhone — Accesso Guidato**
  Impostazioni → Accessibilità → Accesso Guidato → ON (imposta un codice).
  Apri l'app, **triplo clic** sul tasto laterale → *Avvia*. Per uscire: triplo clic + codice.
- **Android — Blocco su schermo (Screen pinning)**
  Impostazioni → Sicurezza → *Blocca su schermo* → ON.
  Apri l'app, tasto *App recenti*, tocca l'icona in alto e *Blocca*. Per sbloccare: gesto + PIN.

Suggerito anche: luminosità fissa, blocco rotazione su verticale, *Non disturbare*.

## 4) Dove finiscono i video e come ritirarli
Ogni saluto viene salvato **sul dispositivo** (storage dell'app, IndexedDB): niente
internet richiesto, nessuna richiesta di permessi per gli ospiti.

Per rivederli / scaricarli c'è una **galleria nascosta** (solo per te):
- **Tieni premuto ~1,5 s nell'angolo in alto a destra** (oppure sulla scritta
  *Campofelice di Roccella*) → inserisci il **PIN `2806`**.
- Da lì: riproduci, scarica i singoli video, oppure **“Scarica tutti (ZIP)”**,
  ed eventualmente *Svuota tutto*.
- Formato: su iPhone/iPad i video sono **.mp4**, su Android/Chrome **.webm**.

> Consiglio: a fine serata entra in galleria e fai **Scarica tutti (ZIP)** per
> avere tutti i saluti in un unico file. Non disinstallare l'app prima di averli
> scaricati, altrimenti lo storage dell'app viene cancellato.

## 5) Personalizzazione veloce
In cima allo `<script>` di `index.html`:
```js
const CONFIG = {
  MAX_SECONDS: 60,      // durata massima
  ADMIN_PIN: "2806",    // ← cambia il PIN della galleria
  VIDEO_BITRATE: 2_500_000,
  FACING: "user"        // "user" = selfie, "environment" = camera posteriore
};
```

## Note
- Funziona offline dopo la prima apertura (service worker).
- Tieni il booth **collegato alla corrente**: la registrazione consuma batteria.
- Prima dell'evento fai una **prova** sul dispositivo reale e con la rete del locale.
