# Guide til Undervisere

## Hvad er dette framework?

Dette er et GitHub template repository designet til at lære datamatikerelever struktureret AI-assisteret udvikling. Det erstatter ikke undervisning i programmering — det giver eleverne et stillads der tvinger dem til at tænke, inden de lader AI'en skrive kode.

Frameworket er inspireret af GitHub Spec-Kit og JetBrains' Junie-tilgang til spec-driven development.

---

## Det pædagogiske rationale

### Problemet med "vibe coding"

Elever der bruger AI-assistenter uden struktur oplever typisk:
- De kan ikke forklare hvad koden gør
- De debugger AI-genereret kode de ikke forstår
- De mister overblikket når projektet vokser
- De lærer AI-brug, men ikke softwareudvikling

### Hvad spec-driven development træner

| Kompetence | Hvordan frameworket adresserer den |
|------------|----------------------------------|
| Kravspecifikation | `/spec` kræver at eleven definerer acceptance criteria |
| Teknisk planlægning | `/plan` kræver at eleven godkender filreferencer og trin |
| Opgavedekomponering | `/tasks` synliggør hvad "atomisk" betyder i praksis |
| Kvalitetssikring | `/review-spec` giver konkret pass/fail mod egne krav |
| Git-disciplin | Hooken og task-strukturen fremmer hyppige, meningsfulde commits |

### Determinisme via proces

Frameworket opnår ikke deterministisk AI-output (det er umuligt med LLMs), men det opnår:
- **Proces-determinisme:** Eleven kan ikke skippe fra idé til kode
- **Scope-determinisme:** Agentens tools er begrænset per fase
- **Verifikations-determinisme:** Review-fasen giver binære pass/fail-resultater

---

## Brug i undervisningen

### Projektstart

Bed eleverne udfylde `CONSTITUTION.md` i grupper inden de skriver en linje kode. Det er en god diskussionsøvelse: *Hvilken tech stack? Hvilke kodestandarder? Hvad er vores non-negotiables?*

### Spec som aflevering

`spec.md` er et naturligt afleverings- og feedbackpunkt. En god spec viser at eleven forstår kravene — inden implementeringen starter. Vurdér:
- Er acceptance criteria verificerbare (ja/nej-svar)?
- Er scope eksplicit (hvad er valgt fra)?
- Stemmer constraints overens med projektets begrænsninger?

### Plan som forståelsescheck

`plan.md` viser om eleven forstår sin egen kodebase. Spørg: "Forklar trin 3 i din plan." Hvis de ikke kan, har de godkendt en plan de ikke forstod.

### Review-rapport som eksamensgrundlag

`/review-spec`-outputtet giver en konkret pass/fail-rapport mod elevens egne krav. Det er et stærkt grundlag for mundtlig evaluering: "Dit kriterie 2 fik FAIL — hvad gik galt, og hvad ville du gøre anderledes?"

---

## Differentiering

**Begynderelever:** Fokusér på spec- og review-faserne. Lad dem skrive spec og bruge `/review-spec` til at tjekke færdig kode — selv hvis de ikke bruger `/plan` og `/tasks` endnu.

**Øvede elever:** Giv dem ejerskab over hele flowet inkl. CONSTITUTION.md. Udfordring: skriv en spec der er god nok til at en anden elev kan implementere den.

**Udvidelse:** Bed eleverne kritisere hinandens specs — hvad er vagt? Hvad mangler i scope? Det er en god peer review-øvelse der ikke kræver at de forstår hinandens kode.

---

## Hvad frameworket ikke gør

- Det garanterer ikke god kode — det garanterer et struktureret proces
- Det erstatter ikke kode-review af selve implementeringen
- Det fungerer bedst med Claude Code i terminalen — andre AI-assistenter kan følge workflow-dokumenterne manuelt, men skills og hooks virker ikke automatisk
