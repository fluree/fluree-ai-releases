# Fluree AI changelog

Customer-facing changes and upgrade information for Fluree AI.

## [0.10.0-rc.11] - 2026-11-23

### Added

- Google Drive is a document source. **Add source → Google Drive** links your
  Google account, then opens My Drive and every shared drive you can read, one
  level at a time; **Hold this folder** makes the source. Its files stay in
  Drive: they are listed, previewed and extracted from there, and the copy
  a preview or an extraction reads is kept on the server only briefly (see
  **How to Connect Google Drive as a Source**). An administrator sets the
  server's own Google OAuth client under **Settings > Services & keys**, which
  shows the redirect URI to register.
- Each document source has its own page. It shows what the source is and its
  documents a page at a time, with folders, sorting, search, and filters by
  type and by whether a document is mapped. Each file shows its upload status,
  its last extraction and the graphs it feeds. Upload files, folders or zip
  archives, make folders, rename the source, and delete documents with every
  graph that would lose entities named first. Tick documents or folders and
  press **Map to graph** to start an extraction on just those.
- **Connect a warehouse** offers **Google BigQuery**. Fluree reads BigQuery's
  Iceberg tables as a Google identity you grant, by workload identity
  federation or with a service-account key. The dialog shows the exact values
  to enter in Google Cloud, and **Test connection** checks the catalog and the
  data files separately. See **How to Connect Google BigQuery** in the built-in
  documentation.
- A lake table's key can be proven. Under **Unique key** in a table's drawer,
  **Check the key against every row** reads the key columns of every row, and
  a key with a value on every row and no two rows alike is recorded with the
  row count and the date it was proven on. Fluree then declares it to the
  engine as the table's key wherever a mapping that keys the table this way is
  bound. A later check that finds a repeat withdraws the proof.
- A whole model can be exported. **Export…** on the model page downloads the
  model at the branch and point in time in view, as Turtle, JSON-LD or RDF/XML.
  The file holds what your access policies let you read.
- A model's page has tabs: **Overview**, **Projects**, **Vocabularies**,
  **Quality**, **Changes**, **Graphs** and **Settings**. **Overview** says what
  the model holds and what is waiting on you, **Quality** lists each governed
  graph's latest check, and **Changes** lists proposals with who approved
  each. History files every commit a merge brought in under its proposal.
- A class's page has tabs: **Overview**, **Projects**, **Properties**,
  **Resolution**, **Records**, **Graphs** and **History**. **Resolution** holds
  every matching setting of the class in one place, and a save there is checked
  by the profile compiler first. **Records** shows golden records beside the
  source records, and names the golden record each source record folds into.
  With a branch of the model in view, **Overview** checks the class's records
  against the branch's shapes, and an administrator can **Simulate with this
  branch** for each resolution project on the class.
- **Context** has a **Vocabularies** view beside **Graph** and **Tiles**: a card
  per vocabulary with its definition and concept count, with find, sort,
  **Import SKOS…** and **New vocabulary**. A vocabulary's concept tree opens on
  its own page, and the rail's **Classes** and **Vocabularies** sections fold.
- **New ontology** can read a data dictionary. A workbook that describes tables,
  with a row per field giving its business name, definition, type and role, is
  drafted as the tables it describes: a class per table and a property per
  field, labelled and defined as the dictionary says, and a relationship for
  each link the dictionary declares. The AI drafting assistant reads the
  dictionary's declarations and notes too. **Read its sheets as records
  instead** maps the sheets themselves.
- A resolution project's tabs are named for the work: **Overview**, **Review**,
  **Golden records**, **Rules** and **Activity**, plus **Relationships** on a
  project that declares them. A status line above the tabs says whether the
  project has run, is running, failed, or has answers and settings waiting,
  and runs or stops it from any tab. **Rules** holds every setting, the
  suggested cutoffs and weights, and the second opinion; **Activity** holds
  match quality against your verdicts, the runs and the decision log.
- A resolution project can offer its tuning back to the model. **Rules** shows
  the project's fields in the model's table, each value the project moved
  marked with who moved it, and **Offer to the model** opens a proposal on a
  branch of the model with the tuned values. A project can also see and take
  its model's changes.
- A resolution profile can name the fields that license an automatic merge
  together, such as a name, a full birth date and a street, and can hold for
  review, or keep apart, any pair a field contradicts, such as two given names
  that are not one name, even when an email agrees. Automatic merging can be
  turned off. Review cards say when one of these rules kept a pair waiting.
- Each pair in the review queue says why the run left it for you: its score is
  below the merge line, no identifier agrees, or earlier answers disagree about
  its records. **Update resolved records** applies every verdict given since
  the last run, from the queue or the status line, and a review is done once
  every uncertain pair has an answer, however few verdicts that took.
- After a full run, **Rules** can suggest new field weights fitted from the
  run's own records, with no verdicts needed. Your verdicts then place the
  cutoffs that go with them, and the two can be applied together once they
  hold up on held-out verdicts.
- A model can declare a person's street, city and postal code on the address
  record the person links to, and a resolution project matches on them and can
  block on the postal code. Each address is compared whole, against one
  address of the other record, so a move counts as one disagreement.
- Anyone who can read a resolution project's graph sees its runs and how many
  pairs wait for review, on the project's page and on a class's **Projects**
  tab.
- The extraction wizard grounds on the vocabularies you choose. It lists the
  model's vocabularies, all checked to begin with, so a model that also holds
  code lists or a second language's copy no longer hands every one of them to
  the matcher. Its steps are worded more plainly.
- The extraction wizard remembers a source's set-up. The target graph,
  vocabularies, linked graphs, taxonomy and extraction of the last run started
  from a source's page come back the next time the wizard opens on that source,
  in the same browser. Anything deleted since is dropped.
- Entity recognition reads a document in its own language. Names and terms are
  matched by their dictionary forms, so plurals and inflected forms meet the
  vocabulary. English and French are built in; sixteen more languages use the
  lemma table set, which a server connected under **Settings > Fluree
  connection** downloads on first use and any other server reads from
  `FLUREE_AI_LEMMA_PACK`. The public knowledge base it links to is built from
  YAGO 4.6, with English and French labels; point `FLUREE_AI_YAGO_PACK` at the
  new index set to use it.
- An **OpenAI-compatible** LLM provider reaches any endpoint that speaks the
  OpenAI Chat Completions API, such as an enterprise LLM gateway or a
  self-hosted model server, for chat, agents, extraction and document parsing,
  and for embeddings when no OpenAI provider is configured. Configure it under
  **Settings > LLM Configuration > LLM providers**, or with
  `OPENAI_COMPATIBLE_API_KEY` and `OPENAI_COMPATIBLE_BASE_URL`. Model ids that
  contain `/` reach the endpoint unchanged.
- An LLM provider other than Bedrock can authenticate with OAuth 2.0 client
  credentials instead of an API key: the server fetches a short-lived token
  from the identity provider, reuses it for most of its lifetime and renews
  it before it expires or when the endpoint refuses it, for chat, embeddings
  and the model list. Set `auth` and `clientSecret` on the provider through
  the API, or `<PREFIX>_TOKEN_URL`, `<PREFIX>_CLIENT_ID` and
  `<PREFIX>_CLIENT_SECRET` in the environment.
- An LLM provider other than Bedrock can carry extra headers (`extraHeaders`,
  and `secretHeaders` for values kept as secrets) and extra top-level body
  fields (`extraBody`) for every request to it, and body fields to leave out
  (`dropBodyFields`), through the API or `<PREFIX>_EXTRA_HEADERS`,
  `_SECRET_HEADERS`, `_EXTRA_BODY` and `_DROP_BODY_FIELDS`. A provider's own
  headers and fields always win, and a secret header can be a provider's only
  credential.
- **Model routing** has a **Link** row, for the decisions that say which known
  entity a name refers to. It can be given its own model without moving chat
  or extraction.
- Each provider card under **Settings > LLM** shows whether its key works:
  accepted, refused, misconfigured or unreachable. `GET
/v1/llm-providers/status` checks every provider and `POST
/v1/llm-providers/{id}/check` checks one.
- A long query can run as a job. Send a query with `Prefer: respond-async` and
  the server answers `202` with a job id at once; `GET
/v1/fluree/query-jobs/{jobId}` reports its state, `…/result` returns the
  answer, and `DELETE` cancels it. A job is visible to whoever started it and
  to administrators, each user may have ten running, and a result is kept for
  24 hours. A request without the header behaves as before.
- When mapping warehouse tables, a table can add details to the records another
  table makes, without declaring a class of its own. The mapping screen says
  what each table's rows are and which table makes the records. Use it for a
  history or detail table, so each record is typed once.
- `fluree-ai backup` writes a stopped server's data directory to one file, and
  `fluree-ai restore` puts it back at any path, on any machine, under the same
  version or a newer one. The file holds every ledger, the uploaded documents,
  the run history and the users; it leaves out caches and settings.
  `--include-secrets` adds the server's connection to a Fluree account, the
  Google accounts users linked, single sign-on client secrets, the provider
  keys saved under **Settings > LLM Configuration**, catalog credentials and
  the license. **How to Back Up and Restore a Standalone Server** lists
  everything an archive holds. `backup inspect` says what a file holds and
  whether this version can restore it, and `backup verify` checks every file
  in it against its checksum. A restore goes into a new or empty data
  directory, never merges, and puts nothing in place unless the whole file
  verifies. A file from an older version needs `--accept-upgrade`.
- `fluree-ai restore` takes an `https://` URL in place of a file and restores
  it as it downloads, and `--start` brings the server up when it finishes.
- A backup can be encrypted so that it can be published and opened only by
  whoever was given its secret: `backup --encrypt` generates a key and prints
  it once, and `backup --passphrase` takes a passphrase
  (`FLUREE_AI_ARCHIVE_PASSPHRASE`, or asked for). Restoring one reads
  `FLUREE_AI_ARCHIVE_KEY` or `FLUREE_AI_ARCHIVE_PASSPHRASE`, or asks. Without
  the secret, `backup inspect` shows only which version the data needs.
- A local lake: Iceberg tables in `<data-dir>/lake/<folder>/<namespace>/<table>`
  are read from the server's own disk, with no catalog service and no object
  store. Connect one under **Connect a warehouse → Tables on this server**,
  browse it, generate a mapping and query it as a graph. A backup carries the
  tables with everything else, and a server restored at another path
  registers its lake graphs there when it starts. Row samples, the refine
  tools and publishing into a local lake are not available yet.
- `fluree-ai notices` prints the third-party data the product uses and its
  licences. The same text ships as `THIRD-PARTY-NOTICES` in the archive and
  the container images.

### Changed

- An extraction run stays **running** until its output is saved to the graph,
  and shows "saving N of M" while it does.
- Starting a run over documents another run is still working on is refused
  (409 `RunInFlight`) instead of running both.
- When a model call made while extracting or resolving gets no usable answer,
  even after retries, the document fails with a message that names the chunk
  or batch, how many had answered, and that it can be re-run once the model is
  reachable. The call to the model is retried after a rate limit (`429`) or a
  server error first.
- Extraction, classification and entity resolution send their sampling
  temperature (0 unless set) to the models that take one, so their answers
  vary less from run to run. A model that takes no temperature is sent none.
- Entity resolution compares records the way people write them. A name reads
  a known nickname as agreement (_Bob_ and _Robert_), compares an initial as
  an initial, and folds accents on Latin letters (_José_ and _Jose_). A date
  reads a bare year inside a full date as partial agreement, and two dates a
  day apart, a year one off, the month and day exchanged or two neighbouring
  digits swapped as a likely typo. A street line agrees however its case,
  punctuation, street type or direction is written, and reads a unit apart
  from the street.
- The business model's Person class starts with better resolution settings: a
  family name worth more than a given name, a given name that holds a pair for
  review when it contradicts, a birth date and a generational suffix (Jr., Sr.)
  that keep namesakes apart, tax IDs that identify no one counted as empty,
  blocking keys built from name and birth date, and shared addresses weighed
  less.
  Existing projects keep their settings; their **Model** card shows the
  change.
- Resolving a large set of records uses every core and much less memory, and
  a re-run writes only what changed.
- Grounding against a large exported set of records (more than 100,000, which
  a grounding view can change with `largePackRecords`) links a mention on its
  own only when it is a multi-word name with support in the same document,
  such as a registry code (LEI, CAS, ISIN) for the same record, another
  mention of it, or a legal form after a distinctive word. Other mentions are
  decided in context, by the trained ranker or the AI arbiter, or left for
  review. In any set exported from a class, a record whose label is a single
  common business word, such as "Core" or "Finance", is decided in context
  the same way, while its multi-word names still link directly.
- A graph read in place shows a class or reference count its tables cannot
  give exactly as unknown, with an upper bound where one is known, and keeps
  such a class in the instances view, the grounding picker and the data model
  agents read.
- **Connect** lists sources most recently changed first, and a source's card
  opens its page.
- The concept page in a vocabulary groups its fields into Labels, Description,
  Hierarchy, Codes and mappings, Editorial notes and History. Each group
  folds, the first three start open, and a folded heading shows how many
  values it holds. A concept's status is changed from the **⋯** menu.
- The model graph zooms on pinch or Ctrl/⌘ + wheel. A plain wheel scrolls the
  page.
- Matching against a model, **Check keys and joins**, **Resolve with AI** and
  refine show that they are running, with the time elapsed.
- A relationship that ends up pointing from a record to itself, such as after
  two records are merged into one, is rejected as a self-reference and kept
  for review instead of being written to the graph.
- A name written with a capital letter refers only to its capitalised
  repetitions: an entity "Apple" no longer takes every "apple" in the text.
- On a hosted stack, a query that would outlast the 60-second limit in front
  of the server is stopped with an error that names `Prefer: respond-async`.
- A data directory can be moved or copied to another path and started there.
  The addresses the server records no longer contain the directory's location.
- The database engine is fluree/db v4.2.9. Changes a query or a write can
  notice:
  - A write that carries a typed value whose text is not a value of its type,
    such as `"N/A"^^xsd:integer`, is refused and commits nothing. JSON-LD
    writes and `INSERT DATA` already refused one; now Turtle, TriG and
    N-Triples writes do too (a bulk upload in Turtle, a vocabulary or model
    updated from a file), and so do a SPARQL update that builds one with
    `STRDT` or copies one with `INSERT … WHERE`, and a `@json` value whose text
    is not JSON. A bulk upload in Turtle commits in chunks, so a refused chunk
    leaves the others loaded. Importing files into a new ledger still keeps
    such a value as text under its type.
  - A value a ledger holds as text under a type it does not fit comes back in
    JSON-LD results with that type,
    `{"@value": "landed", "@type": "xsd:integer"}`, including under a type
    JSON-LD reads from a number or a boolean (`xsd:integer`, `xsd:double`,
    `xsd:boolean`). An `rdf:JSON` value whose text is not JSON comes back the
    same way.
  - Importing a JSON-LD file into a new ledger
    (`POST /v1/datasets/{datasetId}/import`) refuses a value object whose
    `@value` is an array or a map unless it, or its term, has a `@json` type
    (or `@vector`, for an array), with an error that names the `@type` to add.
  - A SPARQL UPDATE request may hold several operations separated by `;`. They
    are applied in order and committed together.
  - SPARQL `DROP GRAPH`, `DROP DEFAULT`, `DROP NAMED` and `DROP ALL` take a
    graph and its history out of queries, where they used to retract its facts
    as `CLEAR` does. Dropping needs an identity with no restricting policy, and
    dropping a graph that does not exist is an error unless `SILENT`.
  - `FILTER(?x = "text")`, and `<`, `<=`, `>`, `>=` against a plain string,
    compare the exact term: they no longer match a language-tagged
    `"text"@en` or a value of another type such as `xsd:anyURI`. Compare
    `STR(?x)` to match the text whatever its tag or type.
  - A query over several graphs (the union default graph, or several `FROM`
    graphs) counts, groups and joins over all of them.
  - Cypher refuses `RETURN`, `WITH` or `UNWIND … AS v` where `v` is already
    bound, and `` n.`@id` `` or `` n.`@type` ``, with an error that names the
    form that works (`AS v_value`, `id(n)`, `labels(n)`). A read clause
    (`MATCH`, `OPTIONAL MATCH`, `WITH`, `UNWIND`, `CALL { … }`) written after a
    write clause is refused too: put the reads first, or split the statement
    in two.
  - An access policy that targets a class (`f:onClass`) applies, in a
    reasoning query, by the classes reasoning derives. Over a mapped table
    whose rows give a subject its class and which declares no key, such a
    policy can refuse the query with an error that names the mapping: declare
    the table's key (`f:key`), or target the policy by property.
  - A branch of a ledger that was never indexed, and a ledger copied under
    another name, read their source's ledger config, so its policies,
    uniqueness and SHACL settings apply to them.
  - An R2RML graph source answers with each triple its mapping defines once,
    so `COUNT`, `SUM` and `AVG` over a table with repeated rows no longer need
    `DISTINCT`, and a reference to a parent with a non-unique key keeps every
    edge.
  - Queries over an Iceberg graph source can use sub-selects, and a triples
    map over an Iceberg table can be narrowed with a restricted `rr:sqlQuery`.
    A join in a query over a mapped table (Iceberg, SQL or Delta) is produced
    a batch at a time, stops when the query is cancelled, and counts against
    its memory budget.
  - JSON-LD reads `@vocab` and `@json` term coercion, a term's `@language`,
    terms defined as `null` and `@index` maps as JSON-LD 1.1 defines them, and
    a number or boolean `@value` keeps its datatype. Values an earlier version
    wrote through such a term keep the shape they were stored in.
  - A JSON-LD query's `iri` constant expands through the `@context`, as an
    `@id` does: `["iri", "skos:Concept"]` with `skos` defined is the full IRI.
    A prefix the `@context` does not define (`tag:`, `arn:`) is an "Unresolved
    compact IRI" error unless `opts.strictCompactIri` is `false`.
  - Fuel moves with a query's shape, both ways. Queries answered by a fast
    path (counts, aggregates, top-k, label regex, geo search), range reads
    under a policy or across datasets, and joins charge for each index leaflet
    they read, as scans already did, so they cost more and a fuel limit
    (`fluree-max-fuel`) can stop them sooner. A string `FILTER` the engine now
    answers on a faster path can cost far less.
  - Publishing or exporting a ledger as a `.flpack` refuses an object of 4 GiB
    or more, with an error that names it.
  - Edge annotations written in a named graph reach the index's annotation
    store, so an export carries them.
  - The `fluree` CLI draws its tables with box-drawing characters. A script
    that reads its table output should ask for `--format json`.

  The engine's compatibility notes list the rest.

### Fixed

- A thinking level set on a **Model routing** row is applied.
- The model screen shows a change the assistant makes to the model as soon as
  it is made, without a reload.
- A warehouse mapping reopens with the choices you saved, and editing a
  table's key on the mapping screen keeps the table and its drawer on screen
  while it matches again.
- **Load more** on a graph read in place never lists a record twice.
- Importing a spreadsheet keeps the leading zeros of a column of zero-padded
  numbers, such as ZIP codes, by reading it as identifiers.
- Signing in and refreshing a session have rate limits of their own, apart
  from the rest of the API's, and a successful sign-in no longer counts toward
  the limit on attempts. Every `429` a rate limit answers carries
  `Retry-After`.
- Reindexing a ledger with `fluree reindex <ledger> --remote <remote>`, or
  through `POST /v1/admin/reindex/{ledgerId}`, rebuilds its index from its
  commits on a standalone server. The CLI waits for the rebuild and reports
  the `t` the new index reaches.
- On a standalone server, the Google redirect URI, the MCP sign-in metadata
  and the discovery documents take their scheme and host from
  `FLUREE_AI_PUBLIC_URL` when it is set.

### Upgrade notes

- The next run of every resolution project is a full run, because the
  comparison rules changed. Scores change on that run for projects with name,
  date, street or address fields.
- A resolution project's saved settings keep working. A profile saved before
  this release is read in this release's terms, and saved in them the first
  time it is read, so the **Rules** tab and every run read the same settings.
  Saving a retired setting through the API is refused, with a message naming
  what replaced it. Verdicts stand; their evidence is refreshed by the next
  run.
- A resolution project created before this release keeps its golden records
  in the project's default graph. A project created after the upgrade keeps
  its results in a generation of their own, and each full run replaces the
  generation; create a project again to move it to generations.
- `FLUREE_CURATION_GC_GENERATIONS` now counts a resolution project's
  generations too: it is how many runs' results a project keeps, the current
  one included. A server that set it to 0 now also keeps every generation a
  full run replaces; set it to 1 to drop them again.
- A Google Drive source now holds one folder, chosen when the source is made.
  A Drive source created with an earlier version has none: create it again on
  its folder.
- Backup, restore and moving are for data directories created by this version
  or a later one. A directory created earlier keeps the absolute paths it
  recorded for past runs and for each dataset's approved model, which resolve
  only at the original location, so `fluree-ai backup` refuses it: copy it
  while the server is stopped, and put it back at the same path.
- `fluree-ai backup` refuses a data directory that links to somewhere outside
  itself, because what the link holds would not be in the file.
  `--skip-external-links` backs up the rest. The links to a YAGO index or lemma
  tables given with `FLUREE_AI_YAGO_PACK` or `FLUREE_AI_LEMMA_PACK` are not
  refused: set the same variable where the backup is restored.
- Multi-node deployments (`--store-dir`, `--data-raft`) are not covered by
  `fluree-ai backup`. Copy every node's data directory and the shared store
  while the servers are stopped, as before.
- A single-node data directory now holds a `version.stamp` file, recording the
  version that last started it. A backup reads it, and refuses a directory
  without one.
- At its first start on this version, a server moves its connection to a
  Fluree account, the Google accounts its users linked and its single sign-on
  client secrets into its secrets directory, beside the keys saved in the app.
  A server restored from a backup made without `--include-secrets` is
  connected to none of them.
- A ledger that has dropped a graph cannot be read by an earlier release, so a
  server cannot be rolled back past this one once a graph has been dropped. A
  resolution project created on this version drops one each time a full run
  replaces its results.
- Reindex a ledger that was created by importing files with an earlier
  version, if the files held `@json` (`rdf:JSON`) or `xsd:duration` values, so
  that this version reads those values as they were written
  (`POST /v1/datasets/{datasetId}/reindex`). The same goes for a ledger
  published as a `.flpack` whose index Fluree CLI 4.2.7 or earlier built by
  importing files (`fluree create --from` or `fluree graph import`). Ledgers
  written by transactions need nothing.

## [0.10.0-rc.9] - 2026-11-03

### Added

- Custom connectors can be built, installed and run from the app. An
  administrator defines one under **Settings > Connectors**, on one page: its
  details, outbound authentication, actions, and incoming data by webhook or
  polling. A connector can also be imported from a manifest file or URL, and
  exported. Anyone can install one from **Connect**, under **Custom
  connectors**, with a form that asks only for what that connector needs.
- Each connection has its own screen: its status, its credential (replaceable,
  never shown), its webhook URL with secret rotation and a test delivery, its
  polling schedule with an immediate poll and a backfill, the Spaces and teams
  that have its tools, and the last seven days of **Activity**. **Try** runs
  one of its actions from a form built from the action's inputs.
  `PATCH /v1/connections/{connectionId}/credential` replaces a connection's
  key, token or webhook secret in place.
- When you ask to see records, the assistant can answer with a grid: a table
  drawn in the answer from queries it writes, with its row count, the dataset
  it reads, and paging past 25 rows. Its queries run as you, under your own
  policies.
- A Space's **Memory** tab shows its memories as a graph, linked through the
  graphs they name, the competency questions they cite and their tags. The
  list view names the memories each one shares something with, and memories
  show their dates and sort newest first.
- The Spaces page shows each Space's open tasks, open findings, chats,
  memories, installed apps and last activity, flags blocked tasks and urgent
  findings, and totals them across your Spaces, with **All**, **Personal** and
  **Team** filters and a sort by name or by recent activity. Spaces keep the
  same order on every visit.
- **View as** in a Space's chat finds, by name, the records an access policy
  is written for, such as suppliers, as well as people. Results are grouped
  by policy, each with how many records it would see.
- In the vocabulary workbench, a new vocabulary's or concept's preferred label
  takes the language you choose. History shows a label renamed in several
  languages as one rename per language, and review shows a proposal that only
  changes a label's language.
- A vocabulary can be read in a content language chosen apart from the
  interface language, from the picker on its page or with `?clang=` in a
  link. Each concept shows its label in that language where it has one, falls
  back between regional variants such as `fr` and `fr-ca`, and marks a label
  shown in another language with its tag. A vocabulary's own name, definition
  and notes are edited in place, in every language.
- A Space's default agent, the one its chat uses when no other is named, can
  be set with `POST` and `PATCH /v1/spaces` (`defaultAgent`; `null` goes back
  to the built-in Fluree agent). The app has no setting for it yet. An agent
  the Space cannot use is refused with a 400.
- `/.well-known/fluree.json` names where the server serves its own
  documentation (`docs.url`) and its MCP endpoint (`mcp.url`, with the Space
  id it requires), and says that a Space answers over MCP only once it has
  been exposed. An agent given only the server's address can find both. A
  server started without the web app leaves out the `docs` block, and a
  cluster's query peer reports whether it serves the documentation itself.
- The built-in documentation opens with a **Start Here** page that routes by
  situation to an ordered path. New pages cover the Business Model and
  building on it, access policies, bringing your own ontology, modeling
  patterns, resolving records across systems, querying across graphs, seeding
  a Space's memory, class definitions, and the output an extraction writes.
  The data quality pages cover statistical rules and the **Quality** tab's
  review queue.
- `fluree-ai notifications reset`, run against a member's cluster listener,
  resets the stored change notifications and makes every server drop its
  cached copies at once. Use it after restoring the ledger store from a backup
  while keeping the nodes' raft state (see the operations runbook). Running it
  when nothing is wrong only costs every server one reread.

### Changed

- A dashboard the assistant writes opens in the side panel as it arrives,
  drawn at half size so it keeps its layout; **expand** draws it at full size.
- The resolution second opinion works through the whole review band. Each run
  samples pairs from across the band and spends its budget on pairs no run has
  asked about, remembering every answer, including unsure ones. A run whose
  second opinion settled anything starts the next run itself, up to six in a
  row; a run started that way settles keep-aparts only, and its merges wait
  for the next run someone starts.
- Before the second opinion acts on a run, twenty of its answers on the audit
  sample must be decisive; an unsure answer no longer counts toward them.
  Below 99% confidence, the model's plain yes or no must also agree before an
  answer becomes a decision.
- Concepts and vocabularies created in the app are labelled with
  `skos:prefLabel` alone, without an `rdfs:label`.
- Only an artifact's creator, the Space owner or an administrator can change
  or delete a saved artifact, such as a dashboard; anyone else is refused
  (403). The Space's **Dashboards** tab shows **Delete** only to them.
- When a reader's grant names a policy class, the graph's property counts and
  fact count are withheld from them rather than shown as zero. On the graph's
  page each class's fields read **withheld by policy**, and the fact count
  shows as a dash or is left out; the data model gives no property counts,
  `fluree info` leaves the property figures out, and a task's property picker
  says "no use visible to you" instead of "unused". The assistant and MCP
  tools say data is not visible to the reader rather than that it does not
  exist.
- The command-line install instructions in the app and in the documentation
  install the `fluree` CLI release this server was built against, instead of
  the latest release.
- Granting a graph to a Space again keeps the grant's policy classes when the
  request leaves `policyClasses` out; a list replaces them, and an empty list
  clears them.

### Fixed

- Excel workbooks uploaded through a graph's **Upload** tab, or read by an
  extraction run, come through cell for cell: date cells are dates instead of
  serial numbers, a blank cell no longer moves the values after it under the
  wrong column, a formula with no saved result is empty instead of its text,
  and Japanese reading guides are no longer added to the text. A workbook
  whose sheets use prefixed markup, as some exporters write, is no longer read
  as empty.
- A quality assessment's statistical rules run again. They reported an error
  on every graph, and on a graph whose model has access policies, the rules
  that compare values within a group checked no records.
- A ledger written many times in quick succession, such as by repeated
  resolution runs, no longer piles up old index versions until the disk fills.
  Each ledger keeps around twenty at most.
- A first ranker can be trained and used. An extraction candidate now appears
  on the **Training** tab, with its own evidence, so it can be activated, and a
  resolution project offers its first fit before any version exists. Rolling
  back on either side judges the version you pick by its own evidence.
- Binding a graph to a model that uses OWL 2 RL or OWL 2 QL reasoning no
  longer fails.
- A refused graph creation no longer leaves "Creating the graph…" spinning: the
  refusal is shown, and only a transient failure is retried. When publishing
  golden records or creating a resolution project completes but a step after
  it does not, you are told which step and how to finish it.
- A member who is not an administrator no longer sees a false "Access denied"
  banner on a graph's access screen, and sees the policy subclasses and the
  model that governs the graph.
- The Stack page no longer describes cloud infrastructure or offers to delete
  the stack.
- A server reached over plain `http` now advertises `http` in its MCP sign-in
  metadata, connector sign-in callbacks and webhook URLs, so MCP clients can
  sign in to it, directly or through a cluster's query peer. Behind a proxy,
  the scheme the proxy reports still wins; set `FLUREE_AI_PUBLIC_URL` when a
  hostname or TLS terminator is in front.
- Saving a Space's MCP settings folds the form back into its bar, whose
  on/off chip shows the saved setting, and the connection details appear once
  exposure is saved on. Settings that fail to load show the error instead of
  a form whose **Save** would overwrite them with defaults.
- An id typed into **New graph**, **New model**, **Resolve identities** or
  **Create a graph** keeps its hyphens; each was removed as it was typed.
- **Settings > Fluree connection** shows a connected server as connected, with
  its account, tier and **Refresh token**, instead of "Not connected". A
  connection that cannot be read, and a refused token refresh, say so.
- `upgrade.sh` no longer reports a successful upgrade when run from a Homebrew
  installation's `share/` directory, where it left the installed `fluree-ai`
  unchanged. It now refuses any directory without a `fluree-ai` executable and
  names the `brew` command to use instead.
- The chat in a Space now follows the prompt extensions written on its agent.
- A Space whose default agent is shared with its team can be exposed over MCP
  to the team's members.
- A provider or **Model routing** change made in Settings takes effect within
  seconds on every server, including a cluster's query peers, app replicas and
  processing nodes. It could take up to a minute, with nothing to show for it,
  while a server kept answering with the previous model. The one-minute
  renewal of the cached configuration remains as a safety net.
- On a cluster, connecting to or disconnecting from another Fluree, and the
  sample catalog's token renewal, now reach every member. Until now the other
  members kept their earlier mounts until they restarted, and their sample
  catalog datasets failed once the token they held expired. Mounted datasets
  resolve on members holding their own data; query peers, app replicas and
  members reading a transaction pool do not mount them.
- A new **IRI prefix** applies to the next resource minted, on every server.
  Until now even the server that saved it kept minting under the previous
  prefix for up to five minutes, and those resources kept the old prefix.
- On a cluster, a task's new or changed data-change trigger applies on every
  server within seconds, once the write is readable there, instead of missing
  or mismatching commits for up to a minute.
- The data model view shows a dataset's new prefixes and role as soon as they
  are saved. Until now it kept the previous ones until the next write to the
  dataset, indefinitely on a dataset nobody writes to.
- An API key deleted right after a request used it is refused. Until now every
  later request with it failed with an internal error, and a key already left
  that way before the upgrade is refused too.
- An import status change, a dataset update or a Space update that lands just
  after the dataset or Space was deleted no longer recreates a partial record,
  after which every read of it failed with an internal error. One already left
  that way before the upgrade reads as deleted.
- On Windows, the server starts again. From 0.10.0-rc.8 it stopped on its
  first start, with a stack overflow while it upgraded its system ledger.
- On Windows, native imports run. Each one failed before it started, with
  "access denied" (os error 5).

### Upgrade notes

- An agent now runs in a Space only if the Space can use it: available
  everywhere or shared with the Space or its team, and not hidden by the
  Space. A chat message, task run or MCP activation naming any other agent is
  refused instead of running, and a task run refused as it starts is closed as
  blocked, with the reason on its timeline. Conversations and tasks given such
  an agent earlier need another one.
- A Space can no longer hide the default agent it chose: choose another
  default first. The built-in Fluree agent can still be hidden; the Space's
  chat, tasks and MCP connection keep running it while it is the default, and
  invoking it by name is refused, as before.
- Data an earlier version read from Excel workbooks this way can hold dates as
  numbers and values under the wrong column. Uploading the workbook again
  reads it correctly. A workbook whose styles are damaged now fails to open
  instead of reading its dates as numbers.
- When the second opinion runs through the Fluree connection, its first run
  after the upgrade asks again about pairs it answered before.
- Dashboards built as generated pages, the format before dashboard
  specifications, are retired. A saved one opens to a notice and can be
  deleted from the Space's **Dashboards** tab. Saving a new one
  (`artifactType: "dashboard"`) is refused with a 400; save a `dashboard-spec`
  instead. `GET /v1/spaces/{spaceId}/artifacts/{artifactId}/content` and
  `GET /v1/dashboards/{dashboardId}/content` are removed.
- A write refused by an access policy now gets 403, and one refused by a
  model's shapes 400, instead of 502. A query that names a ledger that does
  not exist, in its URL, a SPARQL `FROM`, a JSON-LD `from` or `fromNamed`, or
  an `@t:` reference, gets 404 instead of 500. Update clients that read the
  old codes.

### Known limitations

- While an administrator views a chat as someone else, the chat's grids and
  dashboards still run with the administrator's own access, and say so.

## [0.10.0-rc.8] - 2026-10-15

### Added

- Encrypt data at rest with a key you supply: a base64-encoded 32-byte key,
  such as one from `openssl rand -base64 32`, set in
  `FLUREE_AI_ENCRYPTION_KEY` or saved to `secrets.toml` with
  `fluree-ai config set storage.encryption_key`. Ledger data and the server's
  stored files, including uploaded documents, extraction and workflow output,
  staged imports and agent transcripts, are then encrypted on disk. The key is
  never written to the store or to the logs. Read the upgrade notes and known
  limitations before turning it on: files being imported or processed are
  decrypted to working copies that encryption does not cover.
- Dashboards. Ask for one in chat and the app draws it from a specification of
  queries, controls (date range, select, number, text) and panels (KPI, bar,
  line, area, pie, table, text), with no generated code and no build step.
  Each query runs as the person viewing the dashboard, under their own access
  policies. A dashboard saved to a Space is listed on the Space's Dashboards
  tab, where an As of control shows it at an earlier point in time.
- Connect Delta tables as warehouse sources. The **Databricks** tile connects a
  workspace through Unity Catalog with a service principal or a workspace
  token, and Unity issues short-lived storage credentials for each table. The
  **Microsoft OneLake** and **Delta Lake on S3 or ADLS** tiles connect a lake
  root, read with the server's own identity or an Entra service principal,
  whose tables are read as Delta in place and added by name (`schema.table`).
- Credentials entered when connecting a warehouse, such as a catalog token or
  a service principal's client secret, are now kept in `catalog-secrets.json`
  in the server's secrets directory; previously this server could not store
  them. The file is plaintext at rest: protect the volume and back it up with
  `llm-providers.json`.
- Answer grounding checks each chat answer, sentence by sentence, against the
  data the assistant retrieved, and marks each statement as found, not found
  or uncertain. Rows of a table answer are checked one at a time, and clicking
  a mark opens the result it was checked against. The check runs after the
  answer finishes, so it never holds the answer up. An administrator sets it
  under **Settings > LLM Configuration > Model routing**, using the Fluree
  connection where it offers the check, or your own decision-model key. The
  answer and the data it used are sent to the checking service.
- Resolution projects get a second opinion on the pairs their matcher is
  unsure about. A decision model scores a sample of the matcher's confident
  pairs, then the review band. When the run's own evidence shows the model
  agrees with the matcher and with your reviewers, its confident answers become
  standing decisions for the next run; the rest stay in the review queue with
  its answer beside them. A person's verdict always wins, one in ten confident
  answers is held back for a person, and it never joins records someone ruled
  apart. **Record matching** can be routed to any configured model.
- A new `org-name` match type compares organization names in a folded form
  that ignores accents, dotted initials and spelled-out legal forms (Limited
  and Ltd, Public Limited Company and PLC), without folding one legal form into
  another. The starter Business Model's Company legal name uses it.
- Platform administrators can review agent activity under **Settings > Agent
  activity**: totals for each agent across every Space, and a list of chat
  turns, task runs, structured invocations and external MCP client calls,
  filtered by agent, source and outcome. Each entry opens its transcript, with
  the arguments and results of every tool call. The list is also available
  from `GET /v1/admin/activity`.
- Each agent activation writes metadata-only events to the server log under
  the `fluree::activity` target, for monitoring and SIEM tools. The events
  carry identifiers, outcome, timing and token counts, never arguments, results
  or answers.
- The chat assistant can retract and replace values, write to a branch, and
  open and list merge requests. An AI never changes `main` of a model that
  requires review: it puts the change on a branch and proposes it for a person
  to review. The rule also holds for external AI clients connected to a Space's
  MCP endpoint, and an AI cannot turn review off or delete a model while review
  is on. A Space can switch each of the new tools off.
- The vocabulary page is now a workbench. A vocabulary can be renamed, deleted
  while it is empty and unused, and exported as SKOS (JSON-LD) that Import SKOS
  reads back; concepts keep editorial, history and change notes; and a mapping
  is made by searching for the concept. Reviewed AI assists suggest alternate
  and hidden labels, translate missing preferred labels up to 100 at a time,
  and draft a concept's definition.
- The constraint editor can create a new vocabulary for a property to take its
  values from. A class's definition page lists its instances and what uses it.
  Each competency question in a model has an Ask button that sends it to the
  assistant to answer from the model and the graphs it governs.
- A model's settings have a **Shapes on the model itself** switch, as a
  graph's do: enforce rejects a write to the model that breaks one of its
  shapes, and assess admits it. A model enforces unless it is switched.
- Email files (`.eml` and Outlook `.msg`) are extracted as email: the body is
  decoded as written, a reply is split into the messages it quotes, and the
  subject, sender and date become the document's title, creator and date.
  Meeting transcripts (WebVTT and SubRip) are read as speaker turns.
- Each extracted document has one `foaf:Document` node, named after its file,
  that describes it: its media type, size and SHA-256 hash as read, its
  language, word and page counts, the run that read it, and what the file
  declares about itself (title, creator, and created and modified dates from a
  PDF's information dictionary or an Office file's properties).
- Uploading a file whose name is already taken in its folder asks whether to
  **Replace** it or **Keep existing**, in the extraction wizard and the
  add-documents dialog, which marks such a file **already there**. Replacing
  removes the old document, with its extracted graph in every mapped knowledge
  graph, and extracts the new one. The same name in another folder is a
  different file.
- Extraction follows more of what a model's property shapes say. Values take
  the property's datatype, so dates, integers and decimals are typed; a value
  that does not fit is kept as written. Unambiguous decimal commas such as
  `4,75` are read as decimals. A property with `sh:maxCount 1` keeps one value
  per record. Mark a property shape `fid:perDocument true` (`fid:` is
  `https://ns.flur.ee/identity#`) when every record a document yields shares
  one value, or `fid:requiredInDocument true` to reject an extracted record
  that does not state it.
- A completed extraction run reports how much of its output has been written
  to its graph, on the run page and as `materializations` on
  `GET /v1/executions/{id}`.
- Administrators can create a read-only API key with **Create key** under
  **Settings > API keys**. The key is shown once, with a copy button. A
  read-only key can read every dataset on the server, and the system records
  of users, teams and settings, not only what one Space or person can see.
  Give one only to a client that may read all of it; a person connecting the
  CLI should use a command-line token, which acts with their own access.
- Google providers accept an optional Base URL, so a Vertex AI express key can
  be used.
- The included documentation adds guides to importing a spreadsheet,
  connecting a warehouse, mapping warehouse tables into a knowledge graph, and
  reviewing with the second opinion.

### Changed

- With no model chosen for a purpose and no default model on an Anthropic
  provider, chat, code and extraction now default to Claude Sonnet 5.5, and
  reasoning to Claude Opus 5.5. This includes a server whose Anthropic key is
  set in the environment or `secrets.toml`, which previously used Claude
  Sonnet 5 for every purpose: its titles and summaries now use Claude Haiku
  4.5, and document parsing's vision reads use Claude Sonnet 5.5 unless a
  Google key or a doc-parse route sends them to Gemini. To keep an earlier
  model, choose it under **Model routing** or set
  `FLUREE_AI_LLM_DEFAULT_MODEL`.
- **Settings > LLM providers** is now **Settings > LLM Configuration**, with
  **Overview**, **LLM providers** and **Model routing** tabs. Keys, including
  the decision model's, live under **LLM providers**, and purposes under
  **Model routing**, grouped as Language, Vision and Decision.
- Gemini 3 requests that use tools, such as chat, agent and task runs, now
  think at a high level unless a lower reasoning effort is set, following
  Google's guidance for tool use. Turns are slower and use more output tokens.
- In your own private Space, where **Ask** panel conversations run, the
  assistant now has what your teams and Spaces are granted and writes
  directly to graphs you own, instead of only proposing changes;
  administrator rights do not carry over to it. A model that requires review
  still only takes proposals. Shared and team Spaces keep the Space's own
  grants.
- In a conversation, the assistant's queries are limited to the datasets
  selected for it, and it can join those datasets in one query. The assistant,
  agents and tasks are told today's date, and when one of the assistant's
  SPARQL queries uses a standard or dataset prefix without declaring it, the
  declaration is added for it.
- Long, multi-step chat turns on Claude models cost less, reading the
  conversation so far from the provider's prompt cache. Cache reads and writes
  are reported in a turn's token usage.
- Model calls are more resilient. A call that fails before it starts, from a
  network error, a rate limit or a temporary provider error, is retried up to
  twice, waiting as long as a throttling provider asks, up to 30 seconds. An
  exhausted account is not retried. Embedding requests and vision reads retry
  temporary failures the same way. Each model call is logged with its token
  usage and duration.
- The extraction wizard grounds a run in the model you picked by default, so a
  first extraction into a new, empty graph finds the model's terms. A model
  with nothing to match is left out of the run with the reason, and the Output
  step warns before starting a run that has nothing to match.
- When extraction is grounded in views over your own records, such as CRM
  contacts, it links what a document mentions to those records. It never
  writes a document's statements onto them or creates a second copy; the
  statements are kept as evidence a reviewer can read.
- During extraction, the step that merges a document's mentions of one entity
  now uses the model routed for **Extraction** instead of the one for **Code &
  mappings**.
- Large documents extract faster: matches that need a model's confirmation, and
  image regions that need a vision read, are handled several at a time. A
  figure-heavy document is no longer refused for having more than 70 image
  regions; the per-document limit on vision reads now defaults to 560. Runs
  over many documents no longer slow down sharply as they grow.
- A document's finds are reviewed as one list of entities, relationships and
  values instead of separate tabs, filtered by kind, source, matcher and
  status, and sorted in document order or by relevance.
- Document graphs have a new shape. The document is a `foaf:Document`
  whichever extractor read it; the basic text extractor wrote a
  `schema:DigitalDocument`. Its parts and embedding chunks are named after
  their file with no further `/` segment, such as `…-section-4` or
  `…-embedding-3`. A list item is the `doco:Paragraph` its `doco:List`
  contains, a section's title is also stated with `po:containsAsHeader`, a
  link's target is an `xsd:anyURI` literal, and table cells no longer carry
  `doc:sourceDocument`.
- Numbers, dates, relative times and name sorting follow the language chosen
  in the app instead of the browser's, keeping the browser's regional format
  when its language matches. Spreadsheet profile summaries and warnings are
  shown in the app's language.
- A query that reads several ledgers runs under one policy that the caller's
  access to every one of them allows; a query across ledgers the caller reads
  under different policies is refused (403). Only administrators, a ledger's
  owner and API keys can use `fluree multi-query` on a ledger; a member who
  reads it through a grant is refused.
- Only administrators can create API keys. An API key or an agent cannot
  create one, and a request for permissions you cannot grant is refused (403)
  rather than narrowed. **Command-line access** shows the token on its own,
  above the sign-in commands, each with its own copy button.
- Disabling or deleting a sign-in provider signs out everyone who signed in
  through it within about 30 seconds, including command-line tokens issued
  from those sessions. API keys keep working.
- A Space app install now serves its app's published version to the Space's
  members unless it is pinned; a public link to the install keeps serving the
  version it holds. `PATCH /v1/app-instances/{instanceId}` pins an install to
  a version, as in `{"version": "1.2.0"}`, or sets it following the published
  version again with `{"autoUpgrade": true}`.
- A new branch can no longer be named `commit`, `txn`, `index`, `config` or
  `blob`, contain `/` or `#`, or end in `.index` or `.snapshots`, and a new
  ledger can no longer be named `commit`, `txn`, `index`, `config`, `blob`
  or `graph-sources`. Existing ledgers and branches are unaffected.
- `PATCH /v1/datasets/{datasetId}` refuses an unknown key, a wrong-typed
  value or an empty body (400), naming the problem, instead of reporting
  success and changing nothing. A nullable setting sent as `null` is cleared,
  and the response lists what it applied in `updatedFields`.
- Starting a resolution run refuses unknown fields such as `dryRun` (a dry run
  is chosen with `mode`), and the response names the graph that holds the
  run's records (`runsGraph`). A run reads reviewers' decisions as of at least
  the graph's latest commit, or a later `minT`.
- Two person records that share a UK mobile number count as full agreement on
  phone when their names say they are one person. A mobile shared by records
  whose names differ still counts as a possibly shared number.
- When your model caps a link with `sh:maxCount`, reconciling extracted records
  keeps two records apart on a differing link only if the shape caps that
  link, so a person's different roles no longer keep two records apart.
- The **Microsoft OneLake** tile connects a lakehouse's `Tables` folder, read
  as Delta, instead of OneLake's Iceberg endpoint, which can still be
  connected with the **Other Iceberg catalog** tile. The warehouse storage
  check answers in one shape for Iceberg and Delta tables; its `bucket`, `key`
  and `region` fields are removed.
- Query errors answer with a status that names their cause instead of 500:
  a query pinned to a commit that does not exist, or naming a graph its
  ledger does not hold, answers 404; a time the ledger cannot answer, such
  as one after its latest commit, answers 400; and a warehouse table whose
  storage refuses the read answers 403.
- Settings, the included documentation and the admin API no longer offer
  hosted dedicated servers, which this server cannot run. A multi-node
  deployment's dedicated transaction servers are unaffected.
- The agent page's Runs tab and `GET /v1/agent-executions` are removed. Agent
  activity is under **Settings > Agent activity**.
- Large files are streamed to disk while they are materialized or staged for
  import, instead of being held in memory.

### Fixed

- Dropping a ledger, including from the command line, requires delete
  permission on it, and system ledgers cannot be dropped. Viewing a merge
  preview or a graph source's details requires read access.
- Queries are authorized against every ledger they read, however the query
  names it. Explain applies the caller's policy, and Cypher queries apply a
  ledger's default policies, as SPARQL and JSON-LD queries do.
- In a shared Space, an agent used by an administrator or a dataset owner now
  reads and writes under the policy classes on the Space's grant, like every
  other member's agent; previously it could read records the Space's policies
  hide. A dataset a Space excludes is refused to its agents in every case, the
  class restrictions on a Space's grants apply to the assistant's reads in
  chat, and a task an app starts keeps the app's narrower access.
- For anyone whose access to a graph is narrowed by policy classes, the data
  model and the graph's statistics count only the records they can see.
  Quality assessments are refused to such callers, because an assessment's
  findings name records the policies hide.
- Readers of an extraction graph no longer see the names of grounding sources
  and classes they cannot read, and adding or changing a grounding source
  requires read access to it.
- Identity-provider configuration lookups and their results are no longer
  written to the server log.
- Opening a Space app no longer writes an identity record into every graph the
  user owns; it is written only where a graph's policies need it.
- Adding an LLM provider in Settings no longer fails. Editing a provider keeps
  its stored settings, and a disabled provider stays disabled.
- Chat with Gemini 2.5 Pro and other thinking-only models on this server's
  own Google key no longer fails on every request, and multi-step tool use on
  Gemini 3 no longer fails on its second step. When a Gemini reply in chat
  garbles a tool call or comes back empty, the assistant asks again once
  instead of showing a blank answer, and document parsing reports the failure
  instead of recording a blank page. Conversations on Gemini are titled by the
  model.
- Routing a purpose to Claude Sonnet 5.5 or Claude Opus 5.5 no longer fails on
  requests that ask for no extended thinking. Token usage reported for OpenAI
  models no longer counts cached input twice.
- A server whose only model provider is an OpenAI or Google key, with no
  default model set, now answers with an error naming what to set, instead of
  sending another vendor's model name and failing as an unknown model. Set
  `FLUREE_AI_LLM_DEFAULT_MODEL` or route each purpose under **Model routing**.
- On a server with no Fluree connection, the assistant's vector search uses
  the server's own OpenAI provider instead of failing. The assistant can write
  to a model it created earlier in the same turn.
- The assistant keeps the queries it ran and the results it received in
  earlier turns of a conversation, so a follow-up can build on them.
- A SPARQL query whose `FROM` clauses name several ledgers now runs, including
  with `fluree query`, and answers in the requested format, including CSV and
  TSV. A query that does not parse is reported with its line and column. On a
  ledger's query route, SPARQL reads the ledger's named graphs: `GRAPH` returns
  rows, and `ASK`, `CONSTRUCT` and `DESCRIBE` run without a `FROM`.
- The assistant's lookups of extracted records match a record by the words a
  document used for it and count each record once.
- Extraction no longer loses text a document holds: a reply that copies a
  table cell's tab no longer discards the passage, text repeated on every page
  is read, and a record whose quoted context spans several lines is kept.
- A value for a property that takes a vocabulary concept is linked to that
  concept even when written as plain text, and a record a vocabulary lists
  under several names is matched by any of them. A relation stated from the
  wrong end is turned round, and a relation from an entity to itself is
  rejected.
- Names inside email addresses and URLs are no longer proposed as entities,
  while a name written as `@Name` is still found. Teaching a missed mention or
  promoting an alias no longer adds a stray node to the vocabulary.
- A resolution run started right after reviewers record verdicts honors them,
  and a pair judged again keeps only its newest verdict. A nickname matches
  every formal name it shortens (Mike matches Michael and Michaela). A link a
  record states twice, once as a record and once by name, counts as one link.
- Publishing golden records, and linking evidence to them, no longer writes a
  stray `insert` property into the golden dataset.
- An extraction run whose document branches had to be recovered no longer
  stalls, and a retried step in a run that fans out, including one revived
  after a restart, runs as its own child run.
- Saving a sign-in provider checks its claim mapping and refuses an invalid
  claim name with the reason, instead of failing at the next sign-in.
- A spreadsheet over 50,000 rows or 32 MB no longer creates a new model and
  then fails its import; **Create the model only** is offered instead. An
  import written in several parts says so, and if it stops part-way, previewing
  again brings in the rest without duplicates.
- People who are not administrators see that only an admin can connect a
  warehouse, instead of a connect button that cannot work.
- Clicking a find on a page image opens its own decision when the text before
  it contains non-ASCII characters.
- Deleting a task also deletes the files stored for it.
- Upgrading a server whose starter Business Model was deleted no longer fails:
  the upgrade steps that update the Business Model skip it.
- After sign-in, **Context** opens the most recently created model, never the
  starter Business Model while another exists, instead of an arbitrary one. A
  model named in the link, or the last one opened in that browser, still comes
  first.
- Dates, times, `xsd:float` values and integer types such as `xsd:long`
  written with SPARQL UPDATE read back correctly once a dataset is indexed,
  and JSON-LD queries and deletes match them. SPARQL UPDATE now refuses a
  value that is not valid for its datatype.
- A SPARQL UPDATE whose `USING` or `WITH` names a graph that does not exist
  reads an empty graph. It read the default graph, so a mistyped graph name
  could delete default-graph data.
- Writes to a named graph are checked against the SHACL shapes and
  uniqueness constraints that apply to it. The checks looked at another
  graph, so a write that broke them could commit; it is now refused.
- The custom connector tutorial and several API examples in the included
  documentation use routes the server serves.

### Upgrade notes

- Back up the data directory before upgrading, and in a multi-node deployment
  the shared store as well; to roll back, restore those backups. All servers
  in a multi-node deployment must run the same version.
- Dates, times, `xsd:float` values and integer types such as `xsd:long`
  written with SPARQL UPDATE by an earlier version read back wrong once the
  dataset is indexed (a date as `1970-01-01`, for example), and JSON-LD
  queries and deletes did not match them. The committed data is intact:
  after upgrading, reindex each dataset written that way, with
  `fluree reindex` or `POST /v1/datasets/{datasetId}/reindex`.
- Everyone signs in again once after the upgrade. Command-line tokens issued
  earlier keep working until they expire.
- Earlier versions let any signed-in user create an API key with any
  permissions, including an administrator's. Only administrators can create
  keys now, but existing keys keep the permissions they were created with, and
  those apply across the server whoever holds the key. **Settings > API keys**
  lists only your own keys, so ask each user to review theirs and revoke any
  they do not need.
- Encryption at rest is off unless you set a key, and existing data is not
  changed. A key can be set only when a store is created: the first start with
  a key on an empty data directory (or empty shared store) marks it encrypted,
  and every later start refuses to run without that same key. A server also
  refuses to start with a key on a store that already holds unencrypted data.
  Every node of a multi-node deployment needs the same key. Keep the key in a
  secret manager and back it up separately: without it, the data cannot be
  read.
- The database's index cache moves from the system temporary directory to
  `<data-dir>/cache`. Allow for the disk space, or set
  `FLUREE_AI_INDEX_CACHE_DIR` to fast local disk. In a multi-node deployment,
  never point it at the shared store. Earlier versions also copied index files
  into `fluree_binary_cache` in the system temporary directory, which can grow
  large; it is no longer used and can be deleted once the upgraded server is
  running.
- Query text is no longer written to the server log by default. Set
  `LOG_QUERY_BODY=true` to log it while diagnosing a problem. Agent activity
  events log at `info`; with a quieter `RUST_LOG`, add `fluree::activity=info`
  to keep them.
- Answer grounding and the resolution second opinion turn themselves on when
  the Fluree connection offers them or a decision-model key is configured.
  They send answers and the data they used, or the compared field values of
  each pair, to that model. To keep this data on your infrastructure, set
  **Answer grounding** to **Off** under **Model routing**; that also stops the
  second opinion unless **Record matching** is routed to a language model. A
  project's **Second opinion for this project** switch, on its **Decision
  model** card, turns it off for that project alone.
- On first start, the starter Business Model's Company legal name switches
  from the `name` match type to `org-name`, replacing any match type set on
  it. Resolution that compares companies by legal name can then match names
  that differ only in accents, dotted initials or the spelling of a legal
  form, such as Limited and Ltd.
- Document parsing has changed, so the first extraction run over each document
  after upgrading parses it again, including any paid vision reads. Values
  extracted earlier keep their form until the document is extracted again.
- Documents extracted by an earlier version keep the earlier graph shape until
  they are extracted again. Update queries, dashboards and apps that read a
  document's structure: its node, the names of its parts and embeddings, and
  the classes described under Changed.
- `POST /v1/document-repos/{repoId}/upload-url` refuses (409) a file whose name
  is already taken in its folder, and so does
  `POST /v1/document-repos/{repoId}/documents/{documentId}/confirm` when the
  name was taken in between, unless the request sets `replace: true`. Clients
  that upload new versions of a file need to send it; the confirm lists what
  it replaced in `replacedDocumentIds`. A replacing upload is a new document
  with a new id.
- The classes whose records a grounding view supplies are now link-only,
  including every class of a dataset a view takes whole: extraction links an
  entity of such a class to the record carrying its name, and leaves it out
  when no record does. If documents should add new instances of a class,
  scope your grounding views so they leave that class out.
- A Bedrock provider that uses an IAM role needs an External ID when it is
  added or its role is changed; require it in the role's trust policy with an
  `sts:ExternalId` condition. Existing providers keep working.
- An app using a Space app token can no longer put a task on a schedule,
  webhook or data-change trigger, or make it a standing task. A person sets up
  a task's triggers. Tasks an app put on a trigger under an earlier version
  keep running as their creator's own agent, which in the creator's private
  Space can now write to every graph they own; review such tasks and remove
  any you do not want.
- Conversations started before the upgrade keep their earlier agent access
  until a new conversation is started.
- Space app installs that were not explicitly pinned start serving their app's
  published version. To keep one on a particular version, pin it with
  `PATCH /v1/app-instances/{instanceId}`.
- The built-in Presentation Builder agent is removed on first start;
  presentations could not be generated on this server.
- Golden datasets that an earlier version published to, or linked evidence
  into, may hold a stray `insert` property, which republishing does not
  remove.
- Requests that were accepted before now get a 400: unknown fields on
  `PATCH /v1/datasets/{datasetId}`, on starting a resolution run and in a
  sign-in provider's attribute mapping, a JSON-LD query wrapped in a top-level
  `query` object or carrying its own `policy` on a source, and creating a
  branch whose name contains `/` or `#`, such as `feature/x`.

### Known limitations

- The encryption key cannot be changed, and an existing store cannot be
  switched between encrypted and unencrypted in place. Encryption does not
  cover ledger names and branch heads, the names of stored files (an uploaded
  document's file name is part of its path), Raft logs and snapshots, index
  build files, secrets, configuration files or server logs. Files being
  imported or processed are decrypted to working copies in the system
  temporary directory, and in a deployment with separate transaction servers
  imports are kept decrypted under `<store-dir>/prepared-imports`; keep both
  on an encrypted volume, and protect the data volume as before. Reads from
  an encrypted store decrypt each file, so measure your query workload before
  and after turning it on.
- Delta tables under a lake root are added by name, not listed. Row samples,
  the mapping refinement tools and publishing are not available for Databricks
  or Delta connections. Every catalog that does not vend storage credentials
  is read with the same server identity.
- Streaming SPARQL queries still read only a ledger's default graph.
- Answer grounding does not count rows or add up columns, so a correct total
  or count can be marked uncertain.
- Email attachments are listed on the document but not read.
- Two people with the same name and no suffix who share a UK mobile number
  can be merged automatically. A number in national form beginning `07` is
  read as a UK mobile whatever its country.
- An import too large for one request is written in parts, each committed on
  its own, as in earlier versions; if a part fails, the parts before it stay
  written.
- `materializations` counts writes that have started, so `total` can grow
  after `pending` reaches 0. Treat output as complete only when `pending` and
  `failed` are both 0 and the count has stopped changing.
- Agent transcripts and the activity index are not removed automatically.
- Entities and relationships extracted from a document are shared with other
  documents that mention them, so they stay when the document is extracted
  again, replaced or deleted.
- For someone whose access is narrowed by policy classes, the per-property
  statistics of a class they can see still count all of that class's records.

## [0.10.0-rc.3] - 2026-09-23

### Added

- Windows x64 developer downloads and a PowerShell installer, with foreground
  server startup, local setup, and offline licensing. Local data storage requires
  no Windows Developer Mode or administrator privileges.
- Supply a license directly through `FLUREE_AI_LICENSE` for container and cloud
  secret configuration without a license-file mount.
- Guided setup accepts a license key or license file and installs it during setup.
- Settings given to `init` as flags, such as `--data-dir` or `--license-file`, are
  saved in the new configuration.
- Setup finishes with a summary, next steps, and a pointer to command-line help.
- Dedicated transaction servers: transactions and indexing can run on their own
  pool of servers, separate from the servers that run the app and background
  processing. Adding app servers never changes the transaction pool. An
  asynchronous transaction's status stays available for about an hour after it
  finishes.
- The included documentation describes multi-node deployments: member clusters,
  query peers and dedicated transaction servers, with their storage, network and
  upgrade requirements.
- A new mapping screen brings an uploaded spreadsheet (Excel or CSV) or a data
  lake's tables into a governed dataset. You first choose where the data goes:
  a new model built from the file, or an existing model.
- Build a model from a spreadsheet. The Model Drafter proposes, for each sheet,
  the class it becomes, the column that identifies a row, and what each column
  means, with its reasons. The Model Reconciler finds sheets that describe the
  same kind of thing and columns that mean the same across them. Nothing is
  applied until you accept it, and you can edit the draft before saving.
- Map a spreadsheet onto an existing model. A fit check shows where the file's
  records break the model's rules, and the Import Resolver proposes a fix for
  each: a model change, a value map, another column to fill the property from,
  or leaving those records out. Unmapped columns are matched to properties the
  model already has under another name.
- Import an updated spreadsheet again to update your data. Records are matched
  by their identifying column, so changed rows update the existing records
  instead of creating duplicates, and a cell that was emptied clears its value.
  A row with no identifying value is rejected rather than given a made-up
  identity.
- Value maps translate a column's codes to the model's values, and a separator
  splits one cell into several values.
- People who can only propose changes import onto a branch and can propose
  merging it into `main`.
- A dataset governed by a model is shown and browsed in Knowledge using that
  model's classes.
- Agents can declare an input and output schema and be called like a function:
  structured input in, one JSON result out, checked against the schema on the
  server.
- Workbooks that are really documents, such as a policy index, are extracted as
  documents: each sheet becomes a page and each block of cells a table.
- Extracted pages record their printed page number and display size, including
  rotated pages.
- Anyone who can read a dataset can open a source document's original file and
  its individual pages, so an app can show the page a citation points to.

### Changed

- Imports into a governed dataset are much faster.
- Model dialogs offer saving to `main` only to people who can write to it.

### Fixed

- Catalog and identity-provider credentials sent in requests are no longer
  written to server logs.
- Word, Excel and PowerPoint files saved by tools such as Excel Online are
  recognized and extracted instead of failing as generic archives.
- A dataset set to structure only is no longer materialized when a run starts
  from a mapping.
- A processing step that fails permanently is no longer retried.
- Writes during heavy loading wait for indexing to catch up instead of failing.
- Binding a model that has no access policies no longer locks everyone out of
  the dataset. The dataset allows access until the model has policies, and a
  warning says so.
- Administrators and dataset owners see all data in a governed dataset;
  previously their reads could return nothing.

### Known limitations

- A spreadsheet import handles up to 50,000 rows. A larger file is refused
  before anything is written, never partly imported.

### Upgrade notes

- Each server converts its Raft coordination logs to a new format on its first
  start, and earlier versions cannot read the converted logs. Back up the data
  directory before upgrading; to roll back, restore that backup.
- All servers in a multi-node deployment must run the same version. Stop them
  all, upgrade them together, then start them.

## [0.9.25-rc.3] - 2026-09-11

### Added

- Downloads for Apple Silicon Macs and Linux on x86-64 and ARM64.
- Standalone command-line application with guided configuration for login
  settings, AI provider keys, and server ports.
- Setup identifies optional AI provider keys and their uses, recognizes keys
  already set in environment variables, and keeps those values out of saved files.
- Foreground and background server commands, including status, logs, and shutdown.
- An included web app and customer documentation.
- License installation and status commands, with support for renewing a license.

### Fixed

- AI provider keys can be saved and updated in the app on standalone servers,
  with saved keys taking precedence over environment defaults.
- Startup reports an occupied HTTP address before initializing the database or
  starting background work, with guidance for choosing another address.
- Missing-license startup messages explain how to install a license and contact
  support.
- Archive upgrades use the web app included in the new executable and preserve
  explicitly configured custom web directories.
- Superseded copies of the embedded web app under the data directory are removed
  automatically instead of accumulating across upgrades.
