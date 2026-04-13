# /tasks — Generer Task-liste

Du er i **Fase 3: Tasks**. Du må ikke redigere kode-filer i denne fase.

## Trin 1 — Find og læs plan.md

Find den relevante plan i `.specs/`:
- Hvis brugeren angav et feature-navn som argument (f.eks. `/tasks add-contact-form`): brug det
- Hvis kun én feature-mappe eksisterer med plan.md men uden tasks.md: brug den
- Hvis flere feature-mapper har plan.md: spørg brugeren hvilken feature der nedbryddes
- Hvis ingen plan.md eksisterer: stop og bed brugeren om at køre `/plan` først

Læs hele `.specs/<feature-name>/plan.md`. Notér:
- Antal trin og deres afhængigheder
- Risikable trin der kræver ekstra forsigtighed
- Test-kommandoer per trin

## Trin 2 — Nedbryd til atomiske tasks

Hvert task skal opfylde alle tre kriterier:
1. **Afgrænset:** Kan gennemføres i ét kontekstvindue (én agent-session)
2. **Verificerbart:** Har en konkret test-kommando eller check der bekræfter success
3. **Ikke-improviserende:** Præcis nok til at agenten ikke behøver at tage designbeslutninger

Hvis et plan-trin er for stort til ét kontekstvindue, split det i sub-tasks.

## Trin 3 — Skriv tasks.md

Opret: `.specs/<feature-name>/tasks.md`

Format per task:
```
- [ ] **Task N:** [Præcis beskrivelse af hvad der sker — inkl. konkret fil fra plan.md]
  - Verificering: `<kommando>` eller [check der beviser det virker]
```

Hvert task-navn skal indeholde den præcise filsti fra plan.md's **Files to Change**. Skriv f.eks. "Opret `app/forms.py` med ContactForm-klasse" — ikke "Opret form-filen".

Afslut med done-kriterier:
```
## Done
Alle tasks markeret ✓ OG `/review-spec` giver pass på alle acceptance criteria.
```

## Trin 4 — Præsenter og afvent

Sig følgende til brugeren:

> "Tasks gemt i `.specs/<feature-name>/tasks.md`."
>
> "Du er nu klar til **Fase 4: Implement**. Sådan fortsætter du:
>
> - Sig **'implementer task 1'** for at starte — én ad gangen giver mest kontrol
> - Eller sig **'implementer alle tasks'** for at køre dem i forlængelse af hinanden
> - **Commit efter hvert task** med en beskrivende besked der refererer til feature-navnet
>
> Når alle tasks er markeret ✓, kør `/review-spec` for at verificere mod spec.md."

## Regler

- Redigér IKKE kode-filer
- Hvert task skal have én verificeringshandling
- Tasks er rækkefølge-afhængige — skriv dem i den rigtige orden
- Brug præcise filstier fra plan.md, ikke generiske beskrivelser
