# Spec di implementazione — Project Intelligence Skills

## Stato

Proposta pronta per implementazione.

## Obiettivo

Costruire tre skill complementari che aiutino AI e sviluppatori a comprendere un repository, stabilire una base operativa condivisa e applicare regole di sviluppo verificabili.

Le tre skill devono restare separate:

1. `analyze-project` osserva e mappa il progetto.
2. `bootstrap-project` trasforma la mappa in documentazione persistente e contratto operativo AI.
3. `define-dev-rules` definisce o aggiorna le regole di sviluppo.

L’analisi non deve modificare il progetto. Il bootstrap e la definizione delle regole modificano file solo con richiesta esplicita dell’utente.

## Struttura nominale obbligatoria

Ogni cartella deve contenere una skill con corrispondenza nominale:

```text
analyze-project/
└── SKILL.md                 # name: analyze-project

bootstrap-project/
└── SKILL.md                 # name: bootstrap-project

define-dev-rules/
└── SKILL.md                 # name: define-dev-rules
```

Sono ammessi `agents/openai.yaml`, `references/`, `scripts/` e `assets/` solo quando supportano direttamente la skill. Il nome della directory, il campo YAML `name` e il comando di invocazione devono corrispondere.

## Requisiti funzionali comuni

Tutte le skill devono:

- distinguere `observed`, `inferred`, `decided`, `recommended` e `unknown`;
- usare evidenze con percorso file, simbolo o comando;
- non esporre valori segreti, ma solo nomi e posizioni delle variabili;
- rispettare `AGENTS.md`, `CLAUDE.md` e le istruzioni applicabili;
- non trasformare ipotesi in convenzioni del progetto;
- dichiarare cosa è stato verificato e cosa no;
- produrre output leggibile dall’AI in Markdown e, se richiesto, JSON;
- usare HTML solo come vista documentale umana, mai come fonte normativa primaria.

# Skill 1 — analyze-project

## Responsabilità

Leggere il repository e produrre una mappa affidabile di stack, struttura, dipendenze, architettura, flussi, test, delivery e rischi.

## Modalità

- `quick`: manifest, stack, albero delimitato, entry point.
- `standard`: aggiunge flussi runtime, test, CI/deployment, architettura e rischi.
- `deep`: aggiunge tracing a livello di simboli, edge tra package/servizi, configurazione e ipotesi di rischio prioritarie.

## Flusso

1. Identificare root, istruzioni locali e stato del working tree.
2. Rilevare monorepo/workspace e mappare package, servizi e dipendenze tra loro.
3. Leggere manifest, lockfile, script, entry point, Docker e CI.
4. Costruire un albero bounded escludendo vendor, build, cache e output generati.
5. Tracciare entry point, routing, orchestrazione, persistenza, integrazioni, job ed error boundary.
6. Classificare l’architettura senza forzare pattern predefiniti.
7. Valutare confini, ownership, change locality, osservabilità, sicurezza e maintenance hotspot.
8. Eseguire test/build/typecheck solo in modalità standard/deep quando richiesto o autorizzato.

## Output

`project-analysis.md` con:

- executive summary;
- obiettivi e dominio;
- stack e versioni;
- repository map;
- package/service map;
- dipendenze e configurazione;
- architettura e flussi;
- testing e delivery;
- matrice rischio/impatto;
- cinque azioni successive;
- sparring questions;
- unknowns e prossime verifiche.

Output opzionale: `project-analysis.json` con schema stabile e campi `evidence` e `confidence` per ogni claim.

## Criteri di accettazione

- Non confonde una dipendenza importata con una dipendenza dichiarata.
- Riconosce monorepo e almeno i relativi edge interni.
- Ogni rilievo non banale ha evidenza o è marcato `unknown`.
- Non esegue comandi mutativi senza autorizzazione.
- Produce cinque azioni ordinate per leva e rischio.

# Skill 2 — bootstrap-project

## Responsabilità

Creare una base persistente e navigabile che permetta a un nuovo sviluppatore o agente di lavorare nel progetto senza reinterpretarlo da zero.

## Input

Usa `project-analysis.md`/JSON se presenti; altrimenti esegue o richiede un’analisi standard. Non sovrascrive decisioni o documentazione esistente senza mostrarne il conflitto.

## Artefatti

Genera o aggiorna, con diff leggibile:

- `CONTEXT.md`: glossario, obiettivi, attori, invarianti e vincoli;
- `ARCHITECTURE.md`: moduli, responsabilità, ownership, confini e direzione delle dipendenze;
- `DEVELOPMENT.md`: setup, comandi, workflow, criteri di completamento e troubleshooting;
- `AI-CONTRACT.md`: permessi, zone sensibili, regole di stop, verifiche obbligatorie e formato del lavoro;
- `DECISIONS/` o ADR esistenti: decisioni reversibili/irreversibili e motivazioni.

## Contratto operativo AI

Deve includere:

- azioni consentite autonomamente;
- azioni che richiedono conferma;
- file/directory sensibili;
- comandi di verifica per tipo di modifica;
- condizioni di stop;
- gestione di segreti e dati personali;
- formato atteso per piano, modifica, test e risultato.

## Invarianti e confini

Per ogni dominio o modulo documentare:

| Modulo | Responsabilità | Ownership | Usa | Non dovrebbe conoscere | Evidenza |
|---|---|---|---|---|---|

Gli invarianti devono coprire, quando rilevanti, stati, autorizzazioni, idempotenza, consistenza, eventi, cancellazioni e compatibilità API.

## Criteri di accettazione

- Gli artefatti sono coerenti tra loro e non duplicano fonti di verità.
- Il contratto AI contiene criteri di stop concreti.
- Ogni regola importante indica fonte e data di verifica.
- Le decisioni già presenti non vengono riscritte come raccomandazioni.
- Il bootstrap può essere rieseguito senza produrre rumore o duplicati.

# Skill 3 — define-dev-rules

## Responsabilità

Definire, auditare o aggiornare regole di sviluppo applicabili a AI e dev, sulla base dell’analisi e degli strumenti reali del repository.

## Categorie

- stile, naming, formatter, lint e typing;
- testing e livelli di copertura;
- gestione errori e osservabilità;
- sicurezza, segreti, dati personali e input non fidato;
- dipendenze, lockfile e aggiornamenti;
- commit e review;
- architettura, ownership e direzione delle dipendenze;
- migrazioni, health check, deploy e rollback.

## Output

Ogni regola deve contenere:

| Regola | Ambito | Stato | Rationale | Enforcement | Evidenza |
|---|---|---|---|---|---|

Lo stato è uno tra:

- `existing`: già applicata e verificata;
- `recommended`: proposta con costo di adozione;
- `decision-needed`: scelta da prendere dal team.

La skill può proporre `AGENTS.md`, `.cursorrules`, `DEVELOPMENT.md` o file analoghi, ma li modifica solo su richiesta esplicita.

## Matrice modifica → verifica

Deve produrre una matrice simile a:

| Area modificata | Test minimo | Verifica aggiuntiva | Rischio |
|---|---|---|---|

La matrice deve coprire almeno API, dominio, persistenza, UI, autenticazione e infrastruttura quando presenti.

## Criteri di accettazione

- Le regole derivano da tooling o evidenze del repository quando dichiarate `existing`.
- Le raccomandazioni hanno rationale e costo.
- Ogni regola è verificabile da un comando, una review o un criterio osservabile.
- Non impone convention generiche in assenza di decisione del team.
- Se aggiornata, conserva la provenienza e segnala i conflitti.

## Pagina HTML

Ogni bootstrap o definizione di regole può produrre `development-reference.html` come vista umana. Deve:

- essere autonoma e apribile localmente;
- mostrare stato, categoria, ambito, enforcement ed evidenza;
- includere matrice modifica → verifica e decisioni aperte;
- avere accessibilità di base e layout responsive;
- dichiarare che Markdown/JSON sono la fonte primaria;
- essere rigenerabile senza modifiche manuali persistenti.

## Sequenza di implementazione

1. Implementare e validare `analyze-project`.
2. Usare il suo output come input di `bootstrap-project`.
3. Implementare `define-dev-rules` sui documenti bootstrap.
4. Aggiungere generazione HTML e JSON.
5. Eseguire un test end-to-end su un repository piccolo e uno monorepo.
6. Verificare riesecuzione idempotente e gestione di documentazione preesistente.

## Verifica complessiva

La suite di validazione deve dimostrare:

- frontmatter valido e corrispondenza nominale delle tre skill;
- assenza di placeholder;
- report coerente su repository di linguaggi diversi;
- segreti non presenti negli output;
- comandi non eseguiti senza autorizzazione;
- JSON valido e schema stabile;
- HTML aperto localmente e coerente con Markdown;
- differenze documentali minime a una seconda esecuzione.

## Decisioni ancora necessarie

- Directory definitiva degli artefatti generati nel repository.
- Se `CONTEXT.md` e `AI-CONTRACT.md` debbano essere sempre creati o solo su richiesta.
- Schema JSON versionato e compatibilità futura.
- Tool scelto per generare HTML e strategia di aggiornamento.
- Comandi che richiedono sempre conferma anche in modalità `standard/deep`.
