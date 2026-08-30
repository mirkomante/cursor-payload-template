# ADR — Login locale come opzione standard per l'Area App

> Template minimo. Uno per ogni **arco di decisione** del DAG di progetto (`docs/piano-sviluppo/piano.yaml`) — passo standard, non facoltativo (vedi `00-come-eseguire-il-piano.md`, Passo 0). Vale anche per una scelta che resta dentro gli invarianti standard (`auth/`, `email/`, `stack/`), purché condizioni comunque più fasi a valle.

**Stato**: proposta
**Data**: 2026-08-30
**Arco di decisione**: Fase 2 (`fase-2-login.md` §2.6) → Fase 3 (email transazionali in produzione), Fase 4+ (fuori template, gestione utenti locale/SSO nelle sezioni App)

## Contesto

L'invariante generale (`auth/01-autenticazione-invarianti.mdc`) prevede che le aree protette mostrino solo il pulsante SSO, riservando un form locale al solo super-admin di bootstrap su route nascosta (§2.7). L'Area App, in questo template, devia da questo schema di default: offre il form locale come opzione visibile e paritaria a SSO sulla pagina di login standard, non solo come via di emergenza.

## Decisione

L'Area App supporta due metodi di login equivalenti e visibili — SSO e locale email/password — come default del template, non come opzione da attivare caso per caso a discrezione del progetto.

## Alternative considerate

- Solo SSO anche per l'Area App, simmetrico ad Admin — scartata come default di catalogo: presumibilmente non tutti gli utenti App dispongono di un account compatibile col provider SSO scelto (es. dominio Google Workspace aziendale), a differenza degli utenti Admin (staff interno). **Questa motivazione non è documentata nella specifica originale** — resta un'ipotesi ragionevole, da confermare o correggere per ogni progetto reale, non un fatto accertato.
- Login locale solo su richiesta esplicita di progetto, non default di catalogo — scartata per coerenza con l'obiettivo del template di fornire uno standard riusabile "così com'è" per la maggioranza dei progetti, senza richiedere una decisione ripetuta ad ogni nuovo progetto.

## Conseguenze

Richiede integrazione email transazionale funzionante in produzione (Fase 3) per attivazione e reset password — superficie di attacco aggiuntiva rispetto a un'area solo-SSO (tentativi di password, reset flow) non trattata esplicitamente né qui né negli invarianti generali. Il campo `loginMethod` (§2.1) e il tracking `method` in `activityLog` (§2.9) esistono a causa di questa decisione. Un progetto che non necessita di login locale per l'Area App deve dichiararlo esplicitamente come deviazione a Passo 0, non limitarsi a ometterlo in silenzio.
