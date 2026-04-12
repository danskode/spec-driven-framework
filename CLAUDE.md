# Spec-Driven Development Framework — Agent Instructions

Du er en AI-assistent i et spec-driven development workflow. Disse regler gælder altid.

---

## Fase-model

Arbejdet er organiseret i fire obligatoriske faser. Du må ikke springe faser over.

```
/spec  →  [human review]  →  /plan  →  [human review]  →  /tasks  →  implement  →  /review-spec
```

**Regel:** Du må ikke redigere kode-filer, hvis der ikke eksisterer en godkendt `plan.md` i `.specs/<feature-name>/`. Spørg brugeren om at køre `/plan` først.

---

## Fase 1 — Specify (`/spec`)

**Formål:** Forstå intent og definere acceptance criteria.
**Tilladte handlinger:** Læs filer, stil afklarende spørgsmål, skriv spec-dokument.
**Forbudt:** Redigér kode, foreslå implementering, opret nye filer udover spec.md.

Artefakt: `.specs/<feature-name>/spec.md`

---

## Fase 2 — Plan (`/plan`)

**Formål:** Oversæt spec til præcis teknisk plan.
**Tilladte handlinger:** Læs kodebase (research), skriv plan-dokument.
**Forbudt:** Redigér kode-filer.

Artefakt: `.specs/<feature-name>/plan.md`

---

## Fase 3 — Tasks (`/tasks`)

**Formål:** Nedbryd plan til atomiske, verificerbare tasks.
**Tilladte handlinger:** Læs plan.md, skriv tasks-dokument.
**Forbudt:** Redigér kode-filer.

Artefakt: `.specs/<feature-name>/tasks.md`

---

## Fase 4 — Implement

**Formål:** Eksekvér tasks én ad gangen.
**Tilladte handlinger:** Alt — men følg plan.md præcist. Improvisér ikke.
**Vigtigt:** Commit efter hvert task. Hold kontekst lav (frisk session per task ved komplekse features).

---

## Fase 5 — Review (`/review-spec`)

**Formål:** Verificér at implementeringen opfylder spec.md acceptance criteria.
**Tilladte handlinger:** Læs filer, kør tests, rapportér pass/fail.
**Forbudt:** Redigér kode (kør review-spec igen efter eventuelle rettelser).

---

## Artefakt-lokationer

Alle spec-artefakter gemmes i `.specs/<feature-name>/`:
```
.specs/
└── <feature-name>/
    ├── spec.md    ← opret med /spec
    ├── plan.md    ← opret med /plan
    └── tasks.md   ← opret med /tasks
```

Navngivning: kebab-case, beskrivende (`add-user-login`, `fix-cart-total`, `refactor-auth`).

---

## CONSTITUTION.md

Læs altid `CONSTITUTION.md` inden du begynder. Den definerer dette projekts:
- Tekniske stack og begrænsninger
- Kodestandarder
- Non-negotiables

Hvis CONSTITUTION.md ikke er udfyldt, bed brugeren om at udfylde den inden du fortsætter.

---

## Generelle regler

- Stil afklarende spørgsmål frem for at antage — især i spec-fasen.
- Skriv artefakter i stakeholder-sprog (spec) og præcist teknisk sprog (plan/tasks).
- Nævn eksplicit hvis du opdager at en opgave er udenfor scope af den aktuelle spec.
- Commit hyppigt med beskrivende commit-beskeder der refererer til spec-navnet.
