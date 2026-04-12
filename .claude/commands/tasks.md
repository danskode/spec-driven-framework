# /tasks — Generer Task-liste

Du er i **Fase 3: Tasks**. Du må ikke redigere kode-filer i denne fase.

## Trin 1 — Læs plan.md

Find `.specs/<feature-name>/plan.md` (samme feature som seneste spec/plan).

Læs hele planen. Notér:
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
- [ ] **Task N:** [Præcis beskrivelse af hvad der sker]
  - Verificering: `<kommando>` eller [check der beviser det virker]
```

Afslut med done-kriterier:
```
## Done
Alle tasks markeret ✓ OG `/review-spec` giver pass på alle acceptance criteria.
```

## Trin 4 — Præsenter og afvent

> "Tasks gemt i `.specs/<feature-name>/tasks.md`. Du kan nu implementere task for task.
> Kør `/review-spec` når alle tasks er markeret færdige."

## Regler

- Redigér IKKE kode-filer
- Hvert task skal have én verificeringshandling
- Tasks er rækkefølge-afhængige — skriv dem i den rigtige orden
- Brug præcise filstier fra plan.md, ikke generiske beskrivelser
