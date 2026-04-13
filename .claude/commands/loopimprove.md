# /loopimprove — Autonom Kvalitetsforbedring med Friske Øjne

Du er i **Fase 7: Loop Improve**. Din opgave er at forbedre implementeringens kvalitet over N runder, hvor hvert runde starter med friske øjne — ingen akkumuleret bias fra tidligere runder. Ændringerne lander på en ny git branch. En human skal godkende via PR inden merge.

## Trin 0 — Forberedelse

Læs `CONSTITUTION.md` — projektstandarder og non-negotiables.

Sæt `MAX_RUNDER` til argumentet brugeren angav (f.eks. `/loopimprove 5`). Hvis intet argument: brug **3**.

## Trin 1 — Identificér scope

Find feature-mappen i `.specs/`:
- Hvis brugeren angav et feature-navn som argument (f.eks. `/loopimprove 3 add-contact-form`): brug det
- Hvis kun én feature-mappe eksisterer med spec.md, plan.md og tasks.md: brug den
- Hvis flere feature-mapper har alle tre filer: bed brugeren angive feature-navn

Verificér at disse filer eksisterer:
- `.specs/<feature-name>/plan.md` — hvis ikke: stop og bed brugeren om at køre `/plan`, `/tasks` og fuldføre implementeringen først
- `.specs/<feature-name>/tasks.md` — hvis ikke: stop og bed brugeren om at køre `/tasks` og fuldføre implementeringen først

Læs:
- `.specs/<feature-name>/spec.md` — acceptance criteria (din absolutte kvalitetsgulv — de må IKKE brydes)
- `.specs/<feature-name>/plan.md` — hvad der blev bygget og hvorfor
- `.specs/<feature-name>/tasks.md` — hvad der blev implementeret

Notér de primære implementeringsfiler fra `plan.md` → **Files to Change** — disse er dit primære scope. Du MÅ kigge bredere i codebasen, men fokusér her.

Opret `.specs/<feature-name>/loopimprove-log.md`.

Opret ny git branch — tjek først om den allerede eksisterer:
```bash
# Tjek om branchen eksisterer
git branch --list improve/<feature-name>-<YYYY-MM-DD>
# Hvis output er tomt: opret den
git checkout -b improve/<feature-name>-<YYYY-MM-DD>
# Hvis den allerede eksisterer: tilføj -v2, -v3 osv. indtil et unikt navn findes
```
Eksempel: `improve/add-contact-form-2026-04-13` eller `improve/add-contact-form-2026-04-13-v2`

Meld til brugeren: "Starter /loopimprove på branch `improve/<feature-name>-<dato>` — [N] runder planlagt."

## Trin 2 — Forbedringsløkke

Gentag for `RUNDE = 1` til `MAX_RUNDER`:

### 2a — Spawn sub-agent med friske øjne

Brug **Agent tool** til at starte en ny sub-agent for denne runde. Sub-agenten starter uden akkumuleret kontekst fra tidligere runder — det er essensen af "friske øjne".

Giv sub-agenten dette brief (tilpas til den konkrete feature):

```
Du er en erfaren senior-udvikler der reviewer denne implementering med helt friske øjne.

LÆS DISSE FILER FRA BUNDEN:
- CONSTITUTION.md
- .specs/<feature-name>/spec.md (acceptance criteria — disse MÅ IKKE brydes)
- .specs/<feature-name>/plan.md (hvad der blev bygget)
- Alle implementeringsfiler nævnt i plan.md under "Files to Change"
- Evt. relaterede filer du finder via Grep

HVAD DU SKAL GØRE:
Find og implementér de mest værdifulde forbedringer du kan. Tænk på:
- Navngivning: er funktioner, variabler og klasser præcise og selvforklarende?
- Struktur: er koden organiseret klart? Er der unødvendig kompleksitet?
- Fejlhåndtering: er edge cases og fejltilstande håndteret robust?
- Patterns: bruges idiomatiske patterns for dette sprog/framework?
- Duplikering: er der gentaget logik der kan abstraherast?
- Performance: er der åbenlyse ineffektiviteter?
- Læsbarhed: vil en ny udvikler forstå koden hurtigt?

TIDLIGERE RUNDER HAR ÆNDRET:
[liste over hvad tidligere runder ændrede — tom ved runde 1]

REGLER:
- Du MÅ IKKE bryde acceptance criteria fra spec.md
- Begrund HVER ændring kortfattet
- Foretag reelle ændringer — ret ikke noget du ikke har et godt argument for
- Rapportér præcist hvilke filer du ændrede og hvad du ændrede

Returner:
1. Liste over ændrede filer med beskrivelse af ændringen og begrundelse
2. Bekræftelse af at du har verificeret at acceptance criteria stadig er opfyldt
```

### 2b — Modtag og evaluer sub-agentens output

Gennemgå hvad sub-agenten returnerede:

- **Ingen ændringer:** Sub-agenten fandt intet at forbedre → stop løkken tidligt (se 2d)
- **Ændringer:** Fortsæt til 2c
- **Regression opdaget:** Sub-agenten melder at et acceptance criteria er brudt → fortryd sub-agentens ændringer og stop (se 2d)

### 2c — Commit rundens ændringer

```bash
git add <præcise-filstier>   # aldrig git add -A
git commit -m "loopimprove: runde [N] — [kort summary af forbedringer]"
```

Tilføj til `loopimprove-log.md`:

```
## Runde [N]

### Forbedringer
- `[fil]`: [hvad og hvorfor]
- `[fil]`: [hvad og hvorfor]

### Verificering
Alle acceptance criteria: ✅ PASS
(eller: ❌ Regression i kriterie "[tekst]" — ændringer fortrudt)
```

Opdater listen over "hvad tidligere runder ændrede" til brug i næste rundes sub-agent brief.

### 2d — Afbrydelseslogik

Stop løkken tidligt hvis:
- **Sub-agenten fandt intet at forbedre:** Koden er allerede optimal — stop
- **Regression opdaget:** Kør `git revert HEAD` for at fortryde rundens commit, stop med advarsel til brugeren
- **MAX_RUNDER er nået:** Stop normalt

## Trin 3 — Afslutning

Efter MAX_RUNDER (eller tidlig stop):

### 3a — Push branch

```bash
git push -u origin improve/<feature-name>-<dato>
```

### 3b — Slutrapport til brugeren

```
## Loopimprove færdig: <feature-name>

**Runder kørt:** [N] af [MAX_RUNDER]
**Branch:** improve/<feature-name>-<dato>

### Hvad blev forbedret
Runde 1: [summary]
Runde 2: [summary]
...

### Næste trin — HUMAN GATE (påkrævet)

1. Review ændringerne:
   git diff main...improve/<feature-name>-<dato>

2. Kør /review-spec på denne branch for at verificere at criteria stadig er opfyldt

3. Opret en Pull Request til main:
   gh pr create --base main --head improve/<feature-name>-<dato>

4. Merge KUN efter gennemgang og godkendelse — aldrig direkte push til main
```

Gem slutrapporten som den afsluttende sektion i `loopimprove-log.md`.

**Opret IKKE PR automatisk. Merge IKKE til main. Brugeren skal gøre dette manuelt.**

## Regler

- Opret ALTID ny branch — commit aldrig direkte til main, master eller feature branch
- Brug **Agent tool** til hvert improvement runde — dette sikrer friske øjne (ingen kontekst-bias fra tidligere runder)
- Sub-agenten må læse hele codebasen, men skal begrunde alle ændringer
- Regression i acceptance criteria → `git revert HEAD` og stop
- Aldrig `git add -A` — brug præcise filstier
- Aldrig auto-merge og aldrig auto-opret PR — human gate er non-negotiable
- Stop tidligt hvis sub-agenten ikke finder noget at forbedre — tvang ikke tomme runder igennem
- `/loopimprove` er ikke en erstatning for `/review-spec` — kør `/review-spec` på improve-branchen inden PR-merge
