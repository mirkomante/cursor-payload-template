# Prontuario — Avvio di un nuovo progetto

> Sintesi operativa per iniziare un progetto reale con il processo v2 (`cursor-rules` + `cursor-payload-template`). Non sostituisce `00-come-eseguire-il-piano.md` — ne è la checklist d'ingresso.

## 0. Dove si lavora

- **Progetto Claude dedicato**, separato dal progetto meta (quello dove si mantiene il processo stesso). Non è obbligatorio per il processo, ma coerente con la separazione meta-livello / esecuzione già in uso.
- **Conoscenza da caricare in quel progetto Claude, prima di iniziare**: entrambi i repo — `cursor-rules` (catalogo varianti disponibili) e `cursor-payload-template` (file di fase) — non solo uno dei due.

## 1. Passo 0 — Pianificazione (dentro il progetto Claude)

Un'unica sessione può contenere tutto: brainstorming, decisioni, DAG, ADR. Nessuna controindicazione a farlo in un solo progetto/chat.

Ordine corretto dei sotto-passi:

1. **Brainstorming**: punti chiave del progetto, opzioni realizzative.
2. **Decisioni di variante**: provider auth, provider email, database, ambiente cloud (package manager e UI kit sono fissi).
3. **DAG di progetto**: Claude lo costruisce come output della sessione, tipizzando ogni arco (**output** o **decisione**).
4. **ADR**: uno per ogni arco di decisione, scritto **subito dopo** averlo tipizzato nel DAG — mai prima del DAG.

**Annotazioni pratiche**:
- Se il brainstorming è lungo e divergente, aprire una nuova chat *nello stesso progetto Claude* per la parte finale (decisioni → DAG → ADR), per non seppellire l'output in un contesto molto esteso.
- Tenere gli artefatti finali (decisioni, DAG, ADR) testualmente distinti dalle bozze di brainstorming, per non confondere la ricerca futura nella project knowledge.

## 2. Passo 1 — Composizione (in Cursor)

Una volta chiuso il Passo 0:

1. Creare la nuova directory di progetto.
2. Copiare in `.cursor/rules/`: tutto `core/`, tutto `payload-pattern/` (se architettura a 4 aree), e **una sola variante per asse** da `auth/`, `email/`, `stack/`.
3. Copiare in `docs/piano-sviluppo/`: i file di fase master (`fase-1-setup.md`, `fase-2-login.md`, `fase-3-deploy.md`), `00-piano-generale.md`, `00-come-eseguire-il-piano.md`, `ADR-template.md`, `CHANGELOG.md` vuoto, più i file di variante pertinenti alle scelte del Passo 0.
4. Salvare il DAG come `docs/piano-sviluppo/piano.yaml`.
5. Compilare i placeholder di `00-piano-generale.md` (nome progetto, varianti adottate) e il paragrafo di apertura di `core/01-proporzionalita.mdc`.
6. Creare da zero, in `docs/`, eventuali specifiche di progetto (solo se si devia dagli invarianti standard).

Passo 0 e Passo 1 si fanno una volta sola.

## 3. Sanity check iniziale

Prima del primo prompt su Fase 1, sottofase 1.1: revisione di coerenza con Composer sull'**intero insieme** appena composto (regole + fasi + `piano.yaml`), non solo su un asse. Prompt standard:

> Confronta questo file con gli altri file dello stesso asse (stessa cartella), segnala contraddizioni.

— qui esteso a tutto l'insieme.

## 4. Esecuzione — Fase 1 in poi

- Inizio da Fase 1 con Cursor Composer.
- **Una chat per sottofase** (eccezione: sottofasi accoppiate che condividono la stessa integrazione da due lati).
- A ogni chat: indicare esplicitamente file di fase + sottofase + eventuale file di variante. Le `.mdc` sono automatiche, non vanno allegate.
- Ogni regola locale di progetto aggiunta durante l'esecuzione richiede la stessa revisione per asse usata nel catalogo.
- Fine sessione: stato sottofase aggiornato, CHANGELOG aggiornato (Ufficiale/Percepito/Osservato se c'è stata ambiguità), commit locale (push resta manuale).
