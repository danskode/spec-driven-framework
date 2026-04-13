# /plan — Generer Teknisk Plan

Du er i **Fase 2: Plan**. Du må ikke redigere kode-filer i denne fase.

## Trin 1 — Find og læs spec.md

Find den relevante spec i `.specs/`:
- Hvis brugeren angav et feature-navn som argument (f.eks. `/plan add-contact-form`): brug det
- Hvis kun én feature-mappe eksisterer uden plan.md: brug den
- Hvis flere feature-mapper eksisterer uden plan.md: spørg brugeren hvilken feature der planlægges
- Hvis ingen spec.md eksisterer i nogen mappe: stop og bed brugeren om at køre `/spec` først

Læs `.specs/<feature-name>/spec.md` grundigt, især:
- Acceptance Criteria — disse er din målestok
- Constraints — disse er dine begrænsninger
- Scope — hvad er eksplicit ekskluderet

## Trin 1b — Detektér feature-type og læs mønster

Identificér feature-typen ud fra spec.md (Intent + Acceptance Criteria):

| Type | Signal i spec |
|------|--------------|
| **crud** | Opret, læs, rediger, slet, liste, ressource |
| **auth** | Login, logout, signup, session, password, token |
| **rest-api** | Endpoint, HTTP, request, response, API |
| **ui** | Visning, side, formular, komponent, render |
| **background** | Job, kron, asynkron, kø, email-afsendelse, batch |

Læs det tilsvarende mønsterdokument:
- `.specs/_templates/patterns/crud.md`
- `.specs/_templates/patterns/auth.md`
- `.specs/_templates/patterns/rest-api.md`
- `.specs/_templates/patterns/ui.md`
- `.specs/_templates/patterns/background.md`

Brug mønsteret som tjekliste når du skriver plan.md — verificér at planen dækker de relevante punkter fra mønsteret. Hvis featuren spænder over flere typer, læs begge relevante mønsterfiler. Hvis feature-typen ikke passer i nogen kategori: spring mønster-læsningen over og notér det i plan.md under Approach.

## Trin 2 — Research kodebasen

Forstå systemet inden du planlægger. Brug `Glob` og `Grep` til at:
- Find relevante eksisterende filer og funktioner
- Forstå dataflow og afhængigheder
- Identificér edge cases i eksisterende kode

Mål: Planen skal referere til præcise filer og funktioner, ikke hypotetiske.

## Trin 3 — Skriv plan.md

Opret: `.specs/<feature-name>/plan.md`

Brug `.specs/_templates/plan-template.md`. Udfyld `Spec-reference`-feltet øverst med den korrekte sti (`.specs/<feature-name>/spec.md`). Krav til planen:
- **Approach:** Forklar valget af teknisk strategi + hvad der blev fravalgt
- **Files to Change:** Præcise stier — ingen "se relevante filer"
- **Steps:** Nummererede, atomiske, verificerbare. Hvert trin skal kunne gennemføres og tjekkes uafhængigt. Skriv dem så klart at enhver model kan følge dem.
- **Test Commands:** Kørbare kommandoer — ikke "kør dine tests"
- **Rollback:** Beskriv konkrete git-kommandoer eller manuelle trin for trin der er destruktive eller svære at fortryde (f.eks. DB-migrationer, blueprint-registreringer, ændringer i eksisterende filer). Nye filer er lav-risiko — fokus på trin der modificerer eksisterende kode.

## Trin 4 — Præsenter og afvent godkendelse

> "Plan gemt i `.specs/<feature-name>/plan.md`. Gennemgå trin og filreferencer.
> Kør `/tasks` når du er tilfreds med planen."

## Regler

- Redigér IKKE kode-filer
- Henvis til FAKTISKE filer i kodebasen — brug Glob/Grep til at verificere de eksisterer
- Planen skal kunne følges af en agent der ikke har set din research
