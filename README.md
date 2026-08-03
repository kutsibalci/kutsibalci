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

## Project Register

| Project | Stack | Notes |
|---|---|---|
| [Course Registration System](https://github.com/kutsibalci/Course-Registration-System) | ASP.NET Core MVC · EF Core · SQLite | Public catalogue + authenticated admin area |
| [Business Data Panel](https://github.com/kutsibalci/ResturantData) | FastAPI · SQLAlchemy · Pydantic | Paginated REST API, cascading filters, Excel export |
| [Coffee Shop Management](https://github.com/kutsibalci/Small-coffee-Shop-Management-App) | C# · Windows Forms · MySQL | Point-of-sale till and manager panel |
| [Redmine Deployment](https://github.com/kutsibalci/Redmine-Upgrade) | Docker Compose · PostgreSQL · AWS EC2 | Self-hosted stack, built with [Atakan MERGEN](https://github.com/) |
| File Analysis *(private)* | FastAPI · YARA · Celery · Docker | Static and dynamic analysis service — releasing once finished |
| Pansuman Simulator *(private)* | Unity · URP · C# | Wound-dressing training simulator, in progress |

Several of these started as team projects — the Redmine deployment was built with
**Atakan MERGEN**, and I contribute to repositories owned by
[@hzflora](https://github.com/hzflora), including the data-collection work that became
`ResturantData`. Reviewing someone else's schema, and having mine reviewed, taught me more
than any tutorial did.

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
