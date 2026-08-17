# cursor-payload-template

Catalogo template dei documenti di pianificazione (`docs/piano-sviluppo/`) per progetti Next.js + PayloadCMS con architettura a 4 aree. Complementare a **`cursor-rules`** (che contiene le regole `.mdc` per l'agente): questo repository contiene invece i documenti che umano e agente leggono/eseguono insieme, una sottofase alla volta.

Questo repository **non è l'istanza di un progetto**: da qui si copiano in `docs/piano-sviluppo/` del progetto reale solo i file pertinenti alle varianti scelte per quel progetto — le stesse scelte fatte in `cursor-rules`.

Origine: estratto e generalizzato dal progetto Event Manager (sessione di analisi 2026-08-16).

## Struttura

```
README.md
00-piano-generale.md              indice di progetto — placeholder da compilare
00-come-eseguire-il-piano.md      procedimento operativo per condurre le sessioni (Passo 0 incluso)
CHANGELOG.md                      scheletro vuoto formato Keep a Changelog — da rinominare/spostare in docs/piano-sviluppo/ nel progetto reale

fase-1-setup.md                   Fase 1 — Setup, master (provider/stack-agnostico)
fase-1-db-mongodb.md              variante database

fase-2-login.md                   Fase 2 — Login, master
fase-2-auth-google-oauth.md       variante provider auth
fase-2-email-resend.md            variante provider email

fase-3-deploy.md                  Fase 3 — Deploy, master
fase-3-db-mongodb.md              variante database
fase-3-cloud-gcp.md               variante ambiente cloud
fase-3-auth-google-oauth.md       variante provider auth (produzione)

.gitignore
```

**Fase 4 in poi non è inclusa**: è dominio applicativo specifico di ogni progetto, non c'è contenuto da generalizzare — solo la *forma* di un file di fase è riusabile (vedi la sezione dedicata in `00-piano-generale.md`).

## Convenzione di naming

- `fase-N-<componente>.md` è il **master** della fase N: provider/stack-agnostico, sempre incluso.
- `fase-N-<asse>-<opzione>.md` è una **variante**: se ne include una sola per asse, per fase, nel progetto reale.
- A differenza di `cursor-rules`, qui **non ci sono cartelle**: la struttura è piatta perché rispecchia esattamente cosa finirà in `docs/piano-sviluppo/` del progetto reale (anch'esso piatto) — copiare un file non richiede alcuna riorganizzazione. Il raggruppamento è dato dal prefisso `fase-N-`, non da una cartella.

## Come comporre `docs/piano-sviluppo/` per un nuovo progetto

Procedura completa in `00-come-eseguire-il-piano.md` (Passo 0). In sintesi:

1. Copiare sempre: `00-piano-generale.md`, `00-come-eseguire-il-piano.md`, `CHANGELOG.md` (vuoto, pronto all'uso), `fase-1-setup.md`, `fase-2-login.md`, `fase-3-deploy.md`.
2. Copiare, in base alle varianti scelte per il progetto (le stesse scelte in `cursor-rules`): un file `fase-1-db-*`, un file `fase-2-auth-*`, un file `fase-2-email-*`, un file `fase-3-db-*`, un file `fase-3-cloud-*`, un file `fase-3-auth-*`.
3. Compilare in `00-piano-generale.md` i placeholder: nome progetto, sezione "Varianti adottate in questo progetto".
4. Da Fase 4 in poi, scrivere da zero seguendo la stessa forma (struttura di sottofase) usata in Fase 1-3.

## Catalogo varianti — stato attuale

| Fase | Asse | Variante | File | Stato | Ultima verifica su progetto reale |
|---|---|---|---|---|---|
| 1 | Database | MongoDB | `fase-1-db-mongodb.md` | ✅ pronta | Event Manager (2026-08) |
| 1 | Database | PostgreSQL | `fase-1-db-postgres.md` | 🔲 da scrivere quando servirà | — |
| 2 | Auth | Google OAuth | `fase-2-auth-google-oauth.md` | ✅ pronta | Event Manager (2026-08) |
| 2 | Auth | Altro provider | `fase-2-auth-*.md` | 🔲 da scrivere quando servirà | — |
| 2 | Email | Resend | `fase-2-email-resend.md` | ✅ pronta | Event Manager (2026-08) |
| 2 | Email | Altro provider | `fase-2-email-*.md` | 🔲 da scrivere quando servirà | — |
| 3 | Database | MongoDB | `fase-3-db-mongodb.md` | ✅ pronta | Event Manager (2026-08) |
| 3 | Cloud | Google Cloud Run | `fase-3-cloud-gcp.md` | ✅ pronta | Event Manager (2026-08) |
| 3 | Cloud | Azure / AWS | `fase-3-cloud-*.md` | 🔲 da scrivere quando servirà | — |
| 3 | Auth (produzione) | Google OAuth | `fase-3-auth-google-oauth.md` | ✅ pronta | Event Manager (2026-08) |

> **Nota**: "Event Manager (2026-08)" è il progetto di origine da cui questi template sono stati estratti, non un riuso successivo. Aggiornare con progetto e data nuovi quando una variante viene effettivamente riusata in un progetto successivo.

## Come aggiungere una nuova variante — checklist

- Stesso principio di proporzionalità di `cursor-rules`: scrivere una nuova variante solo quando la richiede un progetto reale in corso, non per completezza.
- Verificare prima che la variante corrispondente esista già in `cursor-rules` (o scriverla insieme, se manca — le due sono sempre in coppia).
- Seguire la struttura del file master della fase corrispondente: stesso livello di dettaglio, stesso stile (checklist per l'umano / per l'agente dove pertinente).
- Se la nuova variante introduce un concetto non ancora previsto nel master (è già successo con "email" durante l'analisi di Fase 2), verificare se il master ha bisogno di un piccolo aggiustamento di terminologia per restare genuinamente agnostico.
- Aggiornare la tabella sopra con stato ✅ e data.

## Decisioni prese e perché (log sintetico)

- **Struttura piatta**, non a cartelle come `cursor-rules`: rispecchia esattamente cosa finirà in `docs/piano-sviluppo/` del progetto reale — copiare i file non richiede riorganizzazione. *(2026-08-16)*
- **Master ridotti a obiettivo + checklist di chiusura generica + puntatore** per ogni sottofase "a variante": restano stabili indipendentemente da quante varianti si aggiungono nel tempo, invece di crescere ad ogni nuovo motore/provider aggiunto. *(2026-08-16)*
- **Fase 4 in poi esplicitamente fuori template**: dominio applicativo specifico del progetto, nessun contenuto generalizzabile — solo la forma è riusabile. *(2026-08-16)*
- **Containerizzazione (Dockerfile/Node LTS) riconosciuta come standard fisso indipendente dal cloud**, durante l'analisi di Fase 3: la scelta vive in `cursor-rules` (`stack/01-stile-codice.mdc`), qui si riflette nel fatto che `fase-3-deploy.md` § 3.2 Parte A (build) è generico, non parte di nessuna variante cloud. *(2026-08-16)*
- **Corretto un refuso** in `fase-3-deploy.md` § 3.5 (rimandava a Fase 2 § 2.9 invece che § 2.10 per la verifica runtime di `logout`/`accessDenied` in `activityLog`) e **aggiunta la colonna "ultima verifica su progetto reale"** alla tabella catalogo varianti, per distinguere variante scritta da variante collaudata — entrambe emerse da una revisione esterna (Cursor Composer). Aggiunto anche `CHANGELOG.md` come scheletro vuoto (pura struttura Keep a Changelog, nessun contenuto di dominio, coerente con la decisione che le specifiche restano da scrivere da zero). *(2026-08-17)*
- **Non aggiunta una bozza di `specifica-login-payloadcms.md`** (proposta da una revisione esterna): quasi tutto il contenuto della specifica originale di Event Manager è già assorbito in `auth/`, `fase-2-login.md` e `fase-2-auth-google-oauth.md` — ricrearne uno scheletro rischierebbe di duplicare le stesse decisioni in due posti. Aggiunta invece una riga in testa a `fase-2-login.md`: una specifica di progetto dedicata va scritta solo per le deviazioni dallo standard, non per ripeterlo; se il progetto non devia, non serve alcuna specifica auth. *(2026-08-17)*
- **Passata di coerenza terminologica** (segnalata da una revisione esterna, seconda verifica): tre riferimenti residui presentavano ancora la specifica auth come riferimento presupposto/obbligatorio, in contraddizione con la nota appena aggiunta — corretti con formulazione condizionale in `fase-1-setup.md` (l'architettura vive in `payload-pattern/01-architettura.mdc`, non nella specifica), `fase-2-login.md` (blockquote iniziale), `fase-3-deploy.md` (spike cookie già coperto qui) e `00-piano-generale.md` (le specifiche servono solo per le deviazioni, non come riferimento sempre presente). *(2026-08-17)*

## Repository collegato

**`cursor-rules`** — contiene le regole `.mdc` corrispondenti (`auth/`, `email/`, `stack/` per le stesse varianti elencate qui). Le varianti scelte per un progetto devono essere le stesse in entrambi i repository — non ha senso, ad esempio, scegliere Google OAuth in un repository e un altro provider nell'altro.
