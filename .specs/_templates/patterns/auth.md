# Mønster: Auth-feature

> Referencedokument til brug i plan-fasen. Bruges af /plan — ikke af brugeren direkte.

## Typiske komponenter

| Komponent | Ansvar |
|-----------|--------|
| Login endpoint | Verificér credentials, opret session/token |
| Logout endpoint | Invalider session/token |
| Signup endpoint | Opret bruger, hash password, send verificerings-email |
| Password reset | Generer token, send email, verificér token, opdater password |
| Auth middleware | Verificér session/token på beskyttede ruter |

## Session vs. token

| Tilgang | Hvornår | Gotcha |
|---------|---------|--------|
| Server-side session | Traditionel webapp, samme server | Skalerer ikke horisontalt uden session store |
| JWT token | API, SPA, mobilapp | Kan ikke invalideres uden blocklist — brug kort TTL |
| Cookie-baseret | Webapp med CSRF-beskyttelse | HttpOnly + Secure + SameSite=Strict |

## Sikkerhedscheckliste

- [ ] Passwords hashes med bcrypt / Argon2 — aldrig MD5/SHA1
- [ ] Login-fejl returnerer samme besked uanset om email eller password er forkert (forhindrer bruger-enumeration)
- [ ] Brute force beskyttelse: rate limiting eller account lockout efter N forsøg
- [ ] Reset-tokens er single-use og udløber (maks. 1 time)
- [ ] Sessioner invalideres ved logout — ikke kun på klient-siden
- [ ] HTTPS kræves til alle auth-endpoints
- [ ] CSRF-token på alle state-ændrende formularer

## HTTP-kontrakt

| Operation | Metode | Succeskode | Fejlkode |
|-----------|--------|-----------|---------|
| Login | POST | 200 + session/token | 401 (forkerte credentials) |
| Logout | POST | 204 | 401 (ikke logget ind) |
| Signup | POST | 201 | 400 (validering), 409 (email taget) |
| Reset request | POST | 204 (altid — afslør ikke om email findes) | — |
| Reset confirm | POST | 200 | 400 (ugyldigt/udløbet token) |

## Typiske gotchas

- **Timing attacks:** Brug konstant-tids sammenligning ved token-verificering
- **Session fixation:** Generer ny session-ID ved login
- **Remember me:** Brug separat, langlivet token — ikke forlænget session
- **Email verificering:** Blokér login til email er verificeret? Definer eksplicit.
- **Concurrent sessions:** Tillad flere samtidige sessioner? Invalider alle ved reset?

## Testmønster

```
Enhedstest:  Password hashing, token generering
Integration: Login flow end-to-end, session persistence
Security:    Forkerte credentials, udløbne tokens, replay attacks
```
