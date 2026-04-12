---
name: spec
description: Start her. Genererer en struktureret spec fra din feature-beskrivelse og gemmer den i .specs/<feature-name>/spec.md
user-invocable: true
allowed-tools: Read, Write, AskUserQuestion
---

# /spec — Generer Feature Specification

Du er i **Fase 1: Specify**. Din opgave er at hjælpe brugeren med at skrive en klar, verificerbar spec — inden nogen kode skrives.

## Trin 1 — Læs projektkontekst

Læs disse filer inden du begynder:
- `CONSTITUTION.md` — projektstandarder og constraints
- Eksisterende `.specs/` mapper — for at forstå konventioner og scope

Hvis `CONSTITUTION.md` ikke er udfyldt, stop og bed brugeren om at udfylde den først.

## Trin 2 — Forstå intent

Brugeren har beskrevet hvad de vil bygge. Inden du skriver spec.md:

1. Identificér uklarheder og edge cases i beskrivelsen
2. Stil maksimalt 3 afklarende spørgsmål — de vigtigste først
3. Vent på svar inden du fortsætter

Gode spørgsmål at stille:
- "Hvad sker der hvis [edge case]?"
- "Er [tilstødende funktionalitet] inkluderet eller ekskluderet?"
- "Hvem er den primære bruger af denne feature?"

## Trin 3 — Opret spec.md

Generér et feature-navn i kebab-case fra brugerens beskrivelse.

Opret filen: `.specs/<feature-name>/spec.md`

Brug skabelonen fra `.specs/_templates/spec-template.md`. Udfyld alle sektioner:
- **Intent:** Skriv i stakeholder-sprog. Ingen implementeringsdetaljer.
- **Acceptance Criteria:** Minimum 3 verificerbare kriterier. Hvert punkt kan besvares ja/nej.
- **Scope:** Vær eksplicit om hvad der er ude af scope.
- **Constraints:** Henvis til CONSTITUTION.md for relevante begrænsninger.

## Trin 4 — Præsenter og afvent godkendelse

Vis spec.md indholdet til brugeren og sig:

> "Spec gemt i `.specs/<feature-name>/spec.md`. Gennemgå den og ret hvis nødvendigt.
> Kør `/plan` når du er klar til næste fase."

## Regler

- Skriv IKKE kode eller implementeringsforslag
- Foreslå IKKE teknisk approach — det er plan-fasen
- Artefakt-sprog: dansk eller engelsk, konsistent med eksisterende specs
