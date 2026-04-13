# Loopimprove Log: spec-driven-framework

**Feature:** Framework command files (.claude/commands/)
**Branch:** improve/framework-2026-04-13
**MAX_RUNDER:** 3
**Kvalitetsgulv:** Kommandoerne skal være indbyrdes konsistente, præcise og fuldt anvendelige af en ny bruger uden yderligere vejledning.

---

## Runde 1

### Forbedringer
- `plan.md`: argument-input-case, fallback for ukendt feature-type, præciseret rollback-vejledning
- `tasks.md`: argument-input-case, filsti-krav i task-format med konkret eksempel
- `review-spec.md`: fuld disambiguering i Trin 1, test-kommando-fallback, Bash-scope præciseret, tilføjet "Redigér IKKE spec.md"-regel
- `story.md`: spring spørgsmål over hvis rolle+mål allerede er angivet i brugerens besked

### Verificering
Alle faser bevaret. Kommandoer på dansk. Trin-format intakt. ✅ PASS

---

## Runde 2

### Forbedringer
- `polish.md` + `loopimprove.md`: logisk fejl — feature-navn kendes ikke i Trin 0, fil-læsning og log-oprettelse flyttes til Trin 1 efter feature-identifikation
- `spec.md`: Open Questions-sektion dokumenteret eksplicit med regel om hvornår den udelades
- `plan.md`: Spec-reference-felt nævnes nu i Trin 3 så det ikke forbliver placeholder
- `tasks-template.md`: Done-header og task-eksempel synkroniseret med tasks.md-kommandoen

### Verificering
Alle faser bevaret. Kvalitetsgulv intakt. ✅ PASS

---

## Runde 3

### Forbedringer
- `loopimprove.md`: fix operationel fejl i Trin 2d — regression opdages i 2b (før commit i 2c), så `git revert HEAD` ville fortryde forrige rundes commit; rettet til `git checkout -- <filer>`
- `tasks.md`: tilføjet commit-instruktion i Trin 4's implementeringsvejledning

### Verificering
Alle faser bevaret. Kvalitetsgulv intakt. ✅ PASS

---

## Slutrapport

**Runder kørt:** 3 af 3
**Branch:** improve/framework-2026-04-13

### Hvad blev forbedret
- **Runde 1:** Guard conditions og disambiguation på tværs af 4 kommandoer — agenten klarede nu edge cases der før ville resultere i tavs fejl
- **Runde 2:** Logisk fejl i polish + loopimprove (feature-navn bruges før det kendes), konsistens med templates og spec-sektioner
- **Runde 3:** Operationel bugfix i regression-håndtering, synlighed af commit-regel for brugeren

### Næste trin — HUMAN GATE (påkrævet)

1. Review ændringerne: `git diff main...improve/framework-2026-04-13`
2. Kør `/review-spec` på branchen hvis relevant
3. Opret PR: `gh pr create --base main --head improve/framework-2026-04-13`
4. Merge KUN efter gennemgang og godkendelse
