# ADR — Modello di permessi CRUD sulla collection `users`

> Template minimo. Uno per ogni **arco di decisione** del DAG di progetto (`docs/piano-sviluppo/piano.yaml`) — passo standard, non facoltativo (vedi `00-come-eseguire-il-piano.md`, Passo 0). Vale anche per una scelta che resta dentro gli invarianti standard (`auth/`, `email/`, `stack/`), purché condizioni comunque più fasi a valle.

**Stato**: proposta
**Data**: 2026-09-20
**Arco di decisione**: Fase 2 (`fase-2-login.md` §2.1, §2.10, collection `users`) → Fase 4+ (fuori template, ogni sviluppo che tocchi CRUD su `users` nelle sezioni App reali)

## Contesto

`ADR-001-schema-ruoli-baseline.md` fissa i campi `adminRole`/`appRole` ma non dice chi può creare, modificare o eliminare un record `users`, né se `adminRole` e `loginMethod: locale` possano coesistere sullo stesso record. In assenza di questa decisione, uno schema tecnicamente corretto può comunque produrre stati incoerenti: un account Admin con password locale (mentre l'invariante generale prevede Admin solo-SSO, `auth/01-autenticazione-invarianti.mdc`), un admin che elimina se stesso lasciando una sessione valida su un record inesistente, un admin non-super che promuove un altro utente a super-admin. Nessuno di questi casi è impedito nativamente da Payload: richiedono funzioni `access` esplicite.

## Decisione

1. Un utente con `adminRole ≠ none` non può avere `loginMethod: locale` — validazione bloccante in creazione/modifica, non convenzione lato UI.
2. `access.create`: un admin (non super) non può assegnare `adminRole: super-admin` al nuovo record.
3. `access.update`: ogni utente può modificare il proprio record; un record con `adminRole: super-admin` è modificabile solo da un attore super-admin.
4. `access.delete`: nessun utente può eliminare se stesso, indipendentemente dal ruolo; un record con `adminRole: super-admin` è eliminabile solo da un attore super-admin, e mai se è l'ultimo super-admin rimasto (guardrail già esistente, §2.7/§2.8 — non sostituito da questa regola, resta necessario in aggiunta).
5. `active` ed `emailVerified` (quando presente) hanno `default: false`: nessuno stato di accesso concesso implicitamente alla creazione.

## Alternative considerate

- Permettere ad `adminRole ≠ none` di avere anche `loginMethod: locale` — scartata: contraddirebbe l'invariante generale (Admin solo SSO, form locale riservato al solo bootstrap §2.7) mantenendo due percorsi di autenticazione paralleli per la stessa area senza necessità dichiarata.
- Permettere il self-delete quando esiste un secondo super-admin — scartata: introduce uno stato di sessione incoerente (token valido su un record cancellato) per un guadagno minimo; l'eliminazione del proprio account, se mai richiesta, è un flusso a parte fuori scope di Fase 2.
- Vietare anche il self-update — scartata: bloccherebbe correzioni banali (es. proprio nome) senza un percorso alternativo; nessun requisito osservato lo giustifica.

## Conseguenze

Le funzioni `access.create`/`access.update`/`access.delete` sulla collection `users` diventano funzioni con logica di ruolo (possono restituire una query di vincolo, non solo `true`/`false`), non semplici booleani — pattern di riferimento in `payload-pattern/04-auth-locale-con-sso-esclusivo.mdc`. Il guardrail esistente "non eliminare l'ultimo super-admin" (§2.7/§2.8) resta necessario e distinto dalla regola di self-delete: quest'ultima da sola non impedisce a un super-admin di eliminare l'unico altro super-admin rimasto. `active`/`emailVerified` a `default: false` richiede che il form di creazione Admin li mostri sempre come selezione esplicita — impatta la UI Admin, non solo lo schema. Fase 4+ che introduce nuovi valori di `appRole` eredita questa matrice per la parte `adminRole`; non si estende automaticamente a nuovi ruoli App senza una decisione di progetto dedicata.
