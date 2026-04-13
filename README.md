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
                                                                  │
                                                     FAILs? → /polish [N]
                                                     All pass? → /loopimprove [N] → PR
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
/story [valgfrit] ───────────────────────────────────────────────────────
    │  Output: .specs/<feature>/user-stories.md
    │  Indhold: User stories i PO-sprog, prioriteter (Must/Should/Could)
    │  Spring over hvis du allerede kender de tekniske krav
    │
    ▼ [DU REVIEWER OG GODKENDER]
    │
/spec ──────────────────────────────────────────────────────────────────
    │  Læser user-stories.md automatisk hvis den findes
    │  Output: .specs/<feature>/spec.md
    │  Indhold: Intent, Acceptance Criteria, Scope, Constraints
    │
    ▼ [DU REVIEWER OG GODKENDER]
    │
/plan ──────────────────────────────────────────────────────────────────
    │  Detekterer feature-type → læser mønsterfil automatisk
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
    │  Output: Pass/fail per acceptance criteria
    │
    ├─ FAILs? → /polish [N] ────────────────────────────────────────────
    │      Output: .specs/<feature>/polish-log.md
    │      Retter fejlende kriterier automatisk over N iterationer
    │      Kør /review-spec igen til sidst
    │
    └─ All passing? → /loopimprove [N] ────────────────────────────────
           Output: .specs/<feature>/loopimprove-log.md + ny branch
           Forbedrer kodekvalitet med friske øjne over N runder
           Kræver human PR-review — merger aldrig selv
```

---

## Mappestruktur

```
dit-projekt/
├── CLAUDE.md              ← Agent-regler (rør ikke)
├── CONSTITUTION.md        ← Dine projektstandarder (udfyld dette)
├── .claude/commands/      ← Commands (/spec, /plan, /tasks, /review-spec, /polish, /loopimprove)
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
| `/story` | 0 (valgfrit) | PO-perspektiv: user stories og prioriteter før spec |
| `/spec` | 1 | Genererer spec — læser user-stories.md automatisk hvis den findes |
| `/plan` | 2 | Oversætter spec til teknisk plan — vælger mønsterfil automatisk |
| `/tasks` | 3 | Nedbryder plan til atomiske tasks |
| `/review-spec` | 5 | Verificerer implementering mod acceptance criteria |
| `/polish [N]` | 6 (valgfrit) | Retter fejlende criteria automatisk, N iterationer (default 3) |
| `/loopimprove [N]` | 7 (valgfrit) | Forbedrer kodekvalitet med friske øjne, N runder, ny branch, kræver PR |

---

## For undervisere

Se `docs/for-teachers.md` for pædagogisk rationale og klasserumsbrug.

---

## Licens

MIT — brug det frit, tilpas det til dit projekt.
