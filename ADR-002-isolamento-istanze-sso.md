# ADR — Isolamento delle istanze SSO tra Admin e App

> Template minimo. Uno per ogni **arco di decisione** del DAG di progetto (`docs/piano-sviluppo/piano.yaml`) — passo standard, non facoltativo (vedi `00-come-eseguire-il-piano.md`, Passo 0). Vale anche per una scelta che resta dentro gli invarianti standard (`auth/`, `email/`, `stack/`), purché condizioni comunque più fasi a valle.

**Stato**: proposta
**Data**: 2026-08-30
**Arco di decisione**: Fase 2 (`fase-2-login.md` §2.4/§2.5) → Fase 3 (`fase-3-deploy.md` §3.3, credenziali e callback per ambiente di produzione)

## Contesto

Admin e App condividono la stessa collection `users` (§2.1) e lo stesso provider SSO, scelto a Passo 0 (es. Google OAuth via plugin `payload-oauth2`). Il plugin registra una strategia di autenticazione identificata da `strategyName`, `authorizePath` e `callbackPath` (`auth/01a-google-oauth.mdc`). Usare un'unica istanza condivisa per due aree con requisiti di autorizzazione diversi (whitelist `allowAdmin`/`allowApp` distinte, §2.2; sessioni che non devono attraversare le aree) rischia collisioni di route e di sessione sulla stessa collection, non solo un problema di stile.

## Decisione

Due istanze separate del plugin OAuth, una per Admin e una per App, con `strategyName`, `authorizePath` e `callbackPath` distinti.

## Alternative considerate

- Istanza unica condivisa, con distinzione per-area gestita a runtime (es. parametro `state` OAuth o logica custom) — scartata: richiederebbe lavoro custom sopra il plugin scelto, non supportato nativamente; aumenta la superficie di codice non standard invece di ridurla.
- Provider o librerie diverse per Admin e App — scartata: nessun requisito noto giustifica differenziare il provider tra le due aree; aggiungerebbe complessità senza beneficio.

## Conseguenze

In produzione (Fase 3, §3.3) servono due callback URL registrati separatamente sulla console del provider, due set di variabili d'ambiente distinti, sessioni/cookie isolati per area. Il vincolo si propaga a un eventuale cambio futuro di provider SSO (esplicitamente "fuori scope di Fase 2" in `fase-2-login.md`): un nuovo provider dovrà comunque supportare, o essere adattato per supportare, due istanze isolate sulla stessa collection `users`.
