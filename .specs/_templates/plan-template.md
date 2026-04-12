# Plan: [Feature Name]

> Opret dette dokument efter spec.md er godkendt.
> Planen skal være så præcis at selv en "dum model" kan følge den uden at improvisere.

**Spec-reference:** `.specs/[feature-name]/spec.md`

## Approach

_Beskriv den tekniske strategi og begrundelsen bag valget. Nævn alternativer der blev fravalgt og hvorfor._

## Files to Change

_List præcis hvilke filer der berøres. Inkluder stier og de konkrete funktioner/klasser._

| Fil | Handling | Funktion/sektion |
|-----|----------|-----------------|
| `src/...` | Opret / Rediger / Slet | `function_name()` |

## Steps

_Nummereret og atomisk. Hvert trin skal kunne gennemføres og verificeres uafhængigt._

1. **[Trin 1 titel]**
   - Præcis hvad der sker
   - Hvilken fil ændres
   - Verificering: `[kommando eller check]`

2. **[Trin 2 titel]**
   - ...

## Test Commands

_Kør disse efter hvert trin for at verificere korrekthed._

```bash
# Verificer trin 1
<kommando>

# Verificer trin 2
<kommando>

# Fuld suite
<kommando>
```

## Rollback

_Hvad gøres hvis et trin fejler? Beskriv specifikt for de mest risikable trin._

- **Trin N fejler:** `git checkout -- <fil>` eller `git revert <sha>`
- **Database/state ændret:** _Specifik rollback-procedure_
