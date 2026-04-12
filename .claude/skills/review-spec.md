---
name: review-spec
description: Verificerer at implementeringen opfylder acceptance criteria i spec.md — giver pass/fail per kriterie
user-invocable: true
allowed-tools: Read, Grep, Bash
---

# /review-spec — Spec Compliance Review

Du er i **Fase 5: Review**. Du må ikke redigere kode i denne fase.

## Trin 1 — Identificér feature

Find den feature der skal reviewes:
- Kig i `.specs/` for feature-mapper med både spec.md og tasks.md
- Hvis tasks.md eksisterer men mangler ✓ på alle tasks: advar brugeren om at implementeringen muligvis er ufærdig

## Trin 2 — Læs spec.md acceptance criteria

Læs `.specs/<feature-name>/spec.md`, sektion **Acceptance Criteria**.

List hvert kriterie separat — de er din tjekliste.

## Trin 3 — Verificér hvert kriterie

For hvert acceptance criteria:

1. Find den relevante kode med `Grep` og `Read`
2. Kør relevante test-kommandoer fra plan.md med `Bash` (read-only operationer)
3. Afgør: **PASS** eller **FAIL**

Ved FAIL: Beskriv præcist hvad der mangler eller er forkert — ikke generiske anbefalinger.

## Trin 4 — Rapportér resultat

Output-format:

```
## Spec Review: <feature-name>

| Kriterie | Status | Note |
|----------|--------|------|
| [Kriterie 1 tekst] | ✅ PASS | |
| [Kriterie 2 tekst] | ❌ FAIL | [Præcis beskrivelse af hvad mangler] |
| [Kriterie 3 tekst] | ✅ PASS | |

**Samlet:** X/Y kriterier bestået

[Hvis FAIL på nogle:]
Næste trin: Ret [specifik ting] i [specifik fil/funktion] og kør /review-spec igen.
```

## Regler

- Redigér IKKE kode — selv hvis du ser en åbenlys fix
- Kør kun read-only Bash kommandoer (tests, linting, grep)
- Vær specifik ved FAIL — "det virker ikke" er ikke nok
- Acceptér ikke "det ser rigtigt ud" — verificér med faktiske kommandoer eller kode-check
