# ADR — Schema ruoli baseline (`adminRole` / `appRole`)

> Template minimo. Uno per ogni **arco di decisione** del DAG di progetto (`docs/piano-sviluppo/piano.yaml`) — passo standard, non facoltativo (vedi `00-come-eseguire-il-piano.md`, Passo 0). Vale anche per una scelta che resta dentro gli invarianti standard (`auth/`, `email/`, `stack/`), purché condizioni comunque più fasi a valle.

**Stato**: proposta
**Data**: 2026-08-30
**Arco di decisione**: Fase 2 (`fase-2-login.md` §2.1, collection `users`) → Fase 4+ (fuori template, sviluppo sezioni App reali di ogni progetto)

## Contesto

La collection `users` è condivisa tra Area Admin e Area App (`payload-pattern/02-convenzioni-payload.mdc` — niente collection separate). Serve uno schema di ruoli funzionante fin da Fase 2 per popolare `/admin` (seed super-admin, §2.8) e distinguere un accesso Admin da un accesso App. Ma il template, per costruzione, non può conoscere i ruoli e i permessi specifici di un progetto reale: quelli sono dominio applicativo di Fase 4+, esplicitamente fuori scope per la generalizzazione (vedi `riepilogo-sessione-cursor-rules-template.md`). Serve quindi un baseline minimo, generico per qualunque progetto, che non blocchi Fase 2 in attesa di una specifica di dominio non ancora scritta.

## Decisione

Il template fissa solo il livello di ruolo generico, come due campi `select` singoli e non cumulabili: `adminRole` (`none`/`admin`/`super-admin`) e `appRole` (`none`/`user`, enum aperta a estensione per-progetto). Nessun campo `roles` cumulativo unico.

## Alternative considerate

- Campo `roles` cumulativo unico (multi-select) — scartato: viola la convenzione di catalogo "ruoli come select singolo per area", mescolerebbe permessi di aree diverse in un solo campo.
- Rimandare interamente lo schema ruoli a Fase 4, quando si conoscono i ruoli reali del progetto — scartato: bloccherebbe §2.8 (seed super-admin) e la chiusura di Fase 2, che dipende da uno schema minimo già funzionante.

## Conseguenze

Ogni sviluppo futuro di sezioni App (Fase 4+, fuori template) eredita la separazione `adminRole`/`appRole` e non può ripensarla senza un refactoring dello schema `users`. L'enum di `appRole` resta aperta: i ruoli specifici del progetto si **aggiungono** al baseline `none`/`user`, non lo sostituiscono. La collocazione fisica definitiva dello stub `canAccessSection` (§2.1) resta esplicitamente un punto aperto, da decidere solo alla prima sezione App reale — questa ADR non lo risolve, lo eredita come vincolo dichiarato.
