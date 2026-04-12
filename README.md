# Spec-Driven Development Framework

Et GitHub template repository der hjælper dig med at gå fra vibe coding til struktureret, AI-assisteret udvikling med Claude Code.

**Idé:** Du skriver specifikationen. Agenten følger den. Du reviewér hvert trin.

---

## Kom i gang (5 trin)

### 1. Brug dette repo som template

Klik **"Use this template"** øverst på GitHub-siden og opret dit eget repo.

### 2. Klon og aktivér git hook

**Vigtigt:** Kør alle kommandoer inde i projektmappen.

```bash
git clone https://github.com/<dit-brugernavn>/<dit-projekt>.git
cd <dit-projekt>
git config core.hooksPath .githooks
chmod +x .githooks/pre-commit
```

Hooken advarer dig hvis du committer uden en spec. Den blokerer ikke.

### 3. Udfyld CONSTITUTION.md

Åbn `CONSTITUTION.md` og udfyld dit projekts tekniske stack, kodestandarder og non-negotiables.

Dette er den første ting Claude læser. Gør det grundigt.

### 4. Start udvikling med /spec

For hvert nyt feature eller opgave:

```
/spec "Beskriv hvad du vil bygge"
```

Følg faserne i rækkefølge:

```
/spec → [review] → /plan → [review] → /tasks → implement → /review-spec
```

**Review-trinene er dine.** Gennemgå artefakterne inden du går videre.

### 5. Verificér med /review-spec

Når implementeringen er færdig:

```
/review-spec
```

Giver dig pass/fail per acceptance criteria.

---

## Workflow-oversigt

```
Feature-idé
    │
    ▼
/spec ──────────────────────────────────────────────────────────────────
    │  Output: .specs/<feature>/spec.md
    │  Indhold: Intent, Acceptance Criteria, Scope, Constraints
    │
    ▼ [DU REVIEWER OG GODKENDER]
    │
/plan ──────────────────────────────────────────────────────────────────
    │  Output: .specs/<feature>/plan.md
    │  Indhold: Teknisk approach, præcise filer, trin, test-kommandoer
    │
    ▼ [DU REVIEWER OG GODKENDER]
    │
/tasks ─────────────────────────────────────────────────────────────────
    │  Output: .specs/<feature>/tasks.md
    │  Indhold: Atomisk tjekliste, én verificering per task
    │
    ▼ [IMPLEMENTERING — task for task]
    │
/review-spec ───────────────────────────────────────────────────────────
       Output: Pass/fail per acceptance criteria
```

---

## Mappestruktur

```
dit-projekt/
├── CLAUDE.md              ← Agent-regler (rør ikke)
├── CONSTITUTION.md        ← Dine projektstandarder (udfyld dette)
├── .claude/skills/        ← De 4 skills (/spec, /plan, /tasks, /review-spec)
├── .specs/
│   ├── _templates/        ← Skabeloner (reference)
│   ├── example/           ← Worked example (se her for inspiration)
│   └── <feature-name>/    ← Dine feature-specs gemmes her
└── .githooks/
    └── pre-commit         ← Advarsel ved manglende spec
```

---

## Eksempel

Se `.specs/example/` for et komplet gennemgående eksempel (kontaktformular i Flask):
- `spec.md` — udfyldt spec i stakeholder-sprog
- `plan.md` — teknisk plan med præcise filreferencer
- `tasks.md` — atomisk tjekliste klar til implementering

---

## CI/CD

Tre GitHub Actions kører automatisk på pull requests:

| Workflow | Trigger | Hvad den gør |
|----------|---------|--------------|
| **Spec Lint** | PR åbnes/opdateres | Fejler PR hvis spec mangler eller er ufuldstændig |
| **Spec Summary** | PR åbnes/opdateres | Poster acceptance criteria som kommentar på PR |
| **Tests** | Push + PR | Kører projektets test-suite (pytest/jest auto-detekteret) |

**Branch-navnekonvention:** Brug `feature/<spec-name>` så CI kan matche branch til spec automatisk.

```bash
# Eksempel
git checkout -b feature/add-contact-form
# matcher automatisk til .specs/add-contact-form/spec.md
```

**Branch protection (anbefalet):** Gå til Settings → Branches → Add rule på `main`:
- Hak "Require status checks to pass" og vælg **Spec Lint**
- Studerende kan ikke merge uden en godkendt spec

---

## Skills reference

| Skill | Fase | Hvad den gør |
|-------|------|--------------|
| `/spec` | 1 | Genererer spec fra din beskrivelse |
| `/plan` | 2 | Oversætter spec til teknisk implementeringsplan |
| `/tasks` | 3 | Nedbryder plan til atomiske tasks |
| `/review-spec` | 5 | Verificerer implementering mod acceptance criteria |

---

## For undervisere

Se `docs/for-teachers.md` for pædagogisk rationale og klasserumsbrug.

---

## Licens

MIT — brug det frit, tilpas det til dit projekt.
