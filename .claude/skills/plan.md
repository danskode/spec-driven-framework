---
name: plan
description: Oversætter en godkendt spec til en præcis teknisk implementeringsplan
user-invocable: true
allowed-tools: Read, Write, Glob, Grep
---

# /plan — Generer Teknisk Plan

Du er i **Fase 2: Plan**. Du må ikke redigere kode-filer i denne fase.

## Trin 1 — Find og læs spec.md

Find den seneste eller mest relevante spec i `.specs/`:
- Hvis kun én feature-mappe eksisterer uden plan.md, brug den
- Hvis flere: spørg brugeren hvilken feature der planlægges

Læs `.specs/<feature-name>/spec.md` grundigt, især:
- Acceptance Criteria — disse er din målestok
- Constraints — disse er dine begrænsninger
- Scope — hvad er eksplicit ekskluderet

## Trin 2 — Research kodebasen

Forstå systemet inden du planlægger. Brug `Glob` og `Grep` til at:
- Find relevante eksisterende filer og funktioner
- Forstå dataflow og afhængigheder
- Identificér edge cases i eksisterende kode

Mål: Planen skal referere til præcise filer og funktioner, ikke hypotetiske.

## Trin 3 — Skriv plan.md

Opret: `.specs/<feature-name>/plan.md`

Brug `.specs/_templates/plan-template.md`. Krav til planen:
- **Approach:** Forklar valget af teknisk strategi + hvad der blev fravalgt
- **Files to Change:** Præcise stier — ingen "se relevante filer"
- **Steps:** Nummererede, atomiske, verificerbare. Hvert trin skal kunne gennemføres og tjekkes uafhængigt. Skriv dem så klart at enhver model kan følge dem.
- **Test Commands:** Kørbare kommandoer — ikke "kør dine tests"
- **Rollback:** Specifik for de risikable trin

## Trin 4 — Præsenter og afvent godkendelse

> "Plan gemt i `.specs/<feature-name>/plan.md`. Gennemgå trin og filreferencer.
> Kør `/tasks` når du er tilfreds med planen."

## Regler

- Redigér IKKE kode-filer
- Henvis til FAKTISKE filer i kodebasen — brug Glob/Grep til at verificere de eksisterer
- Planen skal kunne følges af en agent der ikke har set din research
