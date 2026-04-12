# Mønster: CRUD-feature

> Referencedokument til brug i plan-fasen. Bruges af /plan — ikke af brugeren direkte.

## Typiske lag

| Lag | Ansvar | Eksempel |
|-----|--------|---------|
| Controller / Route | Modtager HTTP-request, returnerer response | `UserController`, `POST /users` |
| Service / Use case | Forretningslogik, validering, orkestrering | `UserService.createUser()` |
| Repository / DAO | Dataacess, ingen forretningslogik | `UserRepository.save()` |
| Model / Entity | Datastruktur, feltdefinitioner | `User`, `@Entity` |

## Standard operationer og HTTP-kontrakt

| Operation | Metode | Succeskode | Fejlkode |
|-----------|--------|-----------|---------|
| Opret | POST | 201 Created | 400 (validering), 409 (duplikat) |
| Læs én | GET | 200 OK | 404 (ikke fundet) |
| Læs liste | GET | 200 OK (tom liste = [], ikke 404) | — |
| Opdater | PUT / PATCH | 200 OK | 400, 404 |
| Slet | DELETE | 204 No Content | 404 |

## Valideringscheckliste

- [ ] Påkrævede felter afvises med 400 hvis de mangler
- [ ] Felttyper valideres (string, int, email, dato)
- [ ] Unikhedskrav tjekkes inden skrivning (returner 409, ikke 500)
- [ ] Fremmednøgler verificeres (returnér 400/404, ikke DB-fejl)
- [ ] Maks. feltlængder håndhæves

## Fejlhåndtering

- Ikke-eksisterende ressource → 404 med beskrivende besked
- Duplikat → 409 Conflict
- Validering fejler → 400 med feltspecifikke fejlbeskeder
- Uventet fejl → 500, log stack trace, returner ikke intern detaljer til klient

## Typiske gotchas

- **N+1 queries:** Hent relaterede data i én query, ikke i en løkke
- **Cascade delete:** Hvad sker med relaterede records ved sletning? Definer eksplicit.
- **Soft delete vs. hard delete:** Er sletning permanent eller logisk (deleted_at)?
- **Optimistic locking:** Hvis to brugere opdaterer samme record — hvem vinder?
- **Tom liste:** `GET /resources` med ingen data returnerer `[]` med 200, ikke 404

## Testmønster

```
Enhedstest:  Service-laget isoleret (mock repository)
Integration: Controller → Service → Repository mod testdatabase
```
