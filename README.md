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
and two issues in [NASA's F´](https://github.com/nasa/fprime) flight software framework.

What I keep relearning here: the patch is the easy part. Proving the claim before making it is
the actual work — several findings I was sure about turned out to be false positives, and never
left my machine.

<br>

## Featured

<div align="center">
  <img src="./assets/architecture.svg" alt="Course Registration System architecture" width="100%" />
</div>

**[Course Registration System](https://github.com/kutsibalci/Course-Registration-System)** — a course
registration and management application. Students browse a catalogue and submit applications;
administrators manage courses, instructors and incoming applications from a separate authenticated
area. Persistence is code-first Entity Framework Core over SQLite, with the schema versioned as
migrations rather than hand-edited.

The interesting part was the boundary: keeping the public catalogue and the admin area genuinely
separate instead of hiding buttons in a shared view.

<br>

## Also Building — File Analysis Service

<div align="center">
  <img src="./assets/analysis.svg" alt="File Analysis Service architecture" width="100%" />
</div>

A file analysis pipeline that scans uploads with **YARA** rules, parses PE structure with
`pefile`, and submits samples to a **CAPE** sandbox for behavioural analysis. The work is
queued through **Redis** to a **Celery** worker rather than blocking the request — analysing
an untrusted file is slow, and it has no business happening inside an HTTP handler.

What I took from it: how to keep hostile input away from the request path, why isolation
matters when the thing you are inspecting may be actively malicious, and how to design an
API around work that finishes long after the response does.

*The source stays private while it is still being built.*

<br>

## Focus

| Area | What I'm actually doing about it |
|---|---|
| **API design** | Paginated, validated REST endpoints — `ResturantData` uses Pydantic schemas and page-size limits rather than returning unbounded result sets |
| **Data modelling** | Normalised schemas and code-first migrations; joins at query time instead of duplicated columns |
| **Deployment** | Running services in Docker Compose and putting them on an AWS EC2 host, not just `dotnet run` |
| **Analysis tooling** | Static and dynamic file analysis with YARA rules and Celery workers — the area I find most interesting right now |

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
      <a href="https://github.com/kutsibalci/ResturantData">
        <img src="./assets/card-data.svg" alt="Business Data Panel — FastAPI, SQLAlchemy, Pydantic" width="100%" />
      </a>
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="./assets/card-analysis.svg" alt="File Analysis Service — FastAPI, YARA, Celery, Docker" width="100%" />
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

1. Layered architecture and dependency injection applied to real services, not sample projects
2. SQL query planning — reading execution plans instead of guessing at indexes
3. Data structures and algorithms, properly rather than for exams

<br>

---

<div align="center">

**Get in touch** — <a href="mailto:balcihkutsi@gmail.com">balcihkutsi@gmail.com</a>

<sub>İzmir, Türkiye · open to remote and hybrid roles</sub>

</div>
