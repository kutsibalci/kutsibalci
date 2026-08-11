<div align="center">
  <img src="./assets/hero.svg" alt="Hüseyin Kutsi Balcı — Backend Development Student, Anadolu University" width="100%" />
</div>

<br>

Backend development student at **Anadolu University**, based in **İzmir, Türkiye**.
I build server-side systems end to end — schema first, then the API, then the container
it ships in.

Most of what is here started as something I wanted to understand rather than something I
was told to build: how an ORM actually maps a schema, what a request touches between the
route and the database, what it takes to run a service on a server instead of a laptop.

Some of it has a user who is not me. I build and maintain a **pre-accounting and fleet
tracking system** in daily use at a haulage company, and I ship **mobile applications** in
Flutter and React Native — offline-first, because a phone in a truck cab has no network to
depend on. Alongside that, **peer-to-peer**: a WebRTC mesh where the server only carries
signalling and the media never touches it.

**Open to internships, junior backend roles and open-source collaboration.**

<br>

## Open Source

Eight contributions merged into projects I had no prior connection to — two into
**[CERN's ROOT](https://root.cern)**, and one each into
**[.NET runtime](https://github.com/dotnet/runtime)**, the Rust project's
**[GCC codegen backend](https://github.com/rust-lang/rustc_codegen_gcc)**,
**[systemd](https://github.com/systemd/systemd)**,
**[Eclipse S-CORE](https://github.com/eclipse-score)**,
**[Apache Airflow](https://github.com/apache/airflow)** and
**[the VS Code documentation](https://github.com/microsoft/vscode-docs)**.

**[root-project/root#23002](https://github.com/root-project/root/pull/23002)** — `tmva/tmva/inc/LinkDef5.h`
had been unreachable since 2015. The commit that split TMVA into `libTMVA` and `libTMVAGui` dropped that
file's `#include` from the master LinkDef but left the file itself in the tree, and two later maintenance
sweeps edited it without noticing it was already dead. I traced the commit that orphaned it, checked that
every symbol it declared was already covered by the module that actually owns those classes, and confirmed
against the generated build graph that nothing referenced it — then proposed the removal.

**[root-project/root#23004](https://github.com/root-project/root/pull/23004)** — the same shape once I knew
to look for it. `math/smatrix/inc/LinkDefAll.h` and `math/genvector/inc/Math/LinkDef_GenVectorAll.h` exist
only to `#include` the two real LinkDefs of their package, from a time when each package built one combined
dictionary. The build has since gone back to two dictionaries per package, each naming its own LinkDef
directly, so the aggregates lost their only caller. Neither name appears in any `CMakeLists.txt` or `.cmake`
file, no other LinkDef includes them, and `ROOT_INSTALL_HEADERS` excludes LinkDef headers from the install,
so they are not reachable from outside the repository either. One commit per module, because the two are
independently revertable.

**[dotnet/runtime#131865](https://github.com/dotnet/runtime/pull/131865)** — eight documentation links whose
targets exist but whose relative paths resolve nowhere. The one I liked was in the datacontracts design docs:
they link to `contract_descriptor.md` while the file is `contract-descriptor.md`, and the same document links
to it *correctly* twice elsewhere. So it was an inconsistency inside one file rather than a rename nobody
finished. Another was written with Windows backslashes. The repository has no markdown link checker in CI,
which is why they rotted quietly.

**[rust-lang/rustc_codegen_gcc#945](https://github.com/rust-lang/rustc_codegen_gcc/pull/945)** — I found these
while sweeping `rust-lang/rust`, and the useful part was working out that I was in the wrong repository.
`compiler/rustc_codegen_gcc` is a subtree synced *from* its own project, so a fix landed upstream would have
been overwritten on the next sync. One link pointed at `./doc/gimple.md` from a file already inside `doc/`.
The other pointed at a file deleted a year earlier; I traced the commit that removed it and found the content
had survived inside a broader `debugging.md`, so the entry could be repointed instead of dropped.

**[systemd/systemd#43300](https://github.com/systemd/systemd/pull/43300)** — seven cross-references naming a
man page that exists but giving a section it is not installed in, so `man` sends the reader to the wrong
place. I resolved every `<citerefentry>` in `man/` against `man/rules/meson.build`, which is generated and is
therefore the authoritative list of what ships, and reported only references whose target *is* shipped under a
different section — the section is a fact, not a judgement. The part I would defend in review is what happened
next: CI went red. Rather than call it flaky, I pulled the 5.4 MB job log and found `test-fiber` timing out on
ppc64le with `Ok: 1779, Fail: 0` — nothing failed, one thing did not finish, and a change to six XML files
cannot reach it. I said so, and said plainly that I could find no prior report of that timeout. Two core
maintainers merged it hours later, with the job still red.

**[eclipse-score/communication#853](https://github.com/eclipse-score/communication/pull/853)** — four links in
the design docs of the BMW/Bosch/Mercedes automotive platform. Three used one `../` too many; one image URL
was written `hhttp://`, so a PlantUML diagram had never rendered. The sibling diagram in the same file already
used the correct form, which is what turned a guess into a check.

**[apache/airflow#71179](https://github.com/apache/airflow/pull/71179)** — twenty links in the translation
guide that 404 for every reader while opening fine for every author. The `i18n` README lives under a
directory that Git stores as a **symlink blob**, and GitHub will not traverse one: clone the repository and
the paths resolve, browse it on the web and they do not. That is why nobody had noticed. Three hundred and
seventy-two candidates went in; twenty came out.

**[microsoft/vscode-docs#10119](https://github.com/microsoft/vscode-docs/pull/10119)** — three `setting(...)`
macros naming a setting ID that VS Code does not register, because the casing is wrong: the docs write
`chat.mcp.autoStart` and `Microsoft-sovereign-cloud.environment`, the product registers both in lowercase.
`ConfigurationRegistry` indexes settings in a plain record keyed by the exact ID string, so a differently-cased
key is not the same setting spelled differently — it is an unregistered one. Pasting the documented ID into
`settings.json` gets you the *Unknown Configuration Setting* marker and no behaviour, and on the rendered page
the macro becomes an interactive control that carries the bad ID verbatim. What made it a check rather than a
guess: the four sibling `chat.mcp.*` rows in the same table match the source exactly, and the default shown in
the offending row matches the real setting's enum — so the row describes the right setting and only the ID is
misspelled. I resolved all 771 distinct IDs behind the repository's 1,760 macros against what
`microsoft/vscode` actually registers, which needed two traps handled first: editor settings never appear as a
full literal, so `editor.overtypeCursorStyle` is assembled from an `EditorOption` name and reads as missing to
a naive sweep; and GitHub code search is token-based, so searching `chat.mcp.autoStart` happily matches
`chat.mcp.autostart` and cannot settle a casing question at all. The comparison had to run locally against a
case-sensitive index. A fourth instance lives in release notes for a shipped version, which are a dated record
rather than a document to correct, so I left it alone and said so.

Not every finding should be a pull request.
**[root-project/root#23036](https://github.com/root-project/root/issues/23036)** — `system.rootrc` ships
three settings that nothing in ROOT reads. `WebGui.HttpLoopback` is documented in *two* places while
loopback binding is actually controlled by a file-static variable reachable only from C++; the corroboration
was that in the block where it should have been read, it is the only value not taken from `gEnv` — the six
around it are. Whether each should be deleted or wired up is a maintainer's call, so I filed both directions
rather than picking one. Sergey Linev, who wrote the web GUI, replied *"Yes, some parameters were not cleaned
up"*, opened the fix half an hour later, and it merged with two core approvals. No commit under my name — a
confirmed report instead, which for that class of finding is the honest outcome.

What I keep relearning here: the patch is the easy part. Proving the claim before making it is the actual
work. Those eight .NET links came out of thirty-nine candidates. In the VS Code sweep the tool I reached for
first was the wrong one twice over — GitHub code search is token-based, so it answers a casing question with
a confident yes no matter which casing you type, and a naive scan flags every editor setting as missing
because `editor.overtypeCursorStyle` is never written down anywhere, it is assembled at runtime from an
option name and a prefix. Both had to be understood before a single finding meant anything. The sweep behind
the ROOT settings ran against systemd first and found nothing at all: all thirteen config parser tables
agreed with their man pages, and every apparent mismatch was a deliberate compatibility alias or a page
shared by `xi:include`. A sweep that finds nothing is a result too, and one that finds plenty is usually
wrong. Several findings I was sure about never left my machine, and what is listed above is what a
maintainer agreed with — not what I proposed.

<br>

## Featured — Concurrent Ticketing

**[concurrent-ticketing](https://github.com/kutsibalci/concurrent-ticketing)** — a ticketing
API built around one question: **what stops the same seat from being sold twice?**

Two customers open the same event page and click seat A12 in the same millisecond. The
obvious implementation reads the seat, sees `Available`, and writes `Held` — and so does the
other request, because both read before either wrote. Nothing is wrong with either line; the
bug lives in the gap between them, and it only shows up under load.

The fix is to make the check and the write one statement. `Seat` maps PostgreSQL's `xmin`
system column as an EF Core concurrency token, so the write carries the version the read saw:

```sql
UPDATE seats SET status = 1 WHERE id = @id AND xmin = @version;
```

The first transaction to commit changes `xmin`. The second matches zero rows and gets a 409
instead of overwriting a sale. No table locks, no `SELECT FOR UPDATE`, and two customers
buying *different* seats never contend.

Measured rather than asserted — twenty concurrent requests, one seat, real PostgreSQL in a
container:

```
20 concurrent requests → 1 × 201 Created, 19 × 409 Conflict
database: 1 held seat, 1 reservation
```

The second race is telling anyone about it. Confirming a reservation writes to PostgreSQL
and publishes to RabbitMQ, and no transaction spans both — publish first and the broker
may hold an event for a commit that fails; publish after and the process can die in
between. So the event is written as a row in the **same transaction** as the reservation,
and a dispatcher moves it to the broker afterwards. That is at-least-once rather than
exactly-once, and the consumer absorbs the difference: a receipt row keyed on the message
id, inserted alongside the work, so a duplicate delivery hits the primary key instead of
sending a second e-mail.

`FOR UPDATE SKIP LOCKED` is what lets a second dispatcher be started at all — `FOR UPDATE`
alone would make it queue behind the first.

The tests I value most here are the ones added last. With 84 passing, running the stack by
hand showed `POST /api/auth/register` accepting a three-character password and the literal
string `bu-bir-email-degil` as an e-mail address: the contracts carried `[Required]` and
`[MinLength]`, but nothing evaluated them, and no test crossed the HTTP boundary where they
live. A green suite says the tested thing works. It says nothing about the untested one.

`.NET 10` · `PostgreSQL` · `RabbitMQ` · `Redis` · `JWT with refresh rotation` ·
`Clean Architecture` · `Testcontainers` · **119 tests** · `Docker Compose` · CI

<br>

## Watch Party Sync Engine — where one instance stops being enough

**[watch-party-sync-engine](https://github.com/kutsibalci/watch-party-sync-engine)** — watching video
together in sync: YouTube, your own uploads or a shared screen, with voice and video chat on top. It
deliberately does not touch DRM-protected content; the interesting problem is not the video, it is holding
thousands of sockets in agreement about a single room's state.

Room state changes go through a **Redis Lua** script, so the check and the write are one atomic step rather
than a read-then-write across the network — the same shape as the ticketing seat lock, one layer down. The
transcoding pipeline is ffmpeg producing HLS renditions, driven by a job queue I wrote rather than pulled in,
because the retry and visibility-timeout semantics were the part I wanted to understand.

The claim I care about is horizontal scaling, so it is measured rather than asserted. Latency here is the
round trip from a command to the broadcast arriving back at the *same* client — socket → Redis Lua →
`PUBLISH` → subscribing instance → socket:

| Setup | Connections | p95 | p99 | |
|---|---|---|---|---|
| 1 instance | 800 | 5 ms | 30 ms | healthy |
| 1 instance | 2,500 | **258 ms** | **1,609 ms** | saturated |
| 2 instances | 2,500 | **14 ms** | 47 ms | healthy |
| 2 instances | 5,000 | 27 ms | 74 ms | healthy |

Doubling the instances did not halve the latency: p95 fell by a factor of eighteen and p99 by thirty-four.
That is what queueing looks like once a system is past saturation — not a linear resource you can buy back.

The part worth keeping is the measurement that was wrong. At 5,000 connections the join handshake showed a
p95 of **9,400 ms**, and I spent two rounds fixing the wrong thing: the Postgres pool, then the TCP accept
backlog. Neither moved the number. Three pieces of evidence then pointed the other way — HTTP on the same
host stayed at 3 ms p50 while the handshake was at nine seconds, and server-side instrumentation put 99.2% of
joins under 250 ms. The decisive test changed nothing on the server and split the same 5,000 virtual users
across two k6 containers: **9,400 ms → 97 ms**. The bottleneck was the load generator scheduling five
thousand socket event loops in one process. Without server-side numbers I would have optimised a problem that
did not exist.

`TypeScript (strict)` · `Node 24` · `Redis Lua` · `PostgreSQL` · `ffmpeg / HLS` · `k6` ·
`Prometheus + Grafana` · **104 tests**, including a real Chrome run

### Peer-to-peer, in the same project

The video and voice on top of it do not go through the server at all. Media is a **WebRTC mesh**: every
participant holds a direct connection to every other one, and the server carries only signalling — offers,
answers and ICE candidates relayed over the same socket the room already uses. That choice has a ceiling
written into it. Mesh cost grows with the square of the participants, so it is capped at `MAX_MEDIA_PEERS`;
past that the answer is an SFU, and pretending otherwise would just move the failure to whoever joins tenth.

Two things I only understood by getting them wrong.

**Who offers.** The first version let the lower id offer and the higher id wait. If the participant who turned
their camera on happened to be the higher id, they could not offer, and the other side — with no media to
send — produced an empty offer instead. The connection came up perfectly and carried nothing. It now uses
**perfect negotiation**: anyone with media may offer, and if both do at once, the *polite* peer rolls its own
offer back and takes the other's. Politeness is decided by id ordering, so both ends reach the same verdict
without asking each other.

**Which stream.** I was passing `e.streams[0]` straight up from `ontrack`. Open the microphone, then the
camera, and renegotiation fires a second event whose stream carries audio only — so the video track arrived,
decoded fine, and was simply not in the object the UI was rendering. The fix is to keep one `MediaStream` per
peer and add tracks to it as they arrive. Both bugs looked like connectivity failures and neither was.

The honest limitation: there is public STUN and no TURN. Behind symmetric NAT — corporate networks, some
mobile carriers — the peers will not find each other, and a real deployment needs a relay such as coturn.
That is written in the source next to the ICE configuration rather than discovered by whoever runs it.

<br>

## What testing an old project taught me

<div align="center">
  <img src="./assets/architecture.svg" alt="Course Registration System architecture" width="100%" />
</div>

**[Course Registration System](https://github.com/kutsibalci/Course-Registration-System)** — an
ASP.NET Core MVC application that worked. Adding tests to it was supposed to be a formality:
writing down behaviour that already held.

Several of the first tests failed.

| Finding | What it meant |
|---|---|
| `AdminController` had no `[Authorize]` | The whole management area answered anonymous requests — `POST /Admin/KursSil` deleted a course with no session at all |
| Administrator credentials were string literals | The working password shipped with the source |
| Passwords stored in clear text | Reading the database was reading every password |
| Cancellation never checked ownership | Any signed-in student could cancel anyone else's place by incrementing an id |
| Capacity was a read-then-write race | Counted, compared, then inserted |

The last one is the one worth measuring. Reproducing the original logic under 15 concurrent
applications to a course with capacity 5:

```
old logic (count → compare → insert):   15 enrolled     ← 3× over capacity
current logic (conditional UPDATE):       5 enrolled
```

62 tests now, and CI that fails the build on any dependency with a known advisory.

The same read-then-write shape turned up in the coffee shop till, and I only found it
because I was trying to make the ordering logic testable. Adding the first item to a table
read the table's state, saw it free, then opened a tab — so two waiters on two terminals
both read *free* and both opened one. The order screen only ever shows the newest tab, so
everything written to the other was never billed. One conditional `UPDATE` closes it, the
same way the course capacity was closed. Third time I have written that fix now; I have
stopped thinking of it as a trick and started looking for the shape.

<br>

## Software someone actually runs — pre-accounting and fleet tracking

<div align="center">
  <img src="./assets/card-onyuz.svg" alt="Ön Yüz Muhasebe Sistemi — pre-accounting and logistics tracking, private" width="100%" />
</div>

The one on this page with a user who is not me. **Ön Yüz Muhasebe Sistemi** is a pre-accounting and fleet
tracking system for a small haulage company: trips, fuel, expenses and vehicle paperwork on one side, current
accounts, invoicing, cheques and collections on the other, with the link between them being that a month of
trips is what a customer's invoice is made of. *(Private — it holds a real company's ledger.)*

**Two ways of earning, one invoice model.** Trucks on a standing contract with a factory are billed per trip
at an agreed rate, so a *Job* is opened once and every trip inherits its customer, route and price. Trucks
working the spot market have no job at all — each load is a different firm, and at month end the unbilled
trips for a chosen customer are consolidated into a single invoice. If the same customer gave you both kinds
of work, both land on the same invoice. Getting that to fall out of one data model, rather than two parallel
ones with a reconciliation step between them, was most of the design.

Turkish e-invoicing means the output is **UBL-TR XML**, not a PDF with the right words on it — a schema with
mandatory ordering and identifier rules that the tax authority validates and the customer's accountant
rejects on sight if it is wrong.

**Deployment is the part I would defend.** The user runs a haulage yard, not a server. So: copy the folder,
double-click, and a browser opens on `localhost`. A portable Node runtime ships inside the folder, and the
program touches no registry key, writes nothing to `AppData`, installs no service or startup task, and makes
no network connection at all. Uninstalling is deleting the folder. Every one of those is a decision to *not*
use the convenient thing, and together they are why the software can be handed over on a USB stick and
trusted by someone with no way to audit it.

Node.js server · vanilla JS front end · SQLite · UBL-TR e-invoice · an append-only audit log ·
reports · a seed script that builds a demo dataset and a reset script that backs up before it wipes

<br>

## Mobile

<div align="center">
  <img src="./assets/card-mobile.svg" alt="Mobile applications — Flutter, React Native, Expo, offline-first" width="100%" />
</div>

Four applications, all private, at different stages of finished. The common constraint is that a phone in the
field has no network you can rely on, so state lives on the device and the schema is designed around that
rather than patched for it afterwards.

**Sefer Defteri** — the driver-side companion to the accounting system above. Trips, documents and records
entered from the cab, held in on-device SQLite, with expiry reminders as local notifications and an
export/backup path out. React Native on Expo, in TypeScript, with the database schema and query layer written
by hand rather than through an ORM.

**Sınav Motoru** — not one exam app but an **engine** for many. A pure-Dart core that knows nothing about any
particular exam, plus per-exam question banks as remote-versioned JSON and per-exam build flavors, so each
exam ships as its own Play Store listing off one codebase. Adding an exam is one JSON file, one flavor config
and one store entry — no new code. Spaced repetition for wrong answers, `sqflite` for progress, offline after
first load. First product is the Turkish SRC vocational driving certificates; the store ranking research
behind the one-app-per-exam decision is written down in the repository, because it is the reason the
architecture looks the way it does.

**Kurye Rota** — courier routing, where the ordering turned out to be the easy half. Stops arrive as a block
of text pasted out of WhatsApp, so the app parses free-form Turkish addresses into province / district /
neighbourhood / street / number / recipient. All 81 provinces, ~970 districts and ~50,000 neighbourhoods are
embedded as JSON and it works with no connection; the 2.7 MB neighbourhood file is loaded on first use rather
than at startup. Matching is word-based rather than string-based, because searching over the whole string
destroys the original Turkish characters and leaves you unable to cleanly remove the part that matched. Two
rules earned themselves: *"X Mah"* blocks district matching, or `Kızılay Mah ... Çankaya Ankara` resolves the
neighbourhood as "Çankaya" and sends a courier across the city; and when a neighbourhood name exists in
several districts the app **does not guess** — it puts the candidates on screen as buttons. Coordinates come
from a three-step cascade — local address book, then Nominatim, then Google — so the paid provider is
reached only when the two free ones fail, and a result below district-level confidence is rejected rather
than shown. Ordering is nearest-neighbour followed by 2-opt: an open-route TSP, milliseconds for a hundred
stops, and a couple of percent off optimal, which is the right trade when a courier is standing in the street
waiting for the list.

**Minik Masal** — an audio story player for small children in Flutter, with a parent gate in front of
anything a four-year-old should not reach on their own.

`Flutter / Dart` · `React Native / Expo` · `TypeScript` · `SQLite · sqflite · expo-sqlite` ·
`offline-first` · `local notifications` · `Android build flavors`

<br>

## Also Building — File Analysis Service

<div align="center">
  <img src="./assets/analysis.svg" alt="File Analysis Service architecture" width="100%" />
</div>

A pipeline that scans uploads with **YARA** rules, parses PE structure with `pefile`, and
submits samples to a **CAPE** sandbox. Work is queued through **Redis** to a **Celery** worker
rather than blocking the request — analysing an untrusted file is slow, and it has no business
happening inside an HTTP handler.

The lesson that stuck came from a bug in my own code: YARA compile errors were caught by a bare
`except` and skipped, so a rule file with a syntax error made every sample come back clean. For
a scanner, *no findings* and *the scan never ran* look identical from the outside, and only one
of them means the file is safe. A crash is a good outcome; a false negative is the bad one.

<br>

## Focus

| Area | What I'm actually doing about it |
|---|---|
| **Concurrency** | Optimistic concurrency against a real database, and tests that genuinely race rather than asserting they would |
| **Messaging** | Transactional outbox, at-least-once delivery, idempotent consumers, dead-letter queues — RabbitMQ driven directly rather than through a framework, because the mechanics are the point |
| **API design** | Paginated, validated REST endpoints — with ordering that makes pagination stable and ceilings on anything read into memory |
| **Data modelling** | Normalised schemas, code-first migrations, and constraints in the database rather than only in application code |
| **Peer-to-peer** | WebRTC mesh with perfect negotiation and glare handling, signalling over an existing socket — including knowing where mesh stops and an SFU has to start |
| **Mobile** | Flutter and React Native, offline-first with on-device SQLite, and one codebase shipping as several store listings through build flavors |
| **Deployment** | Docker Compose and AWS EC2 — and, at the other end, software a non-technical user installs by copying a folder, with a portable runtime inside it and nothing written outside it |
| **Analysis tooling** | Static and dynamic file analysis with YARA and Celery — the area I find most interesting right now |

<br>

## Stack

<div align="center">
  <img src="./assets/stack.svg" alt="Technology stack" width="100%" />
</div>

<br>

## Projects

<table>
  <tr>
    <td width="50%">
      <a href="https://github.com/kutsibalci/watch-party-sync-engine">
        <img src="./assets/card-watchparty.svg" alt="Watch Party Sync Engine — TypeScript, Redis Lua, ffmpeg, WebSocket" width="100%" />
      </a>
    </td>
    <td width="50%">
      <a href="https://github.com/kutsibalci/concurrent-ticketing">
        <img src="./assets/card-ticketing.svg" alt="Concurrent Ticketing — .NET 10, PostgreSQL, RabbitMQ, Redis" width="100%" />
      </a>
    </td>
  </tr>
  <tr>
    <td width="50%">
      <!-- Not linked: the repository is private and holds a real company's ledger. -->
      <img src="./assets/card-onyuz.svg" alt="Ön Yüz Muhasebe Sistemi — Node.js, SQLite, UBL-TR e-invoice" width="100%" />
    </td>
    <td width="50%">
      <!-- Not linked: these repositories are private. -->
      <img src="./assets/card-mobile.svg" alt="Mobile — Flutter, React Native, Expo, offline-first" width="100%" />
    </td>
  </tr>
  <tr>
    <td width="50%">
      <a href="https://github.com/kutsibalci/Course-Registration-System">
        <img src="./assets/card-course.svg" alt="Course Registration System — ASP.NET Core MVC, EF Core, SQLite" width="100%" />
      </a>
    </td>
    <td width="50%">
      <!-- Not linked: the repository is private. Restore the link when it goes public. -->
      <img src="./assets/card-data.svg" alt="Business Directory API — FastAPI, SQLAlchemy, Alembic" width="100%" />
    </td>
  </tr>
  <tr>
    <td width="50%">
      <!-- Not linked: the repository is private. Restore the link when it goes public. -->
      <img src="./assets/card-analysis.svg" alt="File Analysis Service — FastAPI, YARA, Celery, Docker" width="100%" />
    </td>
    <td width="50%">
      <!-- Not linked: the repository is private. Restore the link when it goes public. -->
      <img src="./assets/card-redmine.svg" alt="Redmine Deployment — Docker Compose, PostgreSQL, AWS EC2" width="100%" />
    </td>
  </tr>
  <tr>
    <td width="50%">
      <a href="https://github.com/kutsibalci/Small-coffee-Shop-Management-App">
        <img src="./assets/card-coffee.svg" alt="Coffee Shop Management — C#, Windows Forms, MySQL" width="100%" />
      </a>
    </td>
    <td width="50%">
      <!-- Not linked: the repository is private. Restore the link when it goes public. -->
      <img src="./assets/card-pansuman.svg" alt="Pansuman Simulator — Unity, URP, C#" width="100%" />
    </td>
  </tr>
</table>

Some of this is team work — the Redmine deployment was built with **Atakan MERGEN**
([@hzflora](https://github.com/hzflora)), whose repositories I also contribute to.

<br>

## Currently Learning

1. SQL query planning — reading execution plans instead of guessing at indexes
2. What breaks when one service becomes several: distributed tracing, and knowing which
   failures a retry actually fixes
3. Data structures and algorithms, properly rather than for exams

<br>

---

<div align="center">

**Get in touch** — <a href="mailto:balcihkutsi@gmail.com">balcihkutsi@gmail.com</a>

<sub>İzmir, Türkiye · open to remote and hybrid roles</sub>

</div>

