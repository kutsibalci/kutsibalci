<div align="center">
  <img src="./assets/hero.svg" alt="Hüseyin Kutsi Balcı — Backend Development Student, Anadolu University, İzmir" width="100%" />
</div>

<br>

I write backend systems and ship them. Usually that means the schema first, then the API, then
the container it runs in.

The project I spend most time on is a pre-accounting and logistics system for a haulage business.
It already has people using it daily, and I am still developing it — right now I am going through
it closing the gaps I found once it met real data. Besides that I build mobile apps in Flutter and
React Native, and I contribute to open source: **eight of my patches are merged** into ROOT, .NET,
systemd, Airflow, VS Code and others.

**Looking for an internship or a junior backend role.**

<br>

## Projects

<table>
  <tr>
    <td width="50%"><a href="https://github.com/kutsibalci/concurrent-ticketing"><img src="./assets/card-ticketing.svg" alt="Concurrent Ticketing" width="100%" /></a></td>
    <td width="50%"><a href="https://github.com/kutsibalci/watch-party-sync-engine"><img src="./assets/card-watchparty.svg" alt="Watch Party Sync Engine" width="100%" /></a></td>
  </tr>
  <tr>
    <td width="50%"><a href="https://github.com/kutsibalci/on-muhasebe-lojistik-takip"><img src="./assets/card-onyuz.svg" alt="Ön Muhasebe ve Lojistik Takibi" width="100%" /></a></td>
    <td width="50%"><a href="https://github.com/kutsibalci/sinav-motoru"><img src="./assets/card-sinav.svg" alt="Sınav Motoru" width="100%" /></a></td>
  </tr>
  <tr>
    <td width="50%"><a href="https://github.com/kutsibalci/kurye-rota"><img src="./assets/card-kurye.svg" alt="Kurye Rota" width="100%" /></a></td>
    <td width="50%"><a href="https://github.com/kutsibalci/Course-Registration-System"><img src="./assets/card-course.svg" alt="Course Registration System" width="100%" /></a></td>
  </tr>
  <tr>
    <td width="50%"><a href="https://github.com/kutsibalci/File-Analysis"><img src="./assets/card-analysis.svg" alt="File Analysis Service" width="100%" /></a></td>
    <td width="50%"><a href="https://github.com/kutsibalci/life-gate"><img src="./assets/card-lifegate.svg" alt="Life Gate" width="100%" /></a></td>
  </tr>
</table>

<sub>Cards marked <b>PRIVATE</b> link to repositories that are not open to visitors. Happy to walk
through any of them in an interview.</sub>

### Also built

| | |
|---|---|
| [Coffee Shop Management](https://github.com/kutsibalci/Small-coffee-Shop-Management-App) | Windows Forms till for a small café. Making the ordering logic testable is how I found that two waiters could open two tabs on one table and only one got billed. |
| [Sefer Defteri](https://github.com/kutsibalci/sefer-defteri) | The driver's side of the accounting system. Expo, on-device SQLite, document expiry reminders. Works with no signal in a truck cab. |
| [Minik Masal](https://github.com/kutsibalci/minik-masal) | Audio story player for small children, with a parent gate. Written twice: Flutter and Expo. |
| [Business Directory API](https://github.com/kutsibalci/business-directory-api) | FastAPI service collecting business listings by province. Paginated endpoints, Alembic migrations, spreadsheet export. |
| [Redmine Deployment](https://github.com/kutsibalci/Redmine-Upgrade) | Dockerised Redmine with PostgreSQL on AWS EC2. Built with [@hzflora](https://github.com/hzflora). |
| [Pansuman Simulator](https://github.com/kutsibalci/PansumanSimulator) | Unity training simulator for wound dressing. In progress. |

<br>

## Open source

Eight patches merged into projects I had no prior connection to. The pattern is the same each time:
I sweep for a class of defect, throw away almost everything the sweep returns, and open a pull
request only for what I can prove.

| Merged | What it was |
|---|---|
| [root#23002](https://github.com/root-project/root/pull/23002) | A TMVA header unreachable since 2015. I traced the commit that orphaned it and checked every symbol it declared was already covered elsewhere. |
| [root#23004](https://github.com/root-project/root/pull/23004) | Two aggregate LinkDef headers that lost their only caller when the build went back to two dictionaries per package. |
| [systemd#43300](https://github.com/systemd/systemd/pull/43300) | Seven man page cross-references pointing at the wrong section. CI went red; I pulled the 5.4 MB log, showed the failure was an unrelated ppc64le timeout, and said so. Merged with the job still red. |
| [airflow#71179](https://github.com/apache/airflow/pull/71179) | Twenty links that 404 for every reader but open fine for every author. The directory is a git symlink blob and GitHub will not traverse one. 372 candidates went in, 20 came out. |
| [vscode-docs#10119](https://github.com/microsoft/vscode-docs/pull/10119) | Three setting IDs whose casing does not match what VS Code registers, so they resolve to settings that do not exist. I checked all 771 IDs behind 1,760 macros. |
| [dotnet/runtime#131865](https://github.com/dotnet/runtime/pull/131865) | Eight documentation links whose targets exist but whose relative paths resolve nowhere. From thirty-nine candidates. |
| [rustc_codegen_gcc#945](https://github.com/rust-lang/rustc_codegen_gcc/pull/945) | Two broken links. The useful part was working out I was in the wrong repository: it is a subtree, so a fix landed upstream would be overwritten on the next sync. |
| [eclipse-score/communication#853](https://github.com/eclipse-score/communication/pull/853) | Four links in the design docs of the BMW/Bosch/Mercedes automotive platform. One image URL was written `hhttp://`, so a diagram had never rendered. |

One more worth mentioning: [root#23036](https://github.com/root-project/root/issues/23036) was a
report, not a patch. Three settings shipped in `system.rootrc` that nothing in ROOT reads. Whether
to delete them or wire them up was a maintainer's call, so I filed both options. The engineer who
wrote that subsystem replied *"Yes, some parameters were not cleaned up"*, opened the fix half an
hour later, and it merged. No commit under my name, which for that kind of finding is the honest
outcome.

<br>

## Stack

<div align="center">
  <img src="./assets/stack.svg" alt="Stack: languages, backend, data and messaging, mobile, real-time and infrastructure" width="100%" />
</div>

<br>

---

<div align="center">

**[balcihkutsi@gmail.com](mailto:balcihkutsi@gmail.com)** · **[LinkedIn](https://www.linkedin.com/in/h%C3%BCseyinkutsibalci/)**

<sub>İzmir, Türkiye · open to remote and hybrid</sub>

</div>
