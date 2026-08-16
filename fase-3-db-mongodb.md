# Fase 3 — Variante database: MongoDB (Atlas)

> Istruzioni operative per la sottofase **3.1** di `fase-3-deploy.md`. Da allegare insieme a `fase-3-deploy.md` nella chat dedicata a quella sottofase. Vedi anche `stack/01a-db-mongodb.mdc` nel catalogo regole per le convenzioni architetturali che questa variante rispetta.

---

## 3.1 — MongoDB Atlas (cluster M0)

**Passaggio esterno (umano, console Atlas, prima del codice)**:
- [ ] Creare cluster **M0** (free) — non Flex, non Dedicated da subito. L'upgrade a un tier a pagamento va pianificato quando il progetto si avvicina all'uso reale (in-place, stessa connection string), non ora.
- [ ] Region allineata alla region scelta per l'ambiente cloud (§ 3.2), per minimizzare latenza.
- [ ] Network Access: allowlist aperta (`0.0.0.0/0`) è una scelta accettabile per un tool interno a bassa criticità, compensata da TLS sempre attivo (nativo Atlas) + password DB robusta generata random — non introdurre VPC Peering/Private Endpoint (oltretutto non disponibile sui tier M0/Flex) se il progetto non lo richiede esplicitamente. Rivalutare se il progetto ha requisiti di sicurezza più stringenti.
- [ ] **Un solo utente database** (`readWrite` sul solo database del progetto, non `readWriteAnyDatabase`, non ruoli di amministrazione cluster) — stesso utente per lo script di seed e per l'app a runtime, nessuna separazione: entrambi richiedono esattamente gli stessi permessi.
- [ ] Password generata random (generatore Atlas o password manager) — **non annotarla in chiaro da nessuna parte**: verrà incollata direttamente nel gestore di secret dell'ambiente cloud (§ 3.2).
- [ ] Copiare la connection string (`mongodb+srv://...`) con utente/password sostituiti — è il valore che andrà nel secret `DATABASE_URL`.

**Attenzione — errore comune**: verificare che il nome del database nella connection string corrisponda esattamente al nome usato per l'utente/i permessi in Atlas Database Access — un disallineamento tra "nome cluster/progetto Atlas" e "nome database nella stringa di connessione" produce errori di permesso silenziosi, non un errore di connessione esplicito.

**Checklist per l'agente** (dopo conferma umana che Atlas è pronto):
- [ ] Aggiornare `.env.example` con un commento che indichi Atlas come DB di produzione (nessuna credenziale reale nel file).

**Nota percorso DB**: sviluppo su MongoDB Community Server locale (Fase 1) → deploy iniziale su Atlas tier M0 (qui) → migrazione a un tier a pagamento prima dell'uso reale in produzione (upgrade in-place, stessa connection string, nessuna migrazione dati manuale) — verificare il timing esatto quando il progetto si avvicina alla data di utilizzo reale, non pianificarlo ora.

---

Al termine, torna a `fase-3-deploy.md`, sottofase 3.1, e verifica lì la checklist di chiusura prima di passare a 3.2.
