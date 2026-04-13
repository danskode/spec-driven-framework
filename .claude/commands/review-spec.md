# /review-spec — Spec Compliance Review

Du er i **Fase 5: Review**. Du må ikke redigere kode i denne fase.

## Trin 1 — Identificér feature

Find den feature der skal reviewes:
- Hvis brugeren angav et feature-navn som argument (f.eks. `/review-spec add-contact-form`): brug det
- Hvis kun én feature-mappe eksisterer med spec.md: brug den
- Hvis flere feature-mapper eksisterer med spec.md: spørg brugeren hvilken feature der reviewes
- Hvis ingen spec.md eksisterer: stop og fortæl brugeren at der ikke er noget at reviewe — kør `/spec` først

Tjek tasks.md:
- Hvis tasks.md eksisterer men mangler ✓ på alle tasks: advar brugeren om at implementeringen muligvis er ufærdig, og spørg om review skal fortsætte alligevel

## Trin 2 — Læs spec.md acceptance criteria

Læs `.specs/<feature-name>/spec.md`, sektion **Acceptance Criteria**.

List hvert kriterie separat — de er din tjekliste.

## Trin 3 — Verificér hvert kriterie

For hvert acceptance criteria:

1. Find den relevante kode med `Grep` og `Read`
2. Kør test-kommandoer fra plan.md's **Test Commands** med `Bash` — brug kun de kommandoer der er listet der. Hvis plan.md ikke har test-kommandoer: verificér via kode-check alene (Grep + Read) og noter at automatisk verifikation ikke var mulig.
3. Afgør: **PASS** eller **FAIL**

Ved FAIL: Beskriv præcist hvad der mangler eller er forkert — ikke generiske anbefalinger.

**Vigtigt:** Kør kun test-kommandoer der er specificeret i plan.md. Opfind ikke nye testkommandoer — du kan ikke vide om de er destruktive (f.eks. sletter data, ændrer DB-state).

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
- Kør KUN Bash-kommandoer der er eksplicit listet i plan.md's Test Commands — opfind ikke nye kommandoer
- Vær specifik ved FAIL — "det virker ikke" er ikke nok
- Acceptér ikke "det ser rigtigt ud" — verificér med faktiske kommandoer eller kode-check
- Redigér IKKE spec.md — kriterier er fastsat, ikke til forhandling under review
