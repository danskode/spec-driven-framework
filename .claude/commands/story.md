# /story — Generer User Stories (PO-perspektiv)

Du er i **Fase 0: User Stories**. Dette trin er valgfrit men anbefalet for større features eller når du ønsker at tænke fra brugerens perspektiv inden du tænker teknisk.

Output fra dette trin føder direkte ind i `/spec`.

---

## Trin 1 — Forstå forretningskonteksten

Stil maksimalt 2 spørgsmål for at forstå:
- Hvem er den primære bruger? (rolle, kontekst, teknisk niveau)
- Hvad er det forretningsmæssige mål? (ikke "hvad skal bygges" men "hvad skal opnås")

Vent på svar inden du fortsætter.

## Trin 2 — Skriv user stories

Opret: `.specs/<feature-name>/user-stories.md`

Feature-navn: kebab-case, beskrivende, samme navn der vil blive brugt i `/spec`.

### Format per story

```
## Story [N]: [Kort titel]

**Som** [rolle]
**Vil jeg** [handling / kapabilitet]
**Så jeg kan** [forretningsmæssigt mål / udbytte]

### Acceptkriterier
- [ ] Givet [kontekst], når [handling], så [forventet resultat]
- [ ] Givet [kontekst], når [handling], så [forventet resultat]

### Prioritet
Must have / Should have / Could have
```

### Hvad gør en god story

- **Rolle** er specifik: "studerende", "underviser", "gæst" — ikke "bruger"
- **Handling** er bruger-vendt: "nulstille min adgangskode" — ikke "kalde POST /auth/reset"
- **Mål** forklarer *hvorfor*: "så jeg ikke er låst ude" — ikke "så det virker"
- **Acceptkriterier** bruger Givet/Når/Så-format og er testbare

## Trin 3 — Dæk de vigtige perspektiver

Sørg for at stories dækker:
- **Happy path:** Det normale, succesfulde forløb
- **Alternativt forløb:** Hvad sker der ved fejl eller uventet input?
- **Kant-cases:** Tom tilstand, grænseværdier, første gang en bruger møder featuren

Tilføj manglende perspektiver automatisk — spørg ikke brugeren.

## Trin 4 — Præsenter og afvent godkendelse

Vis user-stories.md og sig:

> "User stories gemt i `.specs/<feature-name>/user-stories.md`."
>
> "Gennemgå stories og prioriteringer. Er der roller eller scenarier der mangler?
>
> Kør `/spec` når du er klar — den læser automatisk dine user stories og oversætter dem til verificerbare acceptance criteria."

## Regler

- Skriv IKKE i teknisk sprog (ingen HTTP, ingen klassenavne, ingen databasetermer)
- Skriv ÉN story per brugerkapabilitet — ikke én kæmpe story der dækker alt
- Must have-stories skal kunne implementeres uafhængigt af Could have-stories
