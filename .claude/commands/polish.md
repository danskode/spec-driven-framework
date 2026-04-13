# /polish — Autonom Forbedringsløkke

Du er i **Fase 6: Polish**. Din opgave er at rette fejlende acceptance criteria automatisk og gentage indtil alle kriterier er bestået eller det maksimale antal iterationer er nået.

## Trin 0 — Forberedelse

Læs `CONSTITUTION.md` — projektstandarder og non-negotiables.

Sæt `MAX_ITERATIONER` til argumentet brugeren angav (f.eks. `/polish 5`). Hvis intet argument: brug **3**.

`/polish` er kun til post-implementerings-rettelse — brug den ikke som erstatning for implementeringsfasen.

## Trin 1 — Identificér feature og læs kriterier

Find feature-mappen i `.specs/`:
- Hvis brugeren angav et feature-navn som argument (f.eks. `/polish 3 add-contact-form`): brug det
- Hvis kun én feature-mappe eksisterer med spec.md, plan.md og tasks.md: brug den
- Hvis flere feature-mapper har alle tre filer: bed brugeren angive feature-navn

Verificér at disse filer eksisterer:
- `.specs/<feature-name>/plan.md` — hvis ikke: stop og bed brugeren om at køre `/plan` og `/tasks` først
- `.specs/<feature-name>/tasks.md` — hvis ikke: stop og bed brugeren om at køre `/tasks` og fuldføre implementeringen først

Læs:
- `.specs/<feature-name>/spec.md` — acceptance criteria (din målestok)
- `.specs/<feature-name>/plan.md` — godkendt teknisk plan (filer du må redigere)

Opbyg en intern liste over alle kriterier med status `UKENDT`.

Opret eller åbn `.specs/<feature-name>/polish-log.md` til at logge hvad der ændres per iteration.

## Trin 2 — Startbaseline (Iteration 0)

Kør en fuld review-gennemgang af alle kriterier — identisk med hvad `/review-spec` gør:

For hvert kriterie:
1. Find relevant kode med `Grep` og `Read`
2. Kør relevante test-kommandoer fra `plan.md` med `Bash` (de kommandoer der er markeret under **Test Commands**)
3. Sæt status: **PASS** eller **FAIL** med præcis beskrivelse af hvad der mangler ved FAIL

Skriv resultatet til `polish-log.md`:

```
## Iteration 0 — Startbaseline

| Kriterie | Status | Note |
|----------|--------|------|
| [tekst] | ✅ PASS | |
| [tekst] | ❌ FAIL | [præcis beskrivelse] |

Bestået: X/Y
```

Hvis alle kriterier er PASS allerede: rapportér det og stop. Ingen rettelser nødvendige.

## Trin 3 — Forbedringsløkke

Gentag følgende blok for `ITERATION = 1` til `MAX_ITERATIONER`:

### 3a — Ret alle fejlende kriterier

For hvert kriterie med status FAIL fra forrige iteration:

1. **Analysér årsagen** præcist — ikke generisk. Hvad mangler konkret?
2. **Find den kode der skal rettes** via `Grep` og `Read`. Brug kun filer nævnt i `plan.md` under **Files to Change** — ret ikke filer udenfor planens scope uden at notere det eksplicit i loggen.
3. **Foretag den mindst mulige ændring** der retter netop dette kriterie. Undgå at omskrive kode der allerede passer.
4. **Notér ændringen** i en intern liste: `[fil] → [hvad der blev ændret]`

Hvis et FAIL-kriterie ikke kan rettes (f.eks. mangler ekstern service, kræver ny dependency, eller er udenfor planens scope): markér det som `BLOKKERET` og notér årsagen. Improvisér ikke løsninger udenfor plan.md.

### 3b — Kør fuld review igen

Når alle FAIL-kriterier er behandlet (rettet eller markeret BLOKKERET):

Kør den samme fulde gennemgang som i Trin 2 for **alle** kriterier — ikke kun dem der var FAIL. Dette fanger regressions.

### 3c — Log iteration

Tilføj til `polish-log.md`:

```
## Iteration [N]

### Rettelser foretaget
- `[fil]`: [beskrivelse af ændring] → rettede kriterie "[tekst]"
- (ingen rettelser) hvis intet blev ændret

### Resultater
| Kriterie | Status | Note |
|----------|--------|------|
| [tekst] | ✅ PASS | |
| [tekst] | ❌ FAIL | [præcis beskrivelse] |
| [tekst] | 🚫 BLOKKERET | [årsag] |

Bestået: X/Y
```

### 3d — Commit hvis der blev lavet rettelser

Hvis der blev foretaget mindst én rettelse i denne iteration:

```bash
git add <præcise-filstier>   # aldrig git add -A
git commit -m "polish: iteration [N] — [kort liste over hvad der blev rettet]"
```

Brug kun filstier fra planens **Files to Change** i `git add`.

### 3e — Afbrydelseslogik

Stop løkken tidligt hvis:
- **Alle kriterier er PASS:** Stop nu — gå til Trin 4
- **Nul rettelser blev foretaget OG der stadig er FAIL:** Agenten er kørt fast — stop og gå til Trin 4 med advarsel
- **Alle resterende FAIL er BLOKKERET:** Yderligere iterationer hjælper ikke — stop

Ellers: fortsæt til næste iteration.

## Trin 4 — Slutrapport

Udskriv den samlede slutrapport til brugeren:

```
## Polish-rapport: <feature-name>

**Iterationer kørt:** [N] af [MAX_ITERATIONER]

### Endelig status
| Kriterie | Status | Rettet i iteration |
|----------|--------|--------------------|
| [tekst] | ✅ PASS | (var allerede OK / iteration 1 / ...) |
| [tekst] | ✅ PASS | iteration 2 |
| [tekst] | ❌ FAIL | — |
| [tekst] | 🚫 BLOKKERET | [årsag] |

**Samlet: X/Y kriterier bestået**
```

Hvis alle PASS:
> Alle kriterier bestået. Kør `/review-spec` for endelig verifikation.

Hvis FAIL eller BLOKKERET:
> Følgende kriterier kræver manuel indgriben: [præcis beskrivelse af hvad mangler og hvorfor agenten ikke kunne rette det]
> Næste trin: ret disse manuelt og kør `/polish` eller `/review-spec` igen.

Gem slutrapporten som den afsluttende sektion i `polish-log.md`.

## Regler

- Læs altid `CONSTITUTION.md` inden du retter kode
- Ret KUN filer der er nævnt i `plan.md` under **Files to Change** — notér og begrund undtagelser i loggen
- Foretag mindst mulige ændringer — reparér, omskriv ikke
- Aldrig `git add -A` — brug præcise filstier
- Improvisér ikke nye tekniske løsninger udenfor plan.md — markér dem BLOKKERET
- Stop altid ved `MAX_ITERATIONER` selv hvis kriterier stadig fejler
- `/polish` erstatter ikke `/review-spec` — kør `/review-spec` efter `/polish` for endelig, uafhængig verifikation
