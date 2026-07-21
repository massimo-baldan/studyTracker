# Study Tracker 📓

Mini app per tenere traccia delle ore di studio tue e di un amico, confrontarvi e
darvi un po' di sana competizione. Un solo file HTML, pensato per il telefono,
ospitabile gratis su GitHub Pages.

Non serve installare nulla: è una sola pagina (`index.html`) che gira nel browser.

---

## Come funziona (in breve)

- Non c'è un vero "login": scegli semplicemente il tuo nome dal selettore in alto,
  e da quel momento i dati che inserisci sono i tuoi. Non è una password, è solo
  un modo per dire all'app "sto scrivendo i dati di Marco" oppure "di Luca".
- I dati (ore, pagine, esercizi, obiettivi, nomi) sono salvati in un unico "contenitore"
  JSON online tramite **JSONBin.io**, un servizio gratuito di storage JSON. Così,
  quando tu registri un'ora di studio dal tuo telefono, il tuo amico la vede
  aggiornarsi anche sul suo (l'app si aggiorna da sola ogni 25 secondi, e anche
  ogni volta che riapri l'app).
- Finché non configuri JSONBin (vedi sotto), l'app funziona comunque in
  **modalità locale**: salva tutto solo sul telefono che stai usando, senza
  sincronizzare nulla. Utile per provarla subito, ma per confrontarvi in due
  dovete completare la configurazione.

---

## Passo 1 — Crea il tuo storage JSON gratuito (5 minuti)

1. Vai su **https://jsonbin.io** e crea un account gratuito (email + password, o Google).
2. Una volta dentro, vai alla sezione **API Keys** del tuo account e copia la
   tua **X-Master-Key** (una lunga stringa). Tienila da parte.
3. Crea un nuovo Bin (**Create Bin** / **+ Create**) e incolla questo contenuto
   iniziale esatto:

   ```json
   {
     "config": {
       "names": { "user1": "Amico 1", "user2": "Amico 2" },
       "goals": {
         "user1": { "hours": 0, "pages": 0, "exercises": 0 },
         "user2": { "hours": 0, "pages": 0, "exercises": 0 }
       }
     },
     "entries": {}
   }
   ```

4. Salva il bin e copia il suo **Bin ID** (lo trovi nell'URL del bin o nella pagina
   dei dettagli, è una stringa tipo `64f2a1...`).

## Passo 2 — Incolla le due chiavi nel file

Apri `index.html` con un editor di testo qualsiasi (anche il Blocco Note, o
direttamente su GitHub cliccando sulla matita ✏️ per modificarlo) e cerca queste
righe vicino all'inizio del tag `<script>`:

```javascript
const JSONBIN_CONFIG = {
  BIN_ID: 'INSERISCI_QUI_IL_TUO_BIN_ID',
  API_KEY: 'INSERISCI_QUI_LA_TUA_X_MASTER_KEY'
};
```

Sostituisci i due valori con quelli che hai copiato al passo 1, ad esempio:

```javascript
const JSONBIN_CONFIG = {
  BIN_ID: '64f2a1b3c9e77f001',
  API_KEY: '$2a$10$abcdefghijklmnopqrstuvwxyz1234567890'
};
```

Salva il file. Da questo momento l'app userà JSONBin invece della modalità locale
(l'app se ne accorge da sola: se lasci scritto "INSERISCI_QUI..." resta in locale).

> **Nota sulla sicurezza**: dato che il sito è statico e pubblico, questa chiave
> sarà leggibile da chiunque guardi il codice sorgente della pagina. Per i vostri
> scopi (ore di studio tra due amici, nessun dato sensibile) va benissimo così.
> Se un giorno vuoi essere più prudente, su JSONBin puoi creare una **Access Key**
> con permessi limitati (solo lettura/scrittura su quel bin specifico, niente
> accesso al resto del tuo account) da usare al posto della Master Key.

---

## Passo 3 — Metti tutto su GitHub Pages

1. Vai su **https://github.com** e crea un account gratuito se non ce l'hai già.
2. Crea una nuova repository (pulsante **New**), dalle un nome (es. `quaderno-a-due`),
   e lasciala **pubblica** (con l'account gratuito è necessario che sia pubblica
   per usare Pages — vedi nota sotto).
3. Carica il file `index.html` (quello che hai appena modificato) nella repository:
   trascinalo nella pagina della repo su GitHub ("Add file" → "Upload files"),
   poi clicca **Commit changes**.
4. Vai su **Settings → Pages** (nel menu a sinistra della repo).
5. In "Build and deployment" → "Source", seleziona **Deploy from a branch**,
   scegli il branch `main` e la cartella `/ (root)`, poi salva.
6. Aspetta un paio di minuti: GitHub ti mostrerà un link tipo
   `https://tuonome.github.io/quaderno-a-due/`. Quello è il link da condividere
   con il tuo amico e da salvare come collegamento/icona sulla schermata home
   del telefono (in Safari su iPhone: condividi → "Aggiungi a Home"; su Android/Chrome:
   menu ⋮ → "Aggiungi a schermata Home").

> Con l'account GitHub gratuito la repository deve essere pubblica per poter
> usare Pages (una repo privata richiederebbe un piano Pro a pagamento). Anche
> con un piano a pagamento, il sito pubblicato resterebbe comunque visibile
> pubblicamente a chiunque abbia il link — cambia solo la visibilità del codice
> sorgente nella repo, non della pagina online. Per i vostri scopi non cambia
> nulla di pratico: nessuno troverà il link per caso.

---

## Come si usa

- **Calendario**: tocca il tuo nome in alto per scegliere chi sei, poi tocca un
  giorno per registrare ore, pagine ed esercizi (questi ultimi due facoltativi).
- **Obiettivi**: ognuno imposta il proprio obiettivo settimanale (ore / pagine /
  esercizi). Si applica automaticamente a ogni settimana da lunedì a domenica.
- **Confronto**: qui vedete entrambi fianco a fianco — classifica, barre di
  progresso verso l'obiettivo di ciascuno, quanto manca, e un grafico delle ore
  giorno per giorno. Usate le frecce per guardare le settimane passate.
- **Impostazioni**: cambiate i vostri nomi, controllate se siete sincronizzati,
  o cancellate i vostri dati.

## Limiti da tenere a mente

- La difficoltà degli esercizi è una sola etichetta per giornata (es. "oggi ho
  fatto esercizi di livello medio"), non per singolo esercizio: per un uso tra
  amici è più che sufficiente e mantiene tutto semplice da compilare dal telefono.
- Non c'è un vero sistema di autenticazione: chiunque abbia il link può scegliere
  "chi è" e scrivere dati. Va bene per un progetto tra due persone fidate.
- JSONBin (piano gratuito) ha un limite di richieste generoso ma non infinito;
  per l'uso di due persone che aggiornano dati manualmente non lo raggiungerete mai.
