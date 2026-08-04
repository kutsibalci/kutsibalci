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

**Open to internships, junior backend roles and open-source collaboration.**

<br>

## Open Source

**[root-project/root#23002](https://github.com/root-project/root/pull/23002) — merged into [ROOT](https://root.cern), CERN's data analysis framework for particle physics.**

`tmva/tmva/inc/LinkDef5.h` had been unreachable since 2015. The commit that split TMVA into
`libTMVA` and `libTMVAGui` dropped that file's `#include` from the master LinkDef but left the
file itself in the tree, and two later maintenance sweeps edited it without noticing it was
already dead. I traced the commit that orphaned it, checked that every symbol it declared was
already covered by the module that actually owns those classes, and confirmed against the
generated build graph that nothing referenced it — then proposed the removal.

Still open: [ROOT #23004](https://github.com/root-project/root/pull/23004), the same class of
leftover in `smatrix` and `genvector`; a latent
[undefined-behaviour fix](https://github.com/eclipse-score/baselibs/pull/444) in
[Eclipse S-CORE](https://github.com/eclipse-score), the BMW/Bosch/Mercedes automotive platform;
and [an issue](https://github.com/nasa/fprime/issues/5570) in
[NASA's F´](https://github.com/nasa/fprime) flight software framework.

What I keep relearning here: the patch is the easy part. Proving the claim before making it is
the actual work — several findings I was sure about turned out to be false positives, and never
left my machine.

<br>

## Featured — Seat Reservation API

**[seat-reservation-api](https://github.com/kutsibalci/seat-reservation-api)** — a ticketing
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

`.NET 10` · `PostgreSQL` · `Redis` · `JWT with refresh rotation` · `Clean Architecture` ·
`Testcontainers` · **65 tests** · `Docker Compose` · CI

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
| **API design** | Paginated, validated REST endpoints — with ordering that makes pagination stable and ceilings on anything read into memory |
| **Data modelling** | Normalised schemas, code-first migrations, and constraints in the database rather than only in application code |
| **Deployment** | Docker Compose and AWS EC2, with credentials from the environment and nothing sensitive published on a port |
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
      <a href="https://github.com/kutsibalci/Course-Registration-System">
        <img src="./assets/card-course.svg" alt="Course Registration System — ASP.NET Core MVC, EF Core, SQLite" width="100%" />
      </a>
    </td>
    <td width="50%">
      <a href="https://github.com/kutsibalci/business-directory-api">
        <img src="./assets/card-data.svg" alt="Business Directory API — FastAPI, SQLAlchemy, Alembic" width="100%" />
      </a>
    </td>
  </tr>
  <tr>
    <td width="50%">
      <a href="https://github.com/kutsibalci/File-Analysis">
        <img src="./assets/card-analysis.svg" alt="File Analysis Service — FastAPI, YARA, Celery, Docker" width="100%" />
      </a>
    </td>
    <td width="50%">
      <a href="https://github.com/kutsibalci/Redmine-Upgrade">
        <img src="./assets/card-redmine.svg" alt="Redmine Deployment — Docker Compose, PostgreSQL, AWS EC2" width="100%" />
      </a>
    </td>
  </tr>
  <tr>
    <td width="50%">
      <a href="https://github.com/kutsibalci/Small-coffee-Shop-Management-App">
        <img src="./assets/card-coffee.svg" alt="Coffee Shop Management — C#, Windows Forms, MySQL" width="100%" />
      </a>
    </td>
    <td width="50%">
      <img src="./assets/card-pansuman.svg" alt="Pansuman Simulator — Unity, URP, C#" width="100%" />
    </td>
  </tr>
</table>

Some of this is team work — the Redmine deployment was built with **Atakan MERGEN**
([@hzflora](https://github.com/hzflora)), whose repositories I also contribute to.

<br>

## Currently Learning

1. Concurrency beyond a single row — the outbox pattern, message queues, idempotent consumers
2. SQL query planning — reading execution plans instead of guessing at indexes
3. Data structures and algorithms, properly rather than for exams

<br>

---

<div align="center">

**Get in touch** — <a href="mailto:balcihkutsi@gmail.com">balcihkutsi@gmail.com</a>

<sub>İzmir, Türkiye · open to remote and hybrid roles</sub>

</div>
