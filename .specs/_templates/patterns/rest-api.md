# Mønster: REST API-feature

> Referencedokument til brug i plan-fasen. Bruges af /plan — ikke af brugeren direkte.

## HTTP-verber og semantik

| Verbum | Brug | Idempotent? | Body? |
|--------|------|-------------|-------|
| GET | Hent ressource(r) | Ja | Nej |
| POST | Opret ressource | Nej | Ja |
| PUT | Erstat hel ressource | Ja | Ja |
| PATCH | Opdater delfelt | Nej (typisk) | Ja |
| DELETE | Slet ressource | Ja | Nej |

## Statuskoder — hvornår bruges hvilken

| Kode | Hvornår |
|------|---------|
| 200 OK | Succesfuld GET, PUT, PATCH |
| 201 Created | Succesfuld POST der opretter ressource |
| 204 No Content | Succesfuld DELETE eller handling uden response-body |
| 400 Bad Request | Ugyldig request-body eller manglende felter |
| 401 Unauthorized | Ikke autentificeret |
| 403 Forbidden | Autentificeret men ikke autoriseret |
| 404 Not Found | Ressource eksisterer ikke |
| 409 Conflict | Duplikat, version-konflikt |
| 422 Unprocessable | Valideringsfejl (alternativ til 400) |
| 500 Internal Server Error | Uventet fejl — log, returner ikke detaljer |

## Request/response-design

**Konsistens:**
- Brug samme feltnavne på tværs af endpoints (snake_case eller camelCase — vælg ét)
- Timestamps: ISO 8601 (`2026-04-12T10:00:00Z`)
- Tomme lister: returner `[]`, ikke `null`
- Fejlformat: `{ "error": "beskrivelse", "field": "feltnavn" }` — konsistent på tværs

**Paginering (hvis liste kan vokse):**
```json
{
  "data": [...],
  "page": 1,
  "per_page": 20,
  "total": 142
}
```

**Filtrering og sortering:**
- Query params: `GET /users?role=admin&sort=created_at&order=desc`
- Definer defaults eksplicit — sortering er ikke fri

## Versionering

- URL-baseret: `/api/v1/users` — synligt, enkelt
- Header-baseret: `Accept: application/vnd.api+json;version=1` — renere URLs, sværere at teste
- Vælg strategi i CONSTITUTION.md og hold den konsistent

## Sikkerhedscheckliste

- [ ] Alle endpoints der ændrer state kræver autentificering
- [ ] Authorization tjekkes per ressource (ikke kun per endpoint)
- [ ] Input saniteres — aldrig råt input i SQL/shell
- [ ] Rate limiting på offentlige endpoints
- [ ] CORS konfigureret eksplicit — ikke `*` i produktion

## Typiske gotchas

- **Returner ikke intern ID-struktur:** Brug UUID eller slug ekstern — ikke auto-increment int
- **Partial update med PATCH:** Kun felter der sendes opdateres — resten bevares
- **DELETE er idempotent:** `DELETE /users/123` på allerede slettet ressource → 204 (ikke 404)
- **Konsumér ikke din egen API i tests:** Test service-laget direkte, ikke via HTTP
