# Constitution: [Projekt Navn]

> Dette dokument er dit projekts grundlov. Det definerer ikke-forhandlingsbare standarder
> som agenten skal overholde i alle faser. Udfyld det, inden du begynder at udvikle.
>
> Slet vejledningsnoter (blockquotes) når du har udfyldt sektionerne.

---

## Teknisk Stack

> Hvilke teknologier, sprog og frameworks bruges i dette projekt?

- **Sprog:** _f.eks. Python 3.12, TypeScript 5_
- **Framework:** _f.eks. FastAPI, Next.js, Flask_
- **Database:** _f.eks. PostgreSQL, SQLite, ingen_
- **Test-framework:** _f.eks. pytest, Jest, unittest_
- **Package manager:** _f.eks. uv, npm, pip_

---

## Kodestandarder

> Hvad er kravene til kodeformat og -kvalitet?

- **Formatering:** _f.eks. black + isort, prettier_
- **Linting:** _f.eks. ruff, eslint_
- **Navngivning:** _f.eks. snake_case for Python, camelCase for JS_
- **Kommentarer:** _f.eks. docstrings på alle public functions_

---

## Non-negotiables

> Regler der aldrig må brydes, uanset hvad specen siger.

- [ ] _f.eks. Ingen eksterne dependencies uden eksplicit godkendelse_
- [ ] _f.eks. Al brugerinput valideres ved system-grænsen_
- [ ] _f.eks. Ingen hardcoded secrets eller API-nøgler_
- [ ] _f.eks. Tests skal eksistere for alle nye features_

---

## Afhængigheder og Begrænsninger

> Eksterne systemer, APIs eller services som projektet afhænger af.

- _f.eks. Eksisterende auth-service på port 8001_
- _f.eks. Produktion kører på Python 3.11 (ikke 3.12)_

---

## Out of Scope (altid)

> Hvad må agenten aldrig gøre i dette projekt, uanset instruktion?

- _f.eks. Redigér migrations-filer manuelt_
- _f.eks. Slet data fra databasen_
- _f.eks. Push direkte til main-branch_
