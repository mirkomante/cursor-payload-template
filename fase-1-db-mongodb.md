---
stato: validato
---

# Fase 1.3 — Variante database: MongoDB

> Istruzioni operative per la sottofase **1.3** di `fase-1-setup.md`. Da allegare insieme a `fase-1-setup.md` nella chat dedicata a quella sottofase. Vedi anche `stack/01a-db-mongodb.mdc` nel catalogo regole per le convenzioni architetturali che questa variante rispetta (nome variabile d'ambiente, adapter).

**Questo è un passaggio esterno a Cursor**, anche se locale e non su cloud: richiede che MongoDB sia installato e in esecuzione sul tuo computer prima che l'agente possa configurare la connessione.

**Checklist per l'umano (da seguire prima di procedere con il codice)**:
- [ ] Verificare che **MongoDB Community Server** sia installato e in esecuzione in locale.
- [ ] Nessuna creazione di utenti/permessi è necessaria per l'uso locale di base: MongoDB Community Server, in configurazione di default, non richiede autenticazione per connessioni da `localhost`.

**Checklist per l'agente (dopo conferma umana che MongoDB locale è attivo)**:
- [ ] Inserire la connection string locale come variabile d'ambiente (`.env`, non committata), usando il nome **`DATABASE_URL`** (convenzione Payload, non `MONGODB_URI`):
  ```
  DATABASE_URL=mongodb://127.0.0.1:27017/<nome-progetto>
  ```
  (il nome del database viene creato automaticamente da MongoDB al primo utilizzo — non serve crearlo a mano).
- [ ] Configurare l'adapter MongoDB di Payload (`mongooseAdapter`) nel file di configurazione principale, puntando alla variabile d'ambiente `DATABASE_URL`.
- [ ] Verificare la connessione avviando il progetto in locale e controllando che Payload si connetta senza errori.

**Percorso concordato dev → produzione** (dettaglio operativo completo da scrivere in Fase 3, non qui): sviluppo su MongoDB Community Server locale → deploy iniziale su Atlas tier M0 (gratuito) → migrazione a un tier a pagamento prima dell'uso reale in produzione (upgrade in-place, stessa connection string, nessuna migrazione dati manuale).

---

Al termine di questa checklist, torna a `fase-1-setup.md`, sottofase 1.3, e verifica lì la "checklist di chiusura sottofase" prima di passare a 1.4.
