# Design system — benbel

Extracted from 22 repositories (October 2026): `eau`, `av`, `af`, `geographie`, `climat`,
`arpenteur`, `alpes`, `24ri`, `thalweg`, `embeddings`, `causeries`, `cinema`, `meteo`,
`benbel.github.io`, `cours`, and, for the history, `ggipp` (2020), `random` (2022), `spirographe`,
`cassettes`, `demographie` (2023), `lexflation` and `polynome` (2026). About 20 other repositories
were not read (`these`, `nlp`, `plf`, `ssf`…), so some rules may not hold for them.

How to read it:

- **Rule**: stated in writing in at least one repository and followed by the recent ones
  (`eau`, `av`, `geographie`, `climat`, `arpenteur`, all from July to October 2026).
- **Practice**: not written anywhere, but repeated across repositories.
- **Drift**: places where repositories disagree. §14 lists them with a recommendation for each.

Sources are given as `repo/path` so each rule can be checked.

**What this document is.** It records a working conversation, not only its result, and it will be
used to build the finished standard. It has four parts:

- **Part A, sections 1 to 17**: the design system as extracted from the repositories, with the
  rules agreed during the conversation already folded in.
- **Part B, the working log** (after §17): every question asked, what was found, what was proposed,
  and the status of each point (*decided*, *proposed*, *open*). Proposals in Part B are not yet
  merged into Part A.
- **Part C**: the open decisions, consolidated.
- **Part D**: the plan for the finished project.

Where Part A and Part B disagree, Part B is more recent.

How old each layer is. Most commits since July 2026 are co-written with Claude, so the dates tell
your own constant habits apart from rules that are recent and still settling.

| Layer | Since | Evidence | Status |
|---|---|---|---|
| Bracket layout with the operator at the start of each line (`+`, `%>%`, then `\|>`) and the closing bracket at the content's indent, even in CSS | 2020 | `ggipp` (2020), `random/rart` (2022), `spirographe` and `cinema` (2023) | the most constant habit |
| An institution's chart packaged as a shared theme | 2020 | `ggipp` (`theme_ipp`, IPP palette), then `srpp` (`af`, `av`), then `theme_tufte_fr` | stable; theme minimalism hardened over time (`ggipp` keeps the grid) |
| Generative drawings: `theme_void`, thin strokes, many seeds shown side by side in a grid | 2022 | `random/rart`, `spirographe`, then `alpes` and `thalweg` | stable; `alpes`'s aesthetic charter puts it in writing |
| Monospace pages on `#fcfcfc`/`#111`, `<details>` disclosure, static pages built by scheduled CI | 2023 | `cinema`, `cassettes` (2023), `benbel.github.io`, `meteo`, `lexflation` | stable |
| Analysis framed as a question with counterfactual scenarios | 2023 | `demographie` (« quel phénomène a l'effet le plus important », hypotheses named after people), then `af`, `av` | stable |
| plotnine in Python | 2023 | `demographie`, then `geographie` | stable |
| **No comments at all** | mid-2026, Cour repositories only | `ggipp` (roxygen), `rart`, `spirographe`, `cassettes`, `demographie`, and still `lexflation` and `polynome` in 2026, all have comments; `cinema` is the exception | **recent**, not a lifelong habit |
| Native `\|>` instead of `%>%` | 2026 | `%>%` until 2023 (`rart`, `spirographe`), `\|>` in `eau`, `av`, `climat` | recent |
| `gt.cls`: Tufte margin, sidenotes, Garamond | 2023 | class header dated 2023/10/05 | stable; rubrication restored in 2026 |
| Written firm rules, a single README, one-argument scripts, human coding with model suggestions | July–October 2026 | `eau`, `av`, `af` | recent |
| Tufte chart theme, rubric red in charts, Palladio on screen, TikZ grammar, bilingual Quarto with profiles | September–October 2026 | `geographie`, `arpenteur`. As late as April 2026, `cours` still used Quarto and matplotlib defaults | recent, still to confirm |
| Product principles (§2), one script with no options | September–October 2026 | `eau` diagnosis, `climat/run.sh` | recent, mostly inferred |

---

## 1. Principles

1. **The data comes first, the effect second.** Remove all ink that is not data, in charts, pages and
   documents alike (`eau/README.md` « Graphiques », `climat/docs/PLAN.md` §8, `av/README.md`).
2. **Plainness is a requirement, not a taste.** No clever code, no hidden machinery, no decoration.
   "If a reader has to run the code to find out what it calls, it is wrong." (`climat` §8).
   Breaking a convention "is a defect to correct, not a personal style" (`av/README.md`).
3. **One documentation surface.** Code carries no comments. What needs saying goes into the README,
   the note or the prose of a notebook, and that prose must make sense on its own.
4. **Generated files are never edited by hand.** Every human decision is data (an append-only log,
   a declaration file, a grid in `config.py`) that the pipeline replays (`arpenteur` corrections,
   `eau` `extraits.csv` and `ANGLES_RENOMMES`).
5. **Say what it costs.** Each choice is written down with its price ("Le prix est réel — sans
   quadrillage on ne relève plus une valeur à l'œil", `av`), its failure modes and its non-goals
   (`alpes/ROADMAP.md` « Deliberately not on this list »).
6. **Measure, don't assert.** Results come with a denominator and a reference (`arpenteur` README
   table: `249 / 260`), and a change is judged against a test ("Gate", `alpes/ROADMAP.md`) or a
   blind A/B duel (`alpes`, `arpenteur/tools/fonts`).
7. **Fail loudly and say what to do.** A missing input stops the run and names the command to run
   (`"... est absent : lancer d'abord python code/analyses/figures.py."`, `eau/figures.R`). A tool
   refuses to open on a broken invariant and names the cause (`eau` coding tool: more than ten
   categories).
8. **A model may prepare the skeleton of a text, never write it.** For a note, a handout, course
   prose, an article or any text that is delivered, an LLM may propose the plan, the headings,
   the list of figures and tables, empty placeholders, and the macros the numbers will fill. Every
   sentence of the text itself is written by a person. See §3.4.
9. **Local and self-hosted when data is sensitive.** Use only self-hosted Hugging Face models and
   never a commercial LLM (`eau` « Règles fermes »). Tools that show verbatims run locally and are
   never deployed. Statistical secrecy is enforced in code (`av`).

### 1.1 Named principles: which ones apply

Where a rule here matches a known principle, cite the principle; it gives the rule a reason and a
standard to be checked against. Status: **follows** (the repositories already do it), **adopt**
(it fills a gap found in §14), **partly** (it applies with a stated departure).

| Principle | Source | Status | Where |
|---|---|---|---|
| Data-ink ratio, no chartjunk, small multiples, direct labels, range frames, sidenotes | Tufte, *The Visual Display of Quantitative Information* (1983) and *Envisioning Information* (1990) | follows | §7, `gt.cls`, `arpenteur` book. Departure: Tufte erases ink that is not data, not information; removing the value axis entirely (`eau` « sans axe des notes ») goes beyond him |
| Ranking of visual encodings: position > length > angle > area > colour | Cleveland & McGill (1984) | follows, unstated | dot and interval plots rather than bars or pies; colour only as accent |
| Visual variables (position, size, value, texture, colour, orientation, shape) | Bertin, *Sémiologie graphique* (1967) | follows, unstated | TikZ grammar: value (grey) for input, colour (red) for output, texture (hatching) for missing |
| Grammar of graphics | Wilkinson (1999); Wickham, ggplot2 (2010) | follows | ggplot2 and plotnine everywhere; one theme, layers led by `+` |
| Tidy data: one variable per column, one observation per row, one table per kind of unit | Wickham, *Tidy Data* (2014) | follows | `climat` (« one tidy dataset per source », `measurements` and `variables` tables), `eau` `extraits.csv`. Departure: the delivered tables in notes are wide, for reading, and that is right |
| IMRAD: introduction, methods, results, discussion | scientific article convention | follows in notes | `eau/note.tex` (introduction « sans résultat », données et méthodes, résultats, discussion et limites). **Partly**: a reader who decides (a magistrate, a director) needs the answer first; add a one-paragraph synthesis before the introduction (pyramid principle, Minto 1987), the body staying IMRAD |
| Good enough practices in scientific computing; ten simple rules for reproducible research | Wilson et al. (2017); Sandve et al. (2013) | **adopt** | one script that rebuilds everything, raw data kept verbatim, seeds and versions recorded, every output traced to its inputs: §5.4, and the missing tags and manifests in §14 |
| Literate programming | Knuth (1984) | follows | Quarto notebooks: the prose carries the explanation, the code none |
| Do one thing well; separation of concerns | Unix philosophy (McIlroy, 1978) | follows | §2.3 and §2.4: one tool, one job, one file between tools |
| Gall's law: a complex system that works evolved from a simple one that worked | Gall, *Systemantics* (1975) | follows | `24ri` before `arpenteur`; vertical slices (`climat`) |
| Rule of three, YAGNI | software folklore (Fowler, Beck) | follows | abstraction only for real duplication (§4) |
| Statement on p-values; « the difference between significant and not significant is not itself significant » | ASA (2016); Gelman & Stern (2006) | follows | intervals, not stars (`av`) |
| Typographic measure, old-style figures in text, lining in tables | Bringhurst, *The Elements of Typographic Style* | follows | 35–45 em columns, `Numbers=OldStyle`, `lining-nums` for data |
| Usability heuristics: user control and undo, error prevention, recognition rather than recall | Nielsen (1994) | follows | undo, refuse to open, keys shown on each box |
| WCAG 2.2: text contrast at least 4.5:1, visible keyboard focus, text alternatives | W3C (2023) | **adopt** | `#999` text and `outline: none` in `eau` fail it (§14); `polynome` already does it right (`:focus-visible`, `aria-pressed`) |
| Statistical disclosure control: frequency and dominance rules | Insee, CASD | follows | `av` (11 households, 85 %); `eau` and `geographie` (secondary suppression) |
| PEP 8, `black`, the tidyverse style guide | Python, R | **departs on purpose** | bracket layout and no comments (§4); say so rather than pretend to follow them |
| FAIR data | Wilkinson et al. (2016) | not applicable as a whole | the data is mostly confidential; licences and attributions (§14) are the part that applies |

---

## 2. Deciding what to build

The repositories rarely state these principles, but they show them in their decisions, and once
explicitly: the diagnosis that closed `eau`'s first pipeline (`eau/README.md` « Plan d'analyse
révisé »). Where a repository went against a principle, this section says so.

### 2.1 Start from the artefact and its reader

Every project ends in one artefact that someone reads, and the code exists to produce it:

- `eau`: a note for the Cour des comptes;
- `av`: three notes;
- `geographie`: a course and its handout;
- `arpenteur`: a book and a reading page;
- `alpes`: a drawing on paper.

Before writing code, write down three things:

- who reads the artefact;
- what they decide with it;
- the list of what it contains: in `eau`, the « 15 demandes » specification and the plan of the
  note.

Anything that feeds none of these items is exploration. Exploration is allowed, but §2.6 sets a
time limit on it.

### 2.2 Script, notebook, package, tool or page: take the lowest rung that works

| Need | Form | Example |
|---|---|---|
| Answer one question once | a script that prints to the screen | `eau/croisements.py` |
| Produce a number, table or figure that goes into the artefact | a script that writes a file the document `\input`s | `eau/figures.py` → `output/figures/` |
| Explain while computing | a Quarto notebook | `geographie/notebooks` |
| A method reused on other inputs or by other people | a package with a CLI | `arpenteur`, `alpes` |
| Many human judgements on data | a local interactive tool | `eau/code/applications`, `arpenteur review` |
| Let someone read or explore a result | a static page, with no server | `arpenteur` web and book, `eau/carte.html`, `thalweg` |
| Information that changes on its own (showtimes, weather, the size of the law) | a static page rebuilt by a scheduled GitHub Actions job, deployed to Pages; still no server | `cinema` (daily at 1 am), `meteo` (every 6 hours), `lexflation` (monthly, the bot commits the new data) |
| An instrument (something played, not read) | a static page whose controls are the product | `polynome` (step sequencer). The "no controls" rule of §8 does not apply; accessibility does, and `polynome` is the model for it |

An interface is justified only when **a human judgement is the bottleneck**. It is never justified
for showing results that a figure or a table already shows. There are no dashboards anywhere in
these repositories, and that is deliberate.

### 2.3 One application or several

**Split by mode of attention, not by feature.**

- `eau` has two tools over the same data:
  - `decoupage` shows every answer on one page, in three columns, for building and revising the
    coding grid;
  - `codage` shows one excerpt per screen, driven by the keyboard, for coding quickly.
- `arpenteur` has three faces on one pipeline:
  - `reader`, to read;
  - `book`, to study and print;
  - `review`, to correct.

The same data is used in each case. What differs is the attention the person brings to it.

**Split by audience and lifecycle.**

- `af` keeps `reformes/` (for the Cour) and `papier/` (for a journal) apart: they share the
  counterfactual simulation trunk and nothing else.
- `thalweg` (the published gallery) is a separate repository from `alpes` (the drawing engine).
- A separate repository is justified when the reader, the data access (the CASD), the publication
  or the release rhythm differs. Otherwise use a folder that follows the same layout as its
  siblings, as `av`'s three chains share `scripts/ output/ note/`.

**Behind several faces, keep one source of truth.** One `corrections.jsonl` and one
`extraits.csv`: no interface keeps its own state.

**Never two writers on one file.** `eau`'s two tools both write `extraits.csv`, and the README says
« ne jamais les ouvrir en même temps ». That is a rule enforced only by the user's memory, which
contradicts « refuser d'ouvrir et nommer le remède ». The fix is either one application with two
modes, or a lock file: the second tool refuses to open and names the first.

**Separate heavy computation from the interface.** `predire.py` runs on its own, and the tools read
`predictions.csv` without ever waiting on a model.

**Generalise after a concrete case, never before.**

- `24ri` was written for one book. `arpenteur` generalised it, was measured against `24ri`'s
  hand-corrected annotations, and only then took on *Moby-Dick*, Verne and Thucydides.
- No framework is built before two real cases exist.

### 2.4 What a tool must do, and what it must not

**It must:**

- **do one job from end to end**: input, human decisions, a file that the pipeline replays;
- **keep its state on disk**: reloading resumes the work, a copy is made when it opens, and every
  action can be undone;
- **show each item in its context**: the whole answer, with the excerpt in full ink;
- **offer suggestions without applying them**:
  - the suggestion is preselected and drawn in dashed lines, never applied silently;
  - the record says whether the human *validated* it or *wrote* their own (`validé` / `écrit`);
- **order the work by what needs a human**, in an order fixed when it opens;
- **refuse a broken invariant and name the fix**: in `codage`, ten categories at most, one per key;
- **run locally** when it shows sensitive text: no account, no server, no deployment.

**It must not:**

- **edit the source**: « Le texte n'est jamais modifiable : seules les coupures sont retenues ».
- **export**: export is the pipeline's job.
- **add things nobody asked for**: settings panels, filters, a home page.
- **depend on the network for its core job**: the `arpenteur` book has no tiles, and the review
  tool falls back to Natural Earth.

**Measure** a tool by the number of actions per decision: one key per choice, `Entrée` to move on.

### 2.5 Automating: when a model, when a human

This is `eau`'s lesson. The first pipeline coded the open questions with an LLM, then stacked on
clustering, voting, inter-model robustness, κ and Rogan-Gladen. It was abandoned. Its diagnosis is a
set of rules:

- **Volume decides.** About 100 respondents and 50 to 90 verbatims per question: « un humain lit tout
  en quelques heures ; l'automatisation ne fait rien gagner sur la lecture » (a human reads them all
  in a few hours; automating gains nothing over reading).
- **Measure the object, not the machine.** Agreement between models and fidelity to their own
  construction say nothing about what respondents said. Compare to a human first, before adding any
  correction layer. Against the human coding, the first pipeline scored a Jaccard of 0.26 on q65.
- **The precision you can reach is bounded by non-response, not by the coder.** Calculate that bound
  before automating, because it can make automation useless: a theme can only be placed somewhere
  between 22 % and 49 %, so « 34 % » is a false precision.
- **A model assists and never produces a published number.** It can suggest or flag; every
  published number comes from a deterministic calculation or from human coding.
- **Classify the work as essential or accessory, and certain or risky.** Do the essential and certain
  work first. Drop the accessory and risky work. Never start with the riskiest product of the
  project.
- **Archive abandoned paths** in `old/` with their README, and make sure they feed nothing.

### 2.6 How to develop

- **Build vertical slices**: each phase produces something real from end to end (`climat` §9).
- **Write each work item as Wrong / Done / Gate, with a size S, M or L** (`alpes/ROADMAP.md`). The
  gate is a measurable test, written before the work starts.
- **Judge with a measurement against a reference** when a reference exists (`arpenteur`: 249 dates
  out of 260 on `24ri`).
- **Judge with blind A/B duels** when taste decides (the `alpes` optimizer, the font tournament). One
  question per duel.
- **Write the non-goals down** (« Deliberately not on this list »).
- **Name the riskiest part early, but build the certain part first.**
- **Cut along seams that already exist** when a file grows: `render.py`, at 2,232 lines, was split
  along the stages its own docstring named.
- **Time-box exploration.** A script that prints to the screen either becomes an output of the
  artefact or goes to `old/`. `eau` has more than a dozen screen scripts whose results are copied by
  hand into the note; that is the sprawl this rule prevents.
- **Hand over with a codebook.** A tool that a second person will use (`eau`'s second coder) needs
  written definitions and examples before it is handed over (`livre_de_codes.md`, still empty).

## 3. Writing and documentation

### 3.1 Documents and their roles

| File | Role | Examples |
|---|---|---|
| `README.md` | **The only documentation.** How to run, layout, design, firm rules, pitfalls already fixed, what remains | `eau`, `av`, `arpenteur`, `alpes` |
| `CLAUDE.md` | Short: style rules and commands, or simply "read the README first" | `arpenteur`, `eau` |
| `PLAN.md` | Working plan with status tags `[verified]` `[to verify]` `[decision]` `[open]`, a dated revision log at the top ("this summary wins"), a decisions log, open questions, risks | `geographie`, `climat/docs` |
| `ROADMAP.md` | Numbered items sized S/M/L. Each has **Wrong.** (the diagnosis), **Done.** (what landed) and **Gate.** (the measurable acceptance test), then "Deliberately not on this list" and "How to judge what landed" | `alpes` |
| `livre_de_codes.md`, `data/README.md` | Codebook and data sources: licences, versions, download dates | `eau`, `geographie` |

### 3.2 README structure (practice)

- An opening paragraph that says what the project does and what it produces. For a domain project
  it also states the mechanism at stake: `av` opens with the 70-year tax threshold.
- **Run** (`## Lancer` / `## Use`): a single code block of every command, each with a short trailing
  comment saying what it writes and where.
- **Layout** as a table: `| Folder | Contents |`, then `| Script | Role |`. Each role says how many
  arguments the script takes, what it reads and what it writes.
- **Firm rules** (`## Règles fermes`): bold imperative statements.
- One section per tool or stage, describing behaviour from the user's side: keys, clicks, files
  written.
- Pitfalls written as findings: "Trois choses que le dictionnaire a démenties", "Known errors",
  "Known soft spots", with the fix or the reason it is left open.
- **What remains** (`## Ce qui reste à faire` / `## Ce qui reste ouvert`): ordered, each item with
  the reason for doing it.
- **Tests**: one command, then a sentence listing what the tests cover.

### 3.3 Prose voice

- Declarative, concrete and dense. Units and numbers are exact ("9,0 cm donnent 3 543 pixels").
- Headings are claims more often than labels: "`save_srpp` compte en centimètres", "The light is not
  a sun", "Why this is not just a spatial join".
- Explain the *why*, including history ("Avant, une page rechargée repartait de l'état du
  lancement…") when it prevents the mistake from being repeated.
- No filler and no self-reference.
- **Language follows the audience.** French for French public-sector work (`eau`, `av`, `af`,
  `geographie` course material, file names included). English for tools and international work
  (`arpenteur`, `climat`, `alpes`). A plan can be in English while the course it describes is in
  French (`geographie/PLAN.md`).

### 3.4 Analytical writing (notes)

**Who writes.** A model may prepare the skeleton of a text: the plan, the headings, the list of
figures and tables with what each must show, empty placeholders, the citation keys, the macros the
generated numbers will fill, and checks against the text (a number that disagrees with its source,
a figure never cited). It never writes the text: no sentence of a note, a handout, course prose, an
article or a caption comes from a model, not even as a draft to be edited. This extends the rule of
§2.5 (a model never produces a published number) from numbers to words. In practice, a skeleton
uses blocks that are visibly empty:

```latex
\section{Résultats}
\label{sec:resultats}
% À RÉDIGER : part des départements qui évoquent chaque enjeu ; figure \ref{fig:textes} ; bornes de non-réponse
```

(The `% À RÉDIGER` line is the one comment allowed, and only until the person writes the section.)
This rule covers delivered texts; documentation for the repository (README, PLAN, commit messages)
is not in its scope unless you decide otherwise.

From `eau/README.md` « Règle de rédaction » and `av/README.md` « Conventions graphiques »:

- Every number or claim rests on a reproducible measure (closed questions, coded annotations). No
  claim comes from reading raw free text directly.
- Do not comment on the note itself ("la note écrit", "le résultat le plus solide").
- Do not interpret beyond what the numbers say. Claims carry a tier (descriptive, conditional,
  causal). The `climat` publish layer is specified to reject causal wording at tiers 1 and 2.
- Report intervals, not significance stars. The bounds distinguish "an established small effect"
  from "nothing established".
- Use vocabulary precisely: a *décile* is a threshold, a *dixième* is a group. Use the same
  quantities everywhere ("Trois grandeurs à ne jamais confondre").
- With fewer than about ten values, use a **table** rather than a figure.
- State what is suppressed by statistical secrecy and why. Keep the masking rule explicit
  (`av`: fewer than 11 households, or one household above 85 %).

---

## 4. Code style: universal

The rule is written in `eau`, `av`, `climat`, `geographie` and `arpenteur`.

- **No comments anywhere**: no `#` comments, no docstrings, no roxygen headers, and no comments in
  HTML, CSS or JS. A line that seems to need a comment needs a better name or a smaller function.
- **Nothing clever.** No metaprogramming, metaclasses or decorators beyond `@dataclass`, no walrus
  operator, no nested comprehensions, no monkeypatching, no dynamic attributes. In R: no `assign()`,
  no `do.call()` chains that hide the callee, no non-standard evaluation beyond ordinary
  dplyr/data.table use, no generated function names.
- **Right level of abstraction**: neither spaghetti nor factories or wrappers. A helper earns its
  place by making the call site read better. Three similar blocks do not automatically become a
  function, and a function used once is suspect unless it names a step.
- **Functions read top to bottom**: load, validate, transform, return. Early returns for degenerate
  cases, then the main path unbroken.
- **Full words in names**: `population_weighted_mean`, not `pwm`. Single letters only for a scope of
  a line or two with a conventional meaning. Names are the whole documentation budget.
- **Name intermediate values**, and make the data flow explicit.
- **Declarations are data, never behaviour.** YAML, TOML and config dicts name things and never hold
  code or function names. Dispatch is an explicit `switch()` or `if` on a declared type.
- **Never guess a column name.** Columns and question numbers come from a reference file
  (`data/reference/questionnaires.md`) and are declared in one place (`config.py`).
- **Bracket layout**: a multi-line expression opens a bracket, puts one element per line, and closes
  the bracket *at the indentation of the content*:

```python
result = (
    value
    .first_step()
    .second_step(x, y)
    )

parser.add_argument(
    "path",
    nargs="?",
    default=".",
    )
```

```r
resultat <- (
  table
  |> filter(annee == 2023)
  |> summarise(total = sum(montant))
  )

figure <- (
  ggplot(donnees, aes(x, y))
  + geom_point()
  + theme_tufte_fr()
  )
```

This layout is incompatible with `black`, `ruff format` and `styler`. Lint, but never run a
formatter.

### 4.1 Python

- Lint with `ruff check`, `line-length = 100`, `target-version = "py311"`, and
  `select = ["E","F","W","I","B","UP"]`. Allow `E501` in tests. Never run a formatter.
- `pathlib`, `ROOT = Path(__file__).resolve().parents[n]`, constants in `UPPER_CASE` at the top.
- Plain functions and `@dataclass`. Direct pandas operations and explicit transformations.
- `pytest`, with tests split by subject and fixtures in `helpers.py` (`alpes`). Use synthetic data so
  the tests run without protected data.
- Type hints: optional. `eau` uses them and `arpenteur` does not (see §14).

### 4.2 R

- Native pipe `|>` only, **at the start of the line**, with the chain wrapped in brackets. Never
  `%>%`.
- ggplot layers start the line with `+`, inside brackets.
- Two-space indentation.
- File extension `.r` (`av` rule, used by `climat` and `af`). Note that `eau` and `geographie` use `.R`.
- `targets` + `tarchetypes` + `renv` for pipelines. The project is laid out as a package (`R/`,
  `_targets.r`, `tar_source()`).
- Scripts find their own location, so they run from any working directory (`ICI` in `eau/figures.R`;
  `af/config_ines.r` derives the repository root and keeps a single machine-specific literal).
- Load packages quietly: `suppressMessages({ library(ggplot2) })`.

### 4.3 Scripts and command line

- **Analysis scripts** take **at most one argument** (the data export) and **no options**. With no
  argument they read the most recent export in `data/` (`eau` « Règles fermes »).
- **Tools** (`arpenteur`, `alpes`) have a subcommand CLI (`init`, `ingest`, `review`, `build`,
  `check`, `train`) with few flags. `check --strict` fails while human work remains.
- A script reports to the screen ("à l'écran") or writes into `output/`, and says which in the README.
- **One script reproduces everything, with no options.** Each repository has a single entry point
  at its root, such as `run.sh`, that goes from a clean checkout to every output: environment, tests,
  data preparation, analyses, figures, then the compiled note or rendered site. It takes no arguments
  and no flags. When an input can't be downloaded (an enclave export, a file delivered by hand), the
  script detects that it is absent, builds everything else, and prints what was skipped and why. A
  variant such as synthetic data or the student version is either a separate script with its own
  name, or a step the script always runs. The README's Run block starts with this script; the
  individual commands come after it, for working on a single step.

---

## 5. Architecture

### 5.1 Pipelines in named stages

Each project is a short chain of stages with a fixed contract between them.

| Repo | Stages |
|---|---|
| `arpenteur` | ingest → extract → correct → resolve → publish (web, book, data) → review → train |
| `climat` | download (verbatim, checksummed) → transform (one tidy dataset per source) → join (change of support) → analyse |
| `alpes` | `(params, DEM) → Scene → Picture → drawing` |
| `eau`, `af` | Python or R computes and writes CSV and `.tex` files → R draws the figures → XeLaTeX compiles the note |
| `av` | for each chain (`ehvp/`, `ficovie/`, `grande_transmission/`): `scripts/` → `output/` → `note/` |

Each phase is a vertical slice through all the stages, so something real works at the end of each
one (`climat` §9).

### 5.2 Layout (practice)

```
README.md            the only documentation
CLAUDE.md            style rules and commands, or a pointer to the README
PLAN.md / ROADMAP.md
code/ or scripts/ or src/<pkg>/
data/                never versioned (except data/reference/)
data/reference/      versioned reference material: questionnaires, crosswalks
output/              never versioned; figures/ inside it is read by the note
note/                note.tex + gt.cls (+ litterature/)
tests/
old/                 archived pipeline, with its own README; not maintained
```

### 5.3 Human-in-the-loop data

- Corrections are an **append-only JSONL log** that is replayed on every build. Each entry is
  anchored on the exact text plus a prefix and a suffix, so it survives edits elsewhere. Every
  action can be undone (`arpenteur`).
- When an input changes, the existing human work is **realigned** onto it. The tool refuses to
  overwrite if a piece of that work would be lost, and lists what blocks it (`eau` `decouper.py`).
- Take a backup copy when a tool opens (`extraits.csv.sauvegarde`).
- To rename a category, declare the old name (`ANGLES_RENOMMES`) instead of re-coding.

### 5.4 Reproducibility

- Pin versions (`requirements.txt`, `renv.lock`, packages pinned by commit). Fix the seed
  (`set.seed(2026)`).
- Cache external calls (geocoders, tiles, DBnomics) and date the cache. A cache column says when the
  data was downloaded, not when the file was written.
- Each raw file has a checksum. Running the pipeline again downloads nothing.
- Data contracts (`pointblank`, typed variables). A deliberately mis-tagged variable must fail the
  build.
- Running from a clean checkout regenerates every output.
- **Version sources, not renders.** `.qmd`, `.tex`, scripts and templates go in the repository;
  HTML, PDF and figures are built by the root script or in CI and published from there (GitHub
  Pages through Actions, as `cinema` and `meteo` do). `cours` shows the failure: 41 MB of rendered
  HTML (10 MB of it embedded folium maps), with no `.qmd`, so nothing can be rebuilt or corrected.
  A version that has been delivered is identified by a tag, not by a committed PDF.
  That run is one command: the root script of §4.3.

### 5.5 Dependencies

- Prefer the mature domain ecosystem: R for French official statistics (`sf`, `terra`,
  `exactextractr`, `fixest`, `btb`, `COGugaison`, `happign`), and Python for NLP, ML and deep
  learning.
- Python and R in the same project are fine when each does what it is best at (Python computes,
  R draws in `eau`). For teaching, R and Python sit side by side, with a parity table that states
  the gaps.

---

## 6. Colour and type tokens

### 6.1 Colour

| Token | Value | Use | Where |
|---|---|---|---|
| page | `#fcfcfc` | background of every personal page | github.io, causeries, cinema, embeddings, thalweg, arpenteur |
| ink | `#111111` | text | same |
| quiet | `#6e6e6e` / `#4D4D4D` (charts) | secondary text, axes, numbering | arpenteur, theme_tufte |
| faint | `#bdbdbd`, `#d6d6d6` | inactive marks, borders | arpenteur book |
| land / rivers | `#efefef` / `#b8c8d2` | map background | arpenteur |
| **rubric red** | `#9E1B1B` (canonical); also `#8B1A1A`, `#8f0e21` | *the* accent: headings, places, the highlighted series | gt.cls (2026), Quarto CSS, theme_tufte, arpenteur, 24ri |
| link green | `#286c44` | links and focus on the personal site and tools | github.io, causeries, thalweg |
| teal | `#03565E` | single accent in the `eau` system (figures, map, Shiny) | eau |
| time blue | `#00445e` | dates and time (paired with red for places) | 24ri |
| divergent low | `#3B5B7A` → `#F2F2F2` → rubric | diverging scale | theme_tufte |
| sequential | `#F2F2F2` → rubric | sequential scale | theme_tufte |

Rules:

- **One accent colour per meaning**, with grey for everything else. Places are red. Dates are
  italic (or blue). The current item is in full ink and the rest is quiet.
- Missing or non-response data is shown as **hatching or white**, never as a colour of the scale.
  "Doesn't know" is grey.
- In institutional deliverables the institution's chart (`srpp`) takes precedence over this palette.

### 6.2 Type

The typeface follows the genre of the page: monospace for utilities and lists (personal site,
`cinema`, `cassettes`, `causeries`; `lexflation` uses JetBrains Mono), Palladio for reading and
annotating (`arpenteur`, `eau`, Quarto), a system sans-serif in lower case for an instrument
(`polynome`). Each genre keeps its own accent: green on the personal site, rubric red for documents,
orange `#ffa046` for `polynome`.

| Context | Face |
|---|---|
| Personal site and small web tools | `font-family: monospace`, 16px, `line-height: 1.75` |
| Reading pages, apps, maps, Quarto | Palladio stack: `"URW Palladio L", Palladio, P052, Palatino, "Palatino Linotype", "Book Antiqua", Georgia, serif`; Quarto uses `"TeX Gyre Pagella"` first |
| LaTeX notes | EB Garamond (default `gt.cls`) |
| LaTeX course and handout | TeX Gyre Pagella with `Numbers=OldStyle`, and `mathpazo` for maths |
| Charts | `serif` family, matched to the document (`TeX Gyre Pagella` in the handout) |
| Code in specimens | JuliaMono |

- **Old-style figures in prose and lining figures in data**: `font-variant-numeric: oldstyle-nums`
  on the body, `.nombre { lining-nums }`, `tabular-nums` for counters, and
  `\addfontfeatures{Numbers=Lining}` inside TikZ.
- Headings use normal weight, often italic, at a modest size (`h1` 1.75–2.1rem italic). Hierarchy
  comes from spacing, not from bold.
- Fonts are chosen by tournament (`arpenteur/tools/fonts`): blind left/right duels on real French
  and English prose, a formula and code, at reader, book and print size.

---

## 7. Charts and maps (Tufte)

Written as a rule in `eau`, `av`, `climat`, `geographie` and `arpenteur`.

**Remove:** background, panel border, grid lines, box, legend when a direct label works, subtitles,
internal acronyms in titles, decoration.

**Keep:** data, axis, a short title. A caveat block next to a figure is content and stays.

Reference theme (`geographie/notebooks/theme_tufte.R`, with a plotnine twin in `theme_tufte.py`):

```r
rubrique <- "#9E1B1B"
gris <- "#4D4D4D"

theme_tufte_fr <- function(taille = 10) {
  theme_minimal(base_size = taille, base_family = "serif") +
  theme(
    panel.grid = element_blank(),
    axis.ticks = element_line(linewidth = 0.2, colour = gris),
    axis.ticks.length = unit(2, "pt"),
    axis.text = element_text(colour = gris),
    axis.title = element_text(colour = gris, size = rel(0.9)),
    plot.title = element_text(size = rel(1), face = "plain"),
    plot.title.position = "plot",
    plot.caption = element_text(size = rel(0.7), colour = gris, hjust = 0),
    strip.text = element_text(hjust = 0, colour = gris)
    )
}
```

Also `theme_carte_fr` (`theme_void`, legend at the bottom with a thin key), `degrade_rubrique()` and
`degrade_divergent()`.

Mark and layout conventions:

- Thin strokes: 0.2–0.5 pt lines, ticks of 2 pt, points of about 1.4–1.6.
- Context in light grey, the subject in the accent colour: individuals grey and means red; the whole
  story grey and the current passage red.
- Distributions as median, a point with its value, and the quartiles as a line, with **no value
  axis** (`eau` `figure_bloc`).
- Strip labels flush left. Labels wrapped with `strwrap`. Decimal comma.
- **Level axes start at zero** with `expand_limits(y = 0)`, never `limits = c(0, NA)`, which silently
  drops points.
- Quantile axes are ordered numerically (`facteur_x()`), with `drop = FALSE` so that fully masked
  categories keep their place.
- CI ribbons use `show.legend = FALSE`.
- Size figures for the page column: `LARGEUR <- 6.3` in for `eau`, 4.6 × 2.6 in for the handout,
  centimetres in `save_srpp`. Output is `cairo_pdf` for LaTeX and `png` at 120 dpi for Quarto. A
  check script verifies the dimensions (`av` `verifier_sorties.r`).
- Order categories by value or by clustering, not alphabetically.

Maps:

- Draw the full set of units (all departments, responding or not), then the data on top.
- Maps in a book are SVG produced by the tool itself: no tiles, no network, and **the same scale
  throughout**.
- Web basemaps are desaturated (`saturation = -0.8`, `opacity = 0.7`). Natural Earth sits underneath
  as a fallback. Historical layers fade in by date.
- LISA cluster maps were tried and dropped (an isolated significant unit reads as a cluster). Prefer
  global Moran's I plus raw maps.

---

## 8. Web UI and UX

### 8.1 Layout and chrome

- **No panels, buttons or controls** where reading suffices: "scroll, or `j` and `k`; click a place
  to zoom on it; every paragraph has its address (`#12`)" (`arpenteur`).
- One text column of about 35–45 em. A map sits beside it, `position: sticky`, and follows the
  paragraph in view.
- Tufte page: text at 60 % width, margin figures floated right into the gutter, folio numbers in
  the left margin, indexes and chronology at the end.
- Grid layouts use `minmax(0, 1fr)` columns and `min-width: 0` children.
- Collapse to one column at about 760–900 px. On a phone the map becomes a sticky band at 42vh.
- Pages are printable: `@page { size: A4 }` and a `@media print` block.
- Reset with `* { margin: 0; padding: 0; box-sizing: border-box; }`. Tokens are CSS custom
  properties (`--page`, `--ink`, `--quiet`, `--place`, `--serif`).

### 8.2 Controls

- A button is styled as text: `background: none; border: 0`, underlined, and turns the accent colour
  on hover.
- An input is a bottom border only. A `<select>` is set inside a sentence ("phrase" UI), with an
  accent underline and a ▾ glyph.
- Legends are inline swatches of 9 px squares set in the text.
- Hover states change colour or outline the shape (`path.survole { stroke-width: 1.6 }`), never a
  shadow.
- Personal-site marks: `›` list bullets, `⟡` after links, `[+]` disclosure on `<details>`.
- **Progressive disclosure with native `<details>`** in place of tabs, filters or modals. In
  `cinema`, a day opens onto its films, and a film opens onto its synopsis and showtimes, each film
  summary showing its showtimes before it is opened. On the personal site, the job line opens onto
  the list of reports. No JavaScript is needed: if the "only one open at a time" behaviour is
  wanted, write it in plain JS, without a library.

### 8.3 Interaction

- **Keyboard first** for repetitive work. In `eau/codage` one letter key picks a box, using the
  AZERTY rows `A Z E R T Y U I O P` / `Q S D F G H J K L M`. `Enter` saves and moves to the next
  item, and `←` and `→` move without saving. The font tournament uses the arrow keys for its choices
  and `z` to undo.
- One item per screen when coding. The item is shown **inside its full context**, which is greyed
  out while the item itself is in full ink.
- A model suggestion is preselected and drawn with a dashed border. `Enter` accepts it and any key
  replaces it. The stored status records the difference (`validé` vs `écrit`).
- Review queues are ordered by what needs a human: uncoded items first, then partly coded, then
  "other", then rare categories, then the rest. The order is fixed when the tool opens.
- The current paragraph is in full ink and the others are quiet. A 0.4 s colour transition is the
  only animation.
- The map camera keeps its zoom when the next places fit, eases for short moves and flies for long
  ones. Tiles for the next passages are prefetched where the tile policy allows it.
- Reloading the page resumes from what is on disk. State never lives only in memory.
- Local tools bind to `127.0.0.1` and open the browser themselves.

### 8.4 Implementation

- Static output: one HTML file with inline CSS and JS, or a template filled by Python
  (`build_page.py`, `generate_webpage.py`, `arpenteur publish`). Deployed with GitHub Pages and
  Actions.
- Libraries: MapLibre (Leaflet in older work), KaTeX, OpenSeadragon. Load from a CDN with an exact
  pinned version.
- Shiny apps hold one `STYLE` string with the same tokens and the Palladio stack.

---

## 9. LaTeX

### 9.1 The class `gt.cls`

Shared by every note (`eau`, `av` ×3, `af`, `geographie`). "Tufte-style historical document class
with French rubrication."

- `article` at 10pt. A4 with left 25 mm, **right 70 mm**, top 25 mm, bottom 35 mm, `marginparwidth`
  48 mm and `marginparsep` 12 mm.
- `babel[french]`, `csquotes`, `fontspec`.
- `\linespread{1.3}`, `\parindent 0pt`, `\parskip \baselineskip`.
- `titlesec`: `\section` set `\Large\bfseries` and `\subsection` set `\large\bfseries`.
- `\maketitle`: ragged right, the title at section size, then the author and date in `\small`.
- The abstract is left-aligned, with a bold label and normal body.
- `\rubric{}` sets a bold word in the rubric colour.
- `\sidenote{}` is a numbered `\marginpar`, `\footnotesize`, ragged right.
- The canonical version (2026/09/28, in `geographie/handout`) defines
  `\definecolor{rubrique}{HTML}{9E1B1B}` and uses it in the title and in `\rubric`. Its font is
  Pagella with OldStyle figures, `mathpazo` and `fontspec[no-math]`.

### 9.2 Note conventions

- Compile with **XeLaTeX** (LuaLaTeX also works), twice, from the `note/` folder.
- Standard packages: `booktabs`, `microtype`, `float` (`[H]`), `caption` (small, italic, ragged
  right), `array`, `ragged2e`, `longtable`, `siunitx` (`locale=FR`), and `hyperref` (`hidelinks`, or
  links coloured black). Course material uses `biblatex` (authoryear) with `biber`.
- `\graphicspath{{../output/figures/}}`. Figures and tables are **inputs, never edited by hand**:
  `\input{tab_*.tex}`, plus macros for numbers that update themselves (`controle.tex`) and a macro
  for each quotation (`\citetheme{key}`).
- `\InputIfFileExists{…}{}{}` with a visible placeholder ("Graphique à générer : relancer
  `graphiques.r`") so the note always compiles.
- Full width: `\fullwidth = \textwidth + \marginparsep + \marginparwidth`, with `\figpleine` and
  `\tabpleine` for content that does not fit the column. `\figopt` is the default column width.
- French typography: `\textsc{ddt(m)}` (small caps on lowercase acronyms), `\og{}…\fg{}` or « »
  through csquotes, decimal comma, `~;~` as a separator, and `\emergencystretch=2em`.
- The red `\retenu{}` and `\fort{}` highlight the part of a quotation coded under a theme. A
  quotation reproduces the whole answer.
- `\citer{source}{text}` sets a small quote with its attribution flush right.
- Notes cite in sidenotes (Tufte), not footnotes.
- `\asterism` (⁂ in Symbola) separates sections in essays.

### 9.3 TikZ diagrams

The reference is `geographie/handout/schemas/`: 33 diagrams, every one built from the same
`styles.tex`. The grammar is written down in commit `97a63fb` of `geographie`:

> « Une seule brique de base, la grille de cases blanches au trait noir de 0,5 pt ; gris pour ce qui
> entre, rouge pour ce qui sort ou ce qu'on regarde ; opérations dans des boîtes blanches à bord
> noir sur un flux vertical de X vers Y ; annotations en pointillés au lieu de légendes. Plus de
> violet ni de bleu. »

It says: one basic building block, the grid of white cells with a 0.5 pt black line; grey for what
goes in, red for what comes out or what we look at; operations in white boxes with a black border,
on a vertical flow from X to Y; dotted annotations instead of legends; no more violet or blue.

The inspiration is the diagrams of *The Little Book of Deep Learning* ("lbdl").

#### 9.3.1 The preamble (`schemas/styles.tex`, loaded once by `cours.tex`)

```latex
\usepackage{tikz}
\usetikzlibrary{arrows.meta, calc, decorations.pathreplacing}
\colorlet{rouge}{rubrique}
\colorlet{entree}{black!28}
\newlength{\cote}
\setlength{\cote}{0.28cm}
\tikzset{
  every picture/.style = {x=\cote, y=\cote, line width=0.5pt, execute at begin picture={\addfontfeatures{Numbers=Lining}}},
  fleche/.style = {-{Stealth[length=3.2pt, width=2.6pt]}, line width=0.5pt},
  operation/.style = {draw=black, fill=white, line width=0.5pt, inner xsep=3pt, inner ysep=1.5pt, minimum height=1.5\cote, font=\scriptsize},
  annotation/.style = {font=\scriptsize\itshape},
  repere/.style = {dotted, line width=0.6pt},
  valeur/.style = {font=\tiny},
  noeud/.style = {draw=black, fill=white, circle, inner sep=1pt, minimum size=1.6\cote, font=\scriptsize},
}
\newcommand{\grille}[2]{%
  \foreach \gx in {0,...,#1} {\draw (\gx, 0) -- (\gx, #2);}%
  \foreach \gy in {0,...,#2} {\draw (0, \gy) -- (#1, \gy);}%
}
\newcommand{\case}[3]{\fill[#3] (#1, #2) rectangle ++(1, 1);}
```

`rubrique` comes from `gt.cls` (`#9E1B1B`). `\addfontfeatures` needs fontspec, so XeLaTeX or
LuaLaTeX.

#### 9.3.2 Vocabulary

| Element | How it is drawn | Meaning |
|---|---|---|
| unit | `\cote` = 0.28 cm on both axes, so one cell is 1 × 1 | everything is drawn on integer cell coordinates |
| cell grid | `\grille{w}{h}`, 0.5 pt black, white cells | a table, raster, panel or vector of values |
| input | `\case{i}{j}{entree}` or `\fill[entree]`, 28 % black | what goes into the operation, or the context |
| output / focus | `\case{i}{j}{rouge}` or `\fill[rouge]` | the result, or what the reader should look at |
| value | `node[valeur]` (`\tiny`), white text on a red cell | the actual numbers of the worked example |
| operation | `node[operation]{…}`, white box with black border, `\scriptsize`, a formula or a word | the transformation |
| flow | `fleche`, Stealth tip 3.2 × 2.6 pt, 0.5 pt | the direction of computation |
| annotation | `annotation`, `\scriptsize\itshape` | names, panel captions, axis names |
| leader | `repere`, dotted 0.6 pt, ending in `node[right, annotation]` | replaces the legend; also reference lines and thresholds |
| graph node | `noeud`, white circle with black border | units of a network or graph |
| emphasis | `line width=1.4pt` outline | zones, windows, the cell being discussed |
| row label | `node[left, font=\small]{$X$}` at `x = -0.4` | names the layer: `$X$`, `$Y$`, `$S$`, `$R$`, `$A$` |

Only three inks are allowed: black, `entree` grey and `rouge`, plus white for text on red. There are
no other colours, no gradients, no shadows, no rounded corners and no legend.

#### 9.3.3 Composition rules (practice in all 33 files)

- **The flow goes upwards.** Input X sits at the bottom (y = 0), the operation box is centred above
  it, and output Y is at the top (y ≈ 8–12). Arrows stop about 0.2 units short of the cells:
  `\draw[fleche] (2, 4.2) -- (o.south); \draw[fleche] (o.north) -- (2, 7.8);`.
- **Name rows on the left** in `\small` maths, and **put explanations on the right** as dotted
  leaders that start just past the figure (`x = w + 0.3…0.4`) and run to a common column where the
  text sits: `\draw[repere] (w+0.4, y) -- (w+2, y) node[right, annotation] {…};`.
- **Panels side by side.** Use one `\foreach \k/\nom in {0/{…}, 1/{…}}` with
  `\begin{scope}[shift={(\k*12, 0)}]`, choose what changes inside with `\ifcase\k … \or … \fi`, and
  caption each panel with `node[annotation, below] at (centre, -0.3) {\nom}`. Comparisons
  (quantiles vs equal intervals, random folds vs block folds, fixed radius vs nearest neighbours)
  are always drawn as panels side by side.
- **Numbers are the ones in the text**: the worked example, computed exactly, as in "impacts
  recalculés : diagonale 1,19, total 2". French formatting applies: a decimal comma (`0{,}5` in
  maths, `5,25` in text) and thin spaces (`9\,600`, `21\,°C`, `300\,m`).
- **Scale by changing the unit, not the styles.** Use `[x=1.6\cote, y=1.6\cote]` for wider cells,
  `[x=1cm, y=1cm]` for causal graphs and nested-model diagrams, and `[scale=0.5]` for wide scenes.
  Line widths and fonts stay fixed, so every diagram has the same weight on the page.
- **Data, not drawing.** Loops over lists of values (`\foreach \x [count=\c from 0] in {…}`), cells
  computed with `\pgfmathsetmacro`/`\ifdim`, curves drawn with `plot` over a `domain`. Random layouts
  use a fixed seed (`\pgfmathsetseed{7}`). Point clouds that are too irregular to compute are written
  out as coordinates, generated once and pasted in (`noyaux.tex`).
- **Causal graphs** (`canaux`, `instrument`, `berkson`): bare math nodes, black arrows, and the path
  under discussion in `rouge`. A forbidden or assumed-zero path is drawn `dashed`, a correlation is
  drawn `repere`, and curved edges use `to[bend …]`.
- **Braces** (`decorate, decoration={brace, amplitude=3pt}`) mark extents such as segments, widths
  and heights. Arcs mark angles.

#### 9.3.4 Files and use in the document

- One diagram per file in `schemas/name.tex`, containing only `\begin{tikzpicture}…\end{tikzpicture}`:
  no preamble, no `\documentclass`, no comments.
- It is included with
  `\newcommand{\schema}[1]{\adjustbox{max width=\textwidth}{\input{schemas/#1}}}`, inside a float:

```latex
\begin{figure}[htbp]
\centering
\schema{maup}
\caption{Effet de zonage. Les mêmes seize valeurs, moyennées sur deux zones découpées de deux façons, donnent des écarts entre zones de 2,25 et de 3,75.}
\label{fig:maup}
\end{figure}
```

- **The caption carries the explanation**: a short title, then full sentences that walk through the
  numbers and name the colours ("en rouge", "en gris"). Every diagram is cited in the text with
  `figure~\ref{fig:…}`.
- Test a single diagram with `schemas/test.tex`, which is `\documentclass{../gt}`, `\input{styles}`
  and `\input{\schema}`, compiled with `xelatex "\def\schema{maup}\input{test}"`.
- Diagrams explain a mechanism; charts (ggplot, §7) show data. The two share the grey and rubric
  palette, so they read as one system.

#### 9.3.5 Minimal template

```latex
\begin{tikzpicture}
  \foreach \k/\nom in {0/{avant}, 1/{après}} {
    \begin{scope}[shift={(\k*10, 0)}]
      \case{1}{1}{entree}
      \grille{4}{4}
      \node[operation] (o) at (2, 6.5) {$f(x)$};
      \draw[fleche] (2, 4.2) -- (o.south);
      \draw[fleche] (o.north) -- (2, 8.8);
      \begin{scope}[shift={(0, 9)}]
        \case{2}{2}{rouge}
        \node[valeur, white] at (2.5, 2.5) {1,2};
        \grille{4}{4}
      \end{scope}
      \node[annotation, below] at (2, -0.3) {\nom};
    \end{scope}
  }
  \node[left, font=\small] at (-0.4, 2) {$X$};
  \node[left, font=\small] at (-0.4, 11) {$Y$};
  \draw[repere] (14.4, 11.5) -- (16, 11.5) node[right, annotation] {ce qu'on regarde};
\end{tikzpicture}
```

#### 9.3.6 TikZ generated from data (`af`)

`af/current/reformes/code/tikz_treillis.r` writes the TikZ for a lattice diagram (family quotient
× family allowances × minimum incomes) **from a CSV of results**. It formats the numbers with a
decimal comma and `\,` thousands separators, and returns `NULL` (no figure) when a value is missing.
`genere_note.r` then inserts it into the note. Generating the code from the results is the right
pattern for a diagram whose numbers change with each run. The diagram's style, however, predates the
grammar: it uses `positioning`, a blue `bleutreillis` (RGB 0,90,160), black dots, styles defined
inline, and a standalone version in `output/tikz/`. See §14.

### 9.4 Why not Quarto for the handout

Quarto's PDF margin notes conflict with the class's sidenotes. Use plain LaTeX for prose documents
and Quarto for executable material.

---

## 10. Quarto

From `geographie/notebooks`.

- `engine: knitr` with `reticulate`. R and Python run in **one document**, side by side in
  `::: {.deux-langages}`, a two-column grid that becomes one column below 900 px. CSS labels each
  block "R" or "Python" in small uppercase letter-spaced rubric red.
- Each cell has `#| label:`, prefixed `r-` or `py-`. The Python environment is pinned
  (`RETICULATE_PYTHON`).
- **Profiles** generate the student and teacher versions from one source: `_quarto-eleve.yml` and
  `_quarto-enseignant.yml` set `output-dir` and `title-prefix`, and solutions sit in
  `::: {.content-visible when-profile="enseignant"}`. Render with
  `quarto render --profile eleve|enseignant`.
- Settings: `lang: fr`, `echo: true`, `warning: false`, `message: false`, `dev: png`, `dpi: 120`,
  `fig-width: 5`, `fig-height: 4`, `toc-depth: 2`, `number-sections: true`, `code-copy: true`, grid
  `body-width: 1150px`, `margin-width: 250px`.
- `styles.css`: Pagella body at 1.02rem, colour `#1a1a1a`, headings weight 600 in rubric red. Block
  classes: `.exercice` (3 px rubric left rule), `.correction` (grey rule), `.demo` (dotted rubric
  rule).
- Pedagogy:
  - Each module opens with its plan, prerequisites and references, and ends with a recap pointing
    to the handout chapter.
  - Include at least one "Exemple pas à pas" whose answer is known by construction.
  - Add a simulation that shows where the method works when the real data show nothing.
  - No cell runs longer than about 30 s; heavy results are precomputed in `processed/`.
  - A single-language cell gets one line of prose saying why (code comments are banned).
- Publish analysis sites through `tarchetypes::tar_quarto()` (`climat`).

---

## 11. Data, confidentiality and secure enclaves

- `data/` and `output/` are never versioned. Exports containing personal data (an `email` column)
  stay in `data/`. Opinions expressed by administrations are not personal data and may be named.
- Secure enclave (CASD): code must run there **offline**. Values computed outside the enclave travel
  with the scripts as versioned CSV files (`cibles_dwa.csv`), with a column dating the source. The
  test suite runs outside the enclave on synthetic data.
- Secrecy thresholds are implemented as functions, and each table states its counting unit
  (persons, not rows). Publishing a minimum or maximum is forbidden. Extreme percentiles of small
  cells are treated as individual values.
- Administrative units are resolved against the official reference (`geo.api.gouv.fr`, COG
  vintages) and tolerate typos in the source. Metropolitan France and the overseas departments
  (DROM) are analysed separately.

---

## 12. Git and collaboration

- Commit messages are descriptive and in the repository's language: French in `eau`, `av`, `af` and
  `geographie`, often prefixed by the component (`Note :`, `Ficovie :`, `reformes :`), and English
  in `alpes`, `arpenteur` and `climat`. A title is often a claim: "A face reads combed or it reads
  scratched, and that is two numbers".
- Work happens on branches merged through PRs. Generated outputs are not tracked: `alpes` removed 108 force-added
  optimizer PNGs (68 MB) and made its `.gitignore` exceptions explicit.
- Model identifiers never appear in pushed artefacts (`eau` rule).
- Only a few reference outputs are versioned, as documentation; everything else can be regenerated.
  `.gitignore` exceptions are explicit.

---

## 13. Checklist for a new repository

1. `README.md` as the only documentation: purpose, Run block, layout table, firm rules, what
   remains, tests.
2. `CLAUDE.md`: copy §17.
3. A root `run.sh` with no options that rebuilds every output from a clean checkout.
4. Python: `pyproject.toml` with the ruff configuration from §4.1 and no formatter. R: `renv` and
   `.r` files.
5. `data/` and `output/` git-ignored. `data/reference/` versioned.
6. Copy `theme_tufte.R` / `.py` and `gt.cls` (the 2026/09/28 version).
7. Web: the CSS tokens from §6 and the Palladio or monospace stack. No comments in CSS or JS.
8. Every human decision lives in a replayable file. Generated files are never edited.

---

## 14. Drift found, and a recommendation for each

| # | Divergence | Evidence | Recommendation |
|---|---|---|---|
| 1 | **Comments.** The "no comments" rule is broken in `alpes` (about 3,000 comment lines and 680 docstring lines, with an explicit policy of French comments and English prose), `af` (about 700 R comment lines), `24ri`, `embeddings`, `thalweg`/`causeries` HTML and CSS, and the preamble of `av/ehvp/note/ehvp.tex` | counts by grep | Decide whether `alpes` is a deliberate exception: its README documents the opposite policy. Clean up `af`, which is current work. The others are old or small |
| 2 | **`%>%`** is used about 900 times in `af/current` | `af/current/**` | Migrate to `\|>` when those files are next touched |
| 3 | **Python indentation**: 4 spaces in `eau` and `arpenteur`, 2 spaces in `geographie` (the PLAN says "two-space indent… exactly as in the author's example") | `geographie/data/prepare/*.py` | Pick one. 4 spaces is what ruff's defaults and most Python readers expect, and 2 spaces matches R. I would keep 4 for Python and 2 for R |
| 4 | **R extension**: `.r` is a rule in `av`, but `eau` and `geographie` use `.R` | file listings | Use `.r` everywhere, as the written rule says |
| 5 | **Three rubric reds**: `#9E1B1B`, `#8B1A1A`, `#8f0e21` | gt.cls, eau note, arpenteur/24ri | Make `#9E1B1B` canonical |
| 6 | **Three versions of `gt.cls`**: the 2023 copy (in 5 repos) hard-codes black despite announcing rubrication, and the 2026 copy restores it | md5 differs | Keep one source (a small `gt` repo or a submodule). Make the font an option (`garamond`/`palladio`) and keep a `rubrication` switch |
| 7 | **Accent colour per project**: teal `#03565E` (eau), red (geographie, arpenteur), green `#286c44` (personal web), Cour `srpp` palette (af, av) | §6.1 | This is acceptable. State the principle as "one accent per document, chosen once" |
| 8 | **Type hints** are used in `eau` and absent in `arpenteur` | — | Not covered by any rule. Decide one way |
| 9 | **Shared theme drifts**: `eau/figures.R` redefines a theme inline (black text, legend on top) instead of sourcing `theme_tufte.R`, and `af/charte.r` keeps minor grid lines in its fallback theme | — | Ship one theme file and source it |
| 10 | **HTML comments in `.qmd`** (`<!-- À REVOIR … -->`) | `geographie/notebooks/05_*.qmd` | Move TODOs to the PLAN or README |
| 11 | **Background**: `#fcfcfc` in most pages, but `#faf9f5` in 24ri and `#fff` in eau | — | Use `#fcfcfc` |
| 12 | **TikZ in `af`** does not follow the diagram grammar: it uses a blue `bleutreillis`, styles defined inline, and `positioning` instead of `styles.tex` | `af/current/reformes/code/tikz_treillis.r` | Keep the R generator, but have it emit `operation`/`fleche`/`repere`/`rouge` and `\input` the shared `styles.tex` |
| 13 | **Bug**: `font=\scripointsize` in `af/current/reformes/note/note_af.tex:279` (the generator has `\scriptsize`), so compilation fails on an undefined control sequence | grep | Regenerate the note with `genere_note.r`, or fix that line |
| 14 | **Real violations used as *don't* examples in §15**: a nested comprehension in `eau/decompter.py`, aligned brackets in `eau/figures.R`, a trailing `+` in `av/comparaison_dwa.r`, CSS comments in `thalweg` | §15 | Fix them the next time those files are touched |
| 15 | **No single reproduction script** (`eau`, `av`, `af`, `geographie`, `arpenteur`), or one with options (`geographie/data/prepare/tout_preparer.py`). Only `climat/run.sh` meets the rule | §4.3 | Add a root `run.sh` with no options to each repository; where part of the data comes from the CASD, it builds what it can and says what is missing |
| 16 | **`cinema`**: bare `except:` blocks that return `None` and swallow every error; nested comprehensions; `\` line continuations; dead code (`to_html`, `pypandoc`); `$('details')` called without loading jQuery, so that script does nothing; Allociné scraped with nothing on whether its terms of use allow it | `cinema/programs/build_page.py`, `header.html` | Let errors surface (the CI job then fails visibly); write the plain-JS accordion; check the terms of use or use an open source |
| 17 | **`cours`** publishes renders without sources, uses English and Python only, Quarto and matplotlib defaults (frame, title, boxed legend, four-colour LISA map), and `# TODO` comments | `cours/*.html` | This is the course `geographie` replaced (`PLAN.md` §4). Archive it, or point its README to `geographie` |
| 18 | **`cassettes`** downloads a YouTube playlist of commercial audiobooks (« Bibliothèque de l'aventure ») with `youtube-dl` in CI and republishes the MP3 files on a public GitHub Pages site | `cassettes/programs/download.sh`, `.github/workflows/main.yml` | Almost certainly infringes the rights holders' copyright and YouTube's terms. Make the repository private or stop publishing the audio. This is a legal risk, not a style issue |
| 19 | **`lexflation`**: additions in red `#cf222e` and deletions in green `#2ea043` (the GitHub diff palette): red/green cannot be told apart by about 8 % of men, and red already means "the thing we look at" here; docstrings and French comments throughout; data commits by a bot | `lexflation/scripts/generate_chart.py` | Use one accent and grey (growth in rubric red, shrinkage in grey), or red and blue; comments as §4 decides |
| 20 | **`polynome`**: `argparse` with options and comments in `build/`, a third visual identity | `polynome/build/build.py` | Options are acceptable for a build tool with users; the identity is justified (§6.2). Its accessibility (`:focus-visible`, `aria-pressed`) should be copied into `eau` and `arpenteur` |
| 21 | **Delivered prose written by a model.** Many commits co-authored by Claude rewrite delivered text (`geographie`: « Notebooks : prose reprise » ; `eau`: « Note : discussion rétablie »), which §3.4 now forbids | `git log` | From now on a model prepares skeletons only; check which passages of the current notes and handout were drafted by a model, and rewrite them |

---

## 15. Don't / do

Each pair is labelled with where it comes from:

- **real**: copied from a repository; a real *don't* is a place where the code breaks its own rules.
- **adapted**: real code, shortened.
- **illustrative**: written for this document.

The *do* side is always the house style. Translations of the French snippets are given in
parentheses where they help.

### 15.1 Nothing clever: nested comprehensions

**Don't** (real: `eau/code/analyses/decompter.py`, `completer_enjeux`). This breaks the
"no nested comprehensions" rule.

```python
enjeu_de = {angle: enjeu for grille in GRILLES.values()
            for enjeu, angles in grille.items() for angle in angles}
```

**Do** (illustrative):

```python
enjeu_de = {}
for grille in GRILLES.values():
    for enjeu, angles in grille.items():
        for angle in angles:
            enjeu_de[angle] = enjeu
```

### 15.2 Bracket layout

**Don't** (real: `eau/code/analyses/figures.R`). The arguments are aligned under the opening
bracket, and the bracket closes on the last argument.

```r
theme(panel.grid = element_blank(),
      legend.position = "top",
      legend.title = element_blank())
```

**Do**:

```r
theme(
  panel.grid = element_blank(),
  legend.position = "top",
  legend.title = element_blank()
  )
```

The same applies to Python imports and calls: one element per line, and the closing bracket
indented at the level of the content.

### 15.3 R pipes and ggplot

**Don't** (real: `af/current/reformes/code/utils_af.r` and `av/ehvp/scripts/comparaison_dwa.r`).
These use `%>%` on one line and a trailing `+`.

```r
compose_dispositifs_reconciliation <- function(men)
  men %>% compose_autres_prelevements_sociaux() %>% compose_autres_minima()

palette_d() + scale_x_continuous(breaks = VAGUES) + expand_limits(y = 0) +
```

**Do**:

```r
compose_dispositifs_reconciliation <- function(menages) {
  (
    menages
    |> compose_autres_prelevements_sociaux()
    |> compose_autres_minima()
    )
}

figure <- (
  ggplot(series, aes(vague, montant, colour = source))
  + geom_line(linewidth = 0.4)
  + scale_x_continuous(breaks = VAGUES)
  + expand_limits(y = 0)
  )
```

### 15.4 A comment means the name is wrong

**Don't** (real: `af/current/outils_ines/code/charte.r`). ("Label pushed beyond the bar, on the
sign's side.")

```r
# étiquette décalée au-delà de la barre, du côté du signe
decale_v <- function(x) ifelse(!is.na(x) & x < 0, 1.5, -0.7)
```

**Do** (illustrative): the comment becomes the name.

```r
decalage_etiquette_hors_barre <- function(valeurs) ifelse(!is.na(valeurs) & valeurs < 0, 1.5, -0.7)
```

### 15.5 Full words in names

**Don't** (illustrative):

```r
pwm <- function(d, w) sum(d$v * d[[w]]) / sum(d[[w]])
cc <- unique(m$cc)
```

**Do** (adapted from `climat`, where the rule is written):

```r
population_weighted_mean <- function(measurements, weight) {
  sum(measurements$value * measurements[[weight]]) / sum(measurements[[weight]])
}
commune_codes <- unique(measurements$commune_code)
```

A single letter is acceptable only inside a line or two, with its conventional meaning: `x`/`y` in
a model formula, `k` and `n` in a proportion, `i` in a three-line loop.

### 15.6 Declarations are data, never behaviour

**Don't** (illustrative). The configuration holds a function name, which code then looks up.

```yaml
clay_hazard:
  type: intensive
  transfer: transfer_intensive_weighted
```

```r
moved <- do.call(get(variable$transfer), list(joined))
```

**Do** (real: `climat/R/join/transfer.r`). The declaration names a type, and the dispatch is an
explicit `switch()` that a reader can find with `grep`.

```r
moved <- switch(
  variable$type,
  extensive = transfer_extensive(joined),
  intensive = transfer_intensive(joined),
  extremal = transfer_extremal(joined, variable$extreme),
  categorical = transfer_categorical(joined, variable$aggregation)
  )
```

### 15.7 Fail loudly and say what to do

**Don't** (illustrative):

```python
try:
    extraits = pd.read_csv(EXTRAITS)
except Exception:
    extraits = pd.DataFrame()
```

**Do** (real: `eau/code/analyses/figures.R` and `climat/R/transform/clean_gaspar_catnat.r`). The
message names the missing file and the command that produces it.

```r
if (!file.exists(chemin))
  stop(chemin, " est absent : lancer d'abord python code/analyses/figures.py.",
       call. = FALSE)

stop(sprintf("%s holds no single file matching %s", archive, pattern), call. = FALSE)
```

### 15.8 Analysis scripts: one argument, no options

**Don't** (illustrative):

```python
parser = argparse.ArgumentParser()
parser.add_argument("--export")
parser.add_argument("--seuil", type=float, default=0.95)
parser.add_argument("--sans-drom", action="store_true")
```

**Do** (real: `eau/code/analyses/carte.py`). Thresholds are constants in `config.py`, and an
outre-mer analysis is its own script. ("usage" line; "is missing".)

```python
def main() -> int:
    if len(sys.argv) != 2:
        print("usage : python code/analyses/carte.py data/<sondage>.csv")
        return 2
    for fichier in (EXTRAITS, REPONSES):
        if not fichier.exists():
            print(f"{fichier} est absent.")
            return 1
```

Tools (`arpenteur`, `alpes`) are the exception: they have subcommands, and even then very few flags.

### 15.9 Writing a note

| Don't (illustrative) | Do |
|---|---|
| « Le résultat le plus solide de la note est que les DREAL n'utilisent pas les bons leviers. » (A self-referential claim, plus a judgement.) | « Les directions régionales (\textsc{dreal}, \textsc{draaf}) utilisent le plus le levier qu'elles jugent le moins efficace~: l'animation. » (real: `eau/note/note.tex`) |
| « À la lecture des réponses, les services semblent surtout manquer de moyens. » (Based on reading free text directly.) | « 41 % des départements évoquent le manque de moyens en q117 (bornes 38–47 %). » (illustrative: the number comes from the coded annotations, with bounds for non-response) |
| « L'effet de l'âge est significatif (\*\*\*). » | « Passé 70 ans, les versements baissent de 12 % (intervalle à 95 % : 8 à 16 %). » (illustrative) |
| « le premier décile de niveau de vie » (meaning the group) | « le premier dixième » for the group; « le premier décile » only for the threshold |
| A figure with five bars | A five-row `booktabs` table: below about ten values, a table is denser and more exact |
| `… soit 1 247 répondants.` typed into the `.tex` | `… soit \nombreRepondants{} répondants.`, a macro written by the script into a generated `.tex` (as `controle.tex` does) |

### 15.10 Charts

**Don't** (illustrative):

```r
ggplot(parts, aes(enjeu, part, fill = service)) +
  geom_col(position = "dodge") +
  scale_y_continuous(limits = c(0, NA)) +
  labs(title = "Fig. 3 – Part des DDT(M) évoquant chaque enjeu (q117, export 29/09)",
       subtitle = "Métropole, hors non-répondants")
```

This has the default grey panel and grid, a legend that duplicates the labels, a title with an
acronym and a date, and a subtitle. `limits` also silently drops negative values.

**Do** (adapted from `geographie/handout/figures.R`):

```r
figure <- (
  ggplot(parts, aes(part, reorder(enjeu, part)))
  + geom_point(colour = gris, size = 1.4)
  + geom_point(data = filter(parts, enjeu == enjeu_vise), colour = rubrique, size = 1.6)
  + geom_text(aes(label = virgule(part)), hjust = -0.6, size = 2.4)
  + expand_limits(x = 0)
  + labs(x = NULL, y = NULL, title = "Enjeux évoqués")
  + theme_tufte_fr()
  )
```

It uses the shared theme, values written next to the points, one accent colour for the item being
discussed, categories ordered by value, and an axis that starts at zero. The scope and the date go
in the caption on the note side.

### 15.11 Web controls

**Don't** (illustrative):

```html
<button class="btn btn-primary btn-sm"><i class="icon-check"></i> Valider</button>
```

**Do** (real: `arpenteur/.../review.css`). The button is set as text, takes the accent colour on
hover, and has no box.

```css
button { font: inherit; background: none; border: 0; cursor: pointer;
         text-decoration: underline; text-underline-offset: 2px; }
button:hover { color: var(--place); }
input { border: 0; border-bottom: 1px solid var(--quiet); }
```

Better still is no control at all: scrolling, `j`/`k`, a click on the place itself, and an address
for each paragraph (`#12`).

### 15.12 No comments in CSS either

**Don't** (real: `thalweg/index.html`):

```css
.grid img {
  /* All 30 drawings share this ratio, so the rows stay even … */
  aspect-ratio: 15502 / 10925;
}
```

**Do**: a custom property whose name says what the value is, with the reasoning in the README.

```css
:root { --drawing-ratio: 15502 / 10925; }
.grid img { aspect-ratio: var(--drawing-ratio); }
```

### 15.13 TikZ

**Don't** (adapted from `af/.../tikz_treillis.r`). This uses a blue, styles defined inline, a
legend, and numbers typed in.

```latex
\definecolor{bleutreillis}{RGB}{0,90,160}
\begin{tikzpicture}[rayon/.style = {bleutreillis, semithick}]
  \draw[rayon] (A) -- (B);
  \node at (6, -1) {\textcolor{bleutreillis}{---} interaction};
\end{tikzpicture}
```

**Do** (real: `geographie/handout/schemas/choc.tex`). It uses the shared styles: grey for input,
red for output, a white operation box, an upward arrow, and a dotted leader instead of a legend.
The numbers are the worked example from the text.

```latex
\fill[entree] (4, 0) rectangle ++(2, 2);
\node[operation] (o) at (5, 4.5) {$(I - \rho W)^{-1}$};
\draw[fleche] (5, 2.2) -- (o.south);
\draw[fleche] (o.north) -- (5, 6.8);
\fill[rouge] (4, 7) rectangle ++(2, 2);
\node[valeur, white] at (5, 8) {1,17};
\draw[repere] (10.3, 8) -- (13, 8) node[right, annotation] {somme $= 2 = 1/(1-\rho)$};
```

When the numbers come from a run, keep generating the TikZ from R, as `af` does, but emit these
styles.

### 15.14 Commit messages

| Don't (real) | Do (real) |
|---|---|
| `Update index.html` (×4, `benbel.github.io`) | `Outre-mer : territoires nommés en toutes lettres dans les figures` (`eau`) |
| `.` (`af`) | `Dates: asides, appositions and surlendemain no longer derail the timeline` (`arpenteur`) |
| `ACP commit` (`24ri`) | `Schémas : grammaire unique inspirée de lbdl, noir, gris et rouge` (`geographie`), whose body states the rule |

A commit message names the component, then states what is now true.

### 15.15 One script, no options

**Don't** (real: `geographie/data/prepare/tout_preparer.py`). The entry point has flags, so "the"
result depends on how it was called, and it only covers data preparation.

```python
analyseur.add_argument("--synthetique", action="store_true")
analyseur.add_argument("--sans-telechargement", action="store_true")
```

The other *don't* is having no entry point at all: `eau`'s README lists about twenty commands to
run by hand, in order.

**Do** (adapted from `climat/run.sh`). One file at the root, no arguments, and every stage through
to the final document. A missing manual input is detected and announced, not passed as an option.

```bash
#!/usr/bin/env bash
set -euo pipefail

cd "$(dirname "$0")"

Rscript -e 'renv::restore(prompt = FALSE)'
Rscript -e 'testthat::test_dir(file.path("tests", "testthat"), stop_on_failure = TRUE)'

if [ -n "$(ls -A data/raw/drias_tracc 2> /dev/null)" ]; then
  Rscript -e 'targets::tar_make()'
else
  echo "No DRIAS files in data/raw/drias_tracc: building everything but the projections."
  Rscript -e 'targets::tar_make(names = !starts_with("drias"))'
fi

quarto render notebooks/drought_exposure.qmd
echo "Done: notebooks/drought_exposure.html"
```

For `geographie`, this means `preparer.py` and `preparer_synthetique.py` (or synthetic data as a
fallback that the script announces), called by a root `run.sh` that also renders both Quarto
profiles and compiles the handout.

---

## 16. Reference files: copy these

| For | Open |
|---|---|
| Python analysis script | `eau/code/analyses/carte.py`, `eau/code/analyses/commun.py` |
| Python package and CLI | `arpenteur/src/arpenteur/` (`resolve/dates.py` for plain functions), `arpenteur/pyproject.toml` |
| R pipeline | `climat/_targets.r`, `climat/R/join/transfer.r` |
| R analysis plus figure | `climat/R/analysis/question_drought_density.r` |
| ggplot theme (R and Python) | `geographie/notebooks/theme_tufte.R`, `theme_tufte.py` |
| Figures for a note | `geographie/handout/figures.R`, `eau/code/analyses/figures.R` (§14 lists their drift) |
| LaTeX class | `geographie/handout/gt.cls` (the 2026 version) |
| LaTeX note | `eau/note/note.tex` (preamble macros, generated inputs) |
| LaTeX course | `geographie/handout/cours.tex`, `chapitres/12_panel.tex` |
| TikZ | `geographie/handout/schemas/styles.tex`, `maup.tex`, `choc.tex`, `canaux.tex` |
| Quarto | `geographie/notebooks/_quarto*.yml`, `styles.css`, `05_statistique_spatiale.qmd` |
| Reading page | `arpenteur/src/arpenteur/publish/templates/reader.css`, `book.css` |
| Review or coding tool | `arpenteur/.../review.css`, `eau/code/applications/codage/app.R` |
| Personal site | `benbel.github.io/index.html` |
| README | `eau/README.md` (French), `arpenteur/README.md` (English) |
| Plan / roadmap | `climat/docs/PLAN.md`, `alpes/ROADMAP.md` |
| Static page rebuilt on a schedule | `cinema/.github/workflows/main.yml` (and fix the issues listed in §14 #16) |
| Reproduce everything | `climat/run.sh` |

---

## 17. Drop-in `CLAUDE.md`

```markdown
Read `README.md` first: it is the only documentation (design, firm rules, known pitfalls). For worked don't/do examples and reference files, see `DESIGN_SYSTEM.md` §15–16.

## Who writes
- You may prepare the skeleton of a delivered text (note, handout, course prose, article, caption): plan, headings, list of figures and tables, empty placeholders marked `À RÉDIGER`, citation keys, macros for generated numbers, consistency checks. You never write its sentences, not even as a draft.

## What to build
- Start from the artefact (note, course, page, drawing) and its reader; code that feeds nothing in it is exploration, and a screen script either becomes an output or goes to `old/`.
- Take the lowest rung that works: script, then output file, notebook, package, local tool, static page. An interface only when a human judgement is the bottleneck; never a dashboard.
- Split applications by mode of attention or by audience, never by feature; one source of truth behind every face; never two tools writing the same file (one tool with two modes, or a lock that refuses to open).
- A tool does one job from end to end, keeps its state on disk, shows the item in context, suggests without applying, orders the work by what needs a human, runs locally, and never edits the source or exports.
- A model suggests or flags but never produces a published number; automate only when volume beats human reading, and validate against a human before stacking corrections.
- Generalise after a concrete case; vertical slices; each change has a measurable gate; judge taste by blind duels, one question at a time.

## Code
- No comments anywhere: no `#`, docstrings, roxygen, or comments in HTML/CSS/JS. Explanation goes in the README, the note or the notebook prose.
- Plain code: no metaprogramming, decorators beyond `@dataclass`, walrus, nested comprehensions, monkeypatching, `assign()`/`do.call()` tricks or NSE beyond ordinary dplyr.
- Right abstraction level: small named functions, named intermediate values, explicit data flow, top-to-bottom reading, early returns. Full words in names.
- Declarations are data, never behaviour: configs never hold code or function names; dispatch with an explicit switch.
- Never guess a column name; take it from the reference file and declare it once.
- Multi-line expressions: open a bracket, one element per line, closing bracket indented at the content level. R: `|>` and ggplot `+` at the start of the line. Do not run black, ruff format or styler; lint with `ruff check`.
- One root script (`run.sh`), with no arguments and no options, rebuilds everything from a clean checkout (environment, tests, data, analyses, figures, note or site); a missing manual input is detected and announced, not passed as an option.
- Version sources, never renders: HTML, PDF and figures are built by the root script or in CI. Pages that change on their own are rebuilt by a scheduled CI job, with no server.
- Analysis scripts: at most one argument, no options; they read the latest export in `data/`. A missing input stops the run and names the command to run.
- `data/` and `output/` are never versioned; generated files are never edited by hand; human decisions live in replayable files.
- Self-hosted models only; never a commercial LLM on the data. Never mention a model identifier in pushed files.

## Figures
- Tufte: no background, grid, box, decoration, subtitle, or legend where a direct label works. Thin strokes, grey context, one accent colour (rubric `#9E1B1B` unless the project says otherwise).
- Level axes at zero with `expand_limits(y = 0)`. Intervals, not stars. Fewer than about ten values: use a table.
- Use the shared `theme_tufte_fr()` / plotnine twin. Decimal comma in French work.
- TikZ diagrams: the `schemas/styles.tex` grammar only (grid cells, grey `entree` in, `rouge` out or focus, white `operation` boxes, upward `fleche` flow, dotted `repere` leaders instead of legends). One file per diagram, real numbers from the text, the explanation in the caption.

## Documents
- LaTeX with `gt.cls` (Tufte margin, sidenotes, rubrication), compiled with XeLaTeX. Tables and numbers are generated `.tex` inputs and macros, never typed by hand.
- No claim without a reproducible measure; no comment on the note itself; no interpretation beyond the numbers.

## Web
- Tokens: page `#fcfcfc`, ink `#111111`, quiet `#6e6e6e`, one accent. Palladio stack for reading, monospace for the personal site.
- Disclose progressively with native `<details>` rather than tabs, filters or modals.
- No chrome: text-styled buttons, bottom-border inputs, keyboard first, state on disk, one column below about 820 px, printable.
```


---

# Part B. Working log

The conversation took place in French between the author and Claude, from 9 to 10 October 2026.
Each entry gives the question, what was found, what was proposed, and a status:

- **decided**: the author settled it;
- **integrated**: written into Part A;
- **proposed**: suggested by Claude, not yet accepted;
- **open**: needs a decision (listed again in Part C).

## B1. Extraction (integrated)

Read 14 repositories (`eau`, `av`, `af`, `geographie`, `climat`, `arpenteur`, `alpes`, `24ri`,
`thalweg`, `embeddings`, `causeries`, `cinema`, `meteo`, `benbel.github.io`). The sources were the
READMEs, `CLAUDE.md` files, plans, roadmaps, code, `gt.cls`, the chart themes, CSS and Quarto.
Result: sections 1 and 3 to 17 of Part A. Nothing was committed to the repositories: putting the
same file into 14 repositories would make it drift at once.

## B2. TikZ (integrated, §9.3)

The diagram grammar is stated in `geographie` commit `97a63fb`: one grid of cells, grey in, red out,
white operation boxes, an upward flow, dotted leaders instead of legends, inspired by *The Little
Book of Deep Learning*. Found a compilation bug, `font=\scripointsize` in
`af/current/reformes/note/note_af.tex:279`, and a TikZ diagram in `af` that predates the grammar.

## B3. Don't / do examples (integrated, §15)

The author asked whether examples would help. Answer: yes, for rules that need judgement (clever
code, abstraction, naming, declarations as data, failing loudly, writing notes, charts, web
controls), and no for rules that are already exact values. Real snippets from the repositories
were preferred to invented ones, and each pair is labelled real, adapted or illustrative.

## B4. One script, no options (decided, integrated §4.3 and §15.15)

Author's rule: there must be one script that reproduces everything, with no options. Only
`climat/run.sh` meets it. `geographie`'s preparation script has options, and `eau` has about twenty
commands to run by hand.

## B5. First assessment

Strengths: the epistemics of the notes; human decisions as replayable data; declarations as data;
failing loudly; the TikZ grammar.

Contested points:

1. **"No comments" applied absolutely** pushes explanations into very long READMEs (`eau` 1,760
   lines, `av` 1,262), where they go stale. Proposal: allow one "why" line for a constant, a
   workaround or an external constraint, and docstrings for a public API. **Open.**
2. **No tool enforces the bracket layout**, and it is incompatible with every formatter. **Open**
   (write a checker, or accept the drift).
3. **"Most recent export in `data/`"** makes the input implicit. Proposal: print the file read and
   write a manifest. **Proposed.**
4. **The single README has become a logbook.** Proposal: README for operation and rules, PLAN for
   decisions and findings. **Open.**

## B6. Severe critique (asked for: "what is really wrong")

1. It is not yet a system: nothing is shared (six copies of `gt.cls` in two versions, themes
   copied) and nothing is enforced (no CI in `eau`, `av`, `af`, `geographie`). Since most recent
   commits are co-written with Claude, prose rules do not hold; only mechanical checks do.
2. Reproducibility fails where it matters most: the numbers in the `eau` note « se reprennent à la
   main à chaque nouvel export » (`eau/README.md`, line 1654). This was the most serious finding.
3. Knowledge is not where it is needed: very long READMEs, no comments, no docstrings in
   `arpenteur` although it is an installable package.
4. Taste and correctness are given the same weight: there is no MUST / SHOULD distinction.
5. Accessibility and legibility are sacrificed: `#999` text, `outline: none`, `\tiny` and 9 px
   text, a value axis removed.
6. Small security issues: `causeries` inserts model output with `innerHTML`, loads an unpinned
   `marked` and keeps an API token in `localStorage`; `cinema` calls jQuery without loading it.
7. The document itself was too long, descriptive rather than normative, and mixed Claude's
   recommendations with the author's rules.

Proposed order: generate every number in `eau` as macros; create one shared repository; add CI
with a checker; reduce the standard to about 100 binding lines; relax the comment ban. **Proposed.**

## B7. Product principles (integrated, §2)

The author noted that UX and development principles were missing: when to build one application
or several, and what it must do. Added §2: start from the artefact and its reader; take the lowest
rung that works; split by mode of attention and audience; one source of truth; never two writers
on one file; what a tool must and must not do; when to automate (lessons of `eau`); how to
develop. Weakest parts, extrapolated from few cases: §2.3 and §2.4.

## B8. Further blind spots (proposed, not integrated)

1. Extraction bias: 14 of about 40 repositories, mostly recent and co-written with Claude.
   *Partly addressed in B9 and B10.*
2. No rules for working with an agent: review of agent-written code, verification of its claims,
   PR size, what the agent may decide alone. Only 2 of 14 repositories have a `CLAUDE.md`.
3. Data governance: `av` versions 268 CSV files exported from the CASD on GitHub (private
   repository), and cloud agent sessions read those repositories. Whether CASD and Cour rules allow
   this is not documented. **Open: the author's decision, not a code change.**
4. No tags or releases in any repository: which commit and which export produced a delivered note
   cannot be told. Proposal: a tag for each delivered artefact, plus a manifest of inputs with
   their checksums.
5. Tests check that code runs, not that numbers are right or stable; there is no regression test
   on a note's numbers and no human review of analyses.
6. The reader of a note is never defined (a magistrate, a rapporteur, the public).
7. Licences: only `arpenteur` and `cinema` have one. A public repository without a licence is "all
   rights reserved". Data attributions (IGN, Natural Earth, Pleiades, OSM) are not systematic.
8. No rule for the end of a project (archiving, handover).
9. No stopping criterion or time budget; `eau`'s first pipeline is the counter-example.

## B9. `cours` and `cinema` (integrated)

- `cinema` (2023) already shows the bracket layout, applied even to CSS closing braces, and has no
  comments, so these habits are the author's own and not the agent's. It is a monospace page with
  `<details>`, rebuilt every night by GitHub Actions. Its faults are listed in §14 #16.
- `cours` (April 2026) is the state before the system: rendered HTML only (41 MB, no sources),
  English, Python only, Quarto and matplotlib defaults, comments and `# TODO`. It is the course
  that `geographie` replaced.
- Added: the age table in the header, scheduled CI in §2.2, `<details>` in §8.2, "version sources,
  not renders" in §5.4, and §14 #16 and #17.

## B10. Older repositories (integrated)

Read `ggipp` (2020), `random/rart` (2022), `spirographe`, `cassettes`, `demographie` (2023),
`lexflation` and `polynome` (2026).

- **Correction**: "no comments" is not an old habit. Almost every older repository has comments.
  The rule dates from mid-2026 and holds only in the Cour repositories.
- **Constant since 2020**: the bracket layout with leading operators; an institution's chart
  packaged as a shared theme (`ggipp`, then `srpp`, then `theme_tufte_fr`); generative drawing
  (`rart`, then `spirographe`, then `alpes`); analysis framed as a question with counterfactual
  scenarios (`demographie`); plotnine.
- `polynome` is an instrument, so its controls are the product. It is also the only repository with
  correct accessibility (`:focus-visible`, `aria-pressed`).
- `cassettes` republishes commercial audiobooks downloaded from YouTube on a public page: almost
  certainly a copyright infringement (§14 #18). **Open: make it private or remove the audio.**
- `lexflation` uses red and green for additions and deletions (§14 #19).

## B11. Named principles (integrated, §1.1)

The author asked whether to cite mainstream principles (Tufte, tidy data, IMRAD). Classification:

- **Followed**: Tufte; Cleveland & McGill; Bertin; the grammar of graphics; tidy data (with wide
  tables kept for reading); literate programming; the Unix philosophy; Gall's law; the rule of
  three; the ASA statement on p-values; Bringhurst; Nielsen; the Insee and CASD disclosure rules.
- **Partly**: IMRAD, which the `eau` note follows exactly. A reader who decides needs the answer
  first, so a one-paragraph synthesis should come before the introduction (pyramid principle).
  **Open.**
- **To adopt**: Wilson et al. (*Good enough practices*), Sandve et al. (*Ten simple rules*),
  WCAG 2.2.
- **Departed from on purpose**: PEP 8, `black`, the tidyverse style guide.

## B12. Who writes (decided, integrated: principle 8, §3.4, §17)

Author's rule: an LLM may prepare the skeleton of a note or a text but is never allowed to write
it.

- **Allowed**: the plan, headings, the list of figures and tables, empty `% À RÉDIGER`
  placeholders, citation keys, number macros, consistency checks.
- **Forbidden**: any sentence, including drafts.
- **Scope as written**: delivered texts. README, PLAN and commit messages are excluded until the
  author decides. **Open.**

Several existing commits co-authored by Claude rewrite delivered prose (§14 #21).

## B13. Restructuring for use as a reference (proposed)

Purpose stated by the author: a reference to send to a person or an agent, so that they follow it
or check that a repository follows it. Proposal:

- **Split the files**:
  - `STANDARD.md`: rules only, 300 to 400 lines;
  - `EXAMPLES.md`: the don't/do pairs;
  - `RATIONALE.md`: reasons, named principles, history, reference files;
  - `audits/<date>.md`: drift reports on named repositories;
  - a `CLAUDE.md` generated from the binding rules.
- **Rule format**:
  - a stable identifier (CODE-03, A11Y-01…);
  - a level: MUST, SHOULD or MAY (RFC 2119);
  - a scope;
  - a check, automatic or a yes/no question;
  - an exception, if any;
  - no repository names in the rules.
- **An applicability matrix** by repository type: analysis leading to a note, course, tool or
  package, static page, instrument, generative art. Each repository declares its type and its
  deviations, with reasons, in its `CLAUDE.md`.
- **Audit**: `check.py` for the mechanical rules, a list of questions for the rest, and a report
  format (identifier, status, location, fix).
- **Hygiene**: each rule appears once; the standard obeys its own rules; a version, a date and a
  changelog.

## B14. Reusable objects (proposed)

The author asked for reusable objects (`gt.cls`, TikZ, Python, R). A first attempt to build them
was stopped by the author: think first. Proposal: one versioned repository, tentatively `charte`,
which projects pin by tag; copies are allowed only offline (CASD), checked by checksum.

1. **`tokens.yml`**: colours, fonts, sizes and stroke widths, from which every other object is
   generated. Ends the three-reds problem and allows automatic contrast checks.
2. **LaTeX**:
   - `gt.cls` with options (`garamond` or `palladio`, rubrication on or off), three heading levels,
     `\fullwidth`, `\figcolonne`, `\figpleine[label]{file}{caption}` with a visible placeholder,
     `\tabpleine`, `\rubric`, `\retenu`, `\citer`, `\sidenote`;
   - `gt-schemas.sty` (the TikZ grammar, with prefixed macro names);
   - templates: a note skeleton with a synthesis, IMRAD and `% À RÉDIGER`, a handout, a diagram
     test file.
3. **R package `charte`** (not `gt`, which already exists on CRAN): themes, scales, the helpers
   `av` learned the hard way (zero baseline, numeric factor, `drop = FALSE`, decimal comma,
   `sauver()` in centimetres with a size check). It takes an institutional palette as a parameter
   rather than including `srpp`.
4. **Python twin** on plotnine, plus a matplotlib style for geopandas maps, with a parity test.
5. **Web**: `charte.css` (tokens, reset, reading column, text buttons, focus, `<details>`, print)
   in two variants, `lecture` and `utilitaire`, plus a dependency-free accordion script.
6. **Quarto extension**: the `geographie` HTML format.
7. **Project template**: README skeleton, `CLAUDE.md` declaring type, version and deviations,
   `run.sh`, `.gitignore`, `ruff.toml`, CI, a licence.
8. **`check.py`**: the audit tool.
9. **Not made reusable**: the `arpenteur` maps and the `eau` Shiny tools (one case each), `srpp`
   (owned by the Cour), the diagrams themselves (content).

The repository's own CI would compile a sample note in both fonts, compare the same figure from R
and Python, and check the tokens' contrast. Order of work: tokens, `gt.cls` and the TikZ package,
R, Python, CSS and Quarto, the template and `check.py`, then migration of `eau`, `av`, `af` and
`geographie`, checking that the PDFs come out identical.

## B15. How this differs from usual design systems

Usual systems (Material, Carbon, GOV.UK, DSFR) are UI component libraries for many teams. This one
is a single author's practice manual. It differs in eight ways:

1. It covers code, method, writing, LaTeX and drawing, not only the interface.
2. It tries to remove interface rather than supply it.
3. It is made of rules rather than components.
4. It contains rules of evidence, closer to official-statistics codes of practice than to design.
5. It is enforced through agents.
6. It comes from print rather than the screen.
7. It departs from community standards on purpose.
8. It embraces taste (blind duels, an aesthetic charter).

It lags usual systems on accessibility, governance (versions, changelog, exceptions) and shared
objects. If a tool or page is ever published under the Cour's name, the institution's chart, and
possibly the DSFR and RGAA, would prevail; whether they apply was not checked.

## B16. Accessibility (proposed)

Contrast ratios, computed by hand with the WCAG formula (approximate):

| Colour | On | Ratio | Verdict |
|---|---|---|---|
| `#9E1B1B` | `#fcfcfc` | ≈ 7.8 | pass |
| `#03565E` | white | ≈ 8.4 | pass |
| `#286c44` | `#fcfcfc` | ≈ 6.2 | pass |
| `#6e6e6e` | `#fcfcfc` | ≈ 5.1 | pass |
| `#777` | white | ≈ 4.5 | fails narrowly |
| `#999` | `#fcfcfc` | ≈ 2.8 | fails |
| `#a0a0a0` | `#fcfcfc` | ≈ 2.6 | fails |
| `#bdbdbd` | `#fcfcfc` | ≈ 1.8 | fails if the marks carry meaning |

Proposed rules:

- **A11Y-01 and A11Y-02, contrast**: the lightest grey for text on `#fcfcfc` is about `#737373`;
  meaningful marks are no lighter than `#939393`; pure decoration is exempt.
- **A11Y-03, minimum sizes**: in print, 7 pt text and 0.4 pt data strokes; on screen, 12 px; in
  projection, 18 to 24 px, through a `support = "projection"` parameter in the theme.
- **A11Y-04, never colour alone**: places in `arpenteur` also get an underline and become real
  links; hatching for missing data is kept; direct labels are preferred.
- **A11Y-05, visible focus**: `:focus-visible` everywhere, never `outline: none` on its own.
- **A11Y-06, single-key shortcuts** can be turned off or are active only on focus (WCAG 2.1.4);
  shortcuts read `event.code`, so AZERTY letter rows keep their physical place.
- **A11Y-07, reduced motion**: honour `prefers-reduced-motion`.
- **A11Y-08, language**: a correct `lang` attribute; no `user-select: none` on text.
- **A11Y-09, text equivalents**:
  - `alt` text for figures;
  - a list beside every map;
  - the CSV of every figure published as an annex;
  - tagged PDFs from LaTeX, still to be tested (it is not known whether this is mature enough for a
    custom class).

`check.py` would cover contrast, focus, `alt`, `lang` and minimum sizes. A keyboard walk-through, a
screen-reader pass and a colour-blindness simulation stay manual. The legal scope of the RGAA for
the author's work was not checked.

## B17. Designing: find the form of the problem (proposed)

Prompted by `av/ficovie`. Every one of its 100 tables is the same operation: a population (filters)
× a breakdown (`par`) × a variable × a fixed summary of the distribution. `compute_distribution` is
called 52 times, every output has one long format
(`par…, nombre, personnes, effectif, indicateur, valeur`), and there is one exit point
(`ecrire_csv`, which applies statistical secrecy).

The same pattern appears in five repositories:

- `climat`: any question is a cell of a cross product, computable without new code;
- `eau`: excerpts × an issue/angle grid;
- `arpenteur`: mentions of three kinds, plus one correction log;
- `alpes`: one intensity field λ;
- `av/ficovie`, as above.

Proposed principles:

1. Name the form of the problem in one sentence before writing code.
2. Abstract along the domain, not the syntax. This reconciles the rule against premature functions
   with `compute_distribution`.
3. One canonical, long output format.
4. Every number carries its denominator and its unit.
5. Invariants live at the single point everything passes through.
6. Write down the computation boundaries (arrow until the unit level, then `collect()`; inside and
   outside the CASD).
7. Rehearse on fabricated data with the real schema (`smoke_casd.r`).
8. The form is a data structure and a plain function, not a framework.
9. For interfaces: the data model first, then the screen.
10. One screen per kind of decision, with the keyboard mirroring the model's categories.
11. Every view is fed by the same form.

Proposed removals in Part A:

- trim §2.6 to vertical slices, gates, blind duels and non-goals;
- state "generalise after a concrete case" only once;
- move repository anecdotes to the rationale;
- move "one documentation surface" from principles to rules.

## B18. Analysing: what is analysed, what matters (proposed)

**What is analysed**:

1. A question someone decides on, framed as a comparison or a counterfactual (`demographie`, `af`,
   `av`).
2. The mechanism first: `av` opens with the 70-year tax rule, which says where to look.
3. Distributions rather than means. The recurring subject is "who": redistribution, exposure,
   heterogeneity.
4. The right unit (commune, 200 m square, contract, person, household), with the change of unit
   treated as a problem in itself (MAUP, ecological fallacy).

**What matters**:

5. Measurement before the model: validate against humans, a hand-corrected reference or a
   published figure.
6. Orders of magnitude before precision: non-response bounds dominate; intervals, not stars.
7. Designs that can fail: placebo ages (60, 65, 75, 80), permutations, simulations asking "would
   the method see the effect if it were there?".
8. The tier of evidence stated, with wording to match.
9. Essential or accessory, certain or risky; read everything when the volume is small.
10. Coherence across chains: each quantity defined once, one reference series, one vocabulary.
11. Failures published (LISA maps dropped, modules that show nothing classified as design limits or
    data problems).
12. Confidentiality as a design constraint.

**Blind spots**:

- No analysis plan written before the results: `eau`'s revised plan came after the first
  measurements, and `croisements.py` runs many tests on about 100 departments. Write questions and
  tests first, separate planned from exploratory results, correct for multiple tests.
- No rule on data quality (`av`'s « Trois choses que le dictionnaire a démenties » is the model).
- No minimum detectable effect computed in advance.
- No traceability from each sentence to a table and a line of code.

Proposed order of the finished standard: analyse, design, produce, verify.

## B19. Designing pages and tools (proposed)

**First choose the genre**:

| Genre | Reader's task | Examples | Rules |
|---|---|---|---|
| read | follow a text | `arpenteur` reader and book, `embeddings` | text leads; narrow column; Palladio; no controls |
| consult | find a fact | `cinema`, `meteo`, personal site, `lexflation` | one page; `<details>`; monospace; rebuilt by CI |
| explore a result | compare, locate | `eau/carte.html` | linked views; a sentence instead of a filter panel |
| judge, annotate | make many decisions | `codage`, `decoupage`, `arpenteur` review | one kind of decision per screen; keyboard; state on disk; local |
| play | manipulate | `polynome` | controls are the product |
| contemplate | look | `thalweg` | the image fills the page; deep zoom; nothing around it |

**Principles already applied**:

1. The content is the interface: click the word, click the place; the map follows the text.
2. Overview first, details on demand (Shneiderman, 1996).
3. Linked views.
4. Everything has an address (`#12`), and state survives a reload.
5. Uncertainty is visible:
   - a hollow marker for an approximate place, a dotted underline for a doubtful mention;
   - hatching for non-response;
   - dashed outlines for model suggestions.
6. The human decides: suggest without applying, undo, refuse invalid states.
7. Defaults instead of options; settings live in a configuration file.
8. Motion only for orientation.
9. Works without the network: no server, fallbacks, prefetching, space reserved for images,
   `thalweg`'s divisor grid.
10. No framework, no trackers, a library only when it does the core job (MapLibre, KaTeX,
    OpenSeadragon).

**Blind spots**:

- **Discoverability**: every hidden interaction needs one visible hint or a "?" page.
- **Touch**: hover-only interactions (`eau` map, `lexflation`) need a tap equivalent.
- **Loading, empty, error and stale states**; a page rebuilt by CI shows the date of its data.
- **Testing with someone else**: watch one person use the tool unaided for ten minutes before
  handing it over.
- **Security**: never inject foreign HTML; pin every library.
- **Leaving the page**: how to cite, print or export what is shown.

---

# Part C. Open decisions

| # | Decision | Options | Raised in |
|---|---|---|---|
| 1 | "Why" comments | absolute ban, or one "why" line allowed for constants, workarounds and external constraints | B5 |
| 2 | Documentation of shared packages | README only, or docstrings and roxygen for public APIs | B5, B14 |
| 3 | Python indentation | 2 spaces (`geographie`) or 4 (`eau`, `arpenteur`, ruff default) | §14 #3 |
| 4 | R file extension | `.r` (written rule in `av`) or `.R` | §14 #4 |
| 5 | Bracket layout enforcement | write a checker, or accept the drift | B5 |
| 6 | README and PLAN | README alone, or README for operation plus PLAN for decisions and findings | B5 |
| 7 | Scope of "the model never writes" | delivered texts only, or also README, PLAN and commits | B12 |
| 8 | Note structure | pure IMRAD, or a synthesis first then IMRAD | B11 |
| 9 | Scope of "no options" | analysis scripts only, or tools too | B4, §14 #20 |
| 10 | Language of the standard | French or English | B13 |
| 11 | Name of the shared repository and packages | `charte` or another name, not `gt` | B14 |
| 12 | Prefix for TikZ macros | e.g. `\gtgrille`, `\gtcase` | B14 |
| 13 | Default font | EB Garamond or TeX Gyre Pagella | B14 |
| 14 | Delivery to the CASD | versioned archive, or copy checked by checksum | B14 |
| 15 | Shared repository public or private | fonts are free, so no obstacle either way | B14 |
| 16 | Canonical red | `#9E1B1B` proposed | §14 #5 |
| 17 | Type hints in Python | required, allowed or banned | §14 #8 |
| 18 | CASD exports on GitHub and agent access | check with CASD and Cour rules | B8 |
| 19 | `cassettes` | make private, or stop publishing the audio | B10 |
| 20 | Licences for public repositories | which licence, and data attributions | B8 |
| 21 | Rules for working with agents | review, verification of claims, PR size, autonomy | B8 |
| 22 | Analysis plan before results | required for notes, or not | B18 |

---

# Part D. Plan for the finished project

1. **Settle Part C**, at least decisions 1 to 10.
2. **Write the standard** in the order analyse, design (code, pages and tools), produce (code,
   charts, LaTeX, TikZ, Quarto, web, accessibility), verify. Use the rule format and the
   applicability matrix of B13, and fold in B16 to B19.
3. **Move everything else out**: examples to `EXAMPLES.md`; reasons, named principles and history
   to `RATIONALE.md`; §14 to `audits/2026-10.md`.
4. **Build `charte`** as in B14: tokens first, then LaTeX, R, Python, web and Quarto, the project
   template and `check.py`.
5. **Migrate** `eau`, `av`, `af` and `geographie` to `charte`, checking that their PDFs come out
   identical, then fix the MUST-level findings of the audit, starting with the `eau` numbers.
6. **Generate the `CLAUDE.md` block** from the MUST rules, and declare type, version and deviations
   in each repository.
