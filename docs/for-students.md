# Guide til Studerende

## Hvad er spec-driven development?

Spec-driven development er en arbejdsmetode hvor du definerer **hvad** du vil bygge, inden du (eller en AI-agent) begynder at skrive kode.

Det lyder simpelt — og det er det i princippet. Det svære er disciplinen: at holde sig fra at hoppe direkte til kode, selv når man har en god idé om løsningen.

**Problemet med vibe coding:**
Du fortæller en AI "byg en login-side" og får 200 linjer kode. Virker det? Måske. Gør det det du mente? Uvist. Kan du forklare hvad det gør til din underviser? Sandsynligvis ikke.

**Med spec-first:**
Du ved præcist hvad koden skal gøre, fordi du definerede det selv. AI'en er et værktøj — du er arkitekten.

---

## Faserne forklaret

### Fase 1: `/spec` — Hvad vil du bygge?

Kør `/spec "Beskriv din feature"` og Claude stiller dig spørgsmål for at forstå dit intent.

Output er en `spec.md` med:
- **Intent:** Hvad og hvorfor (i menneske-sprog)
- **Acceptance Criteria:** Konkrete tjekpunkter der definerer "færdig"
- **Scope:** Hvad er med og hvad er bevidst valgt fra
- **Constraints:** Begrænsninger (tech stack, tid, afhængigheder)

**Din opgave:** Læs spec.md. Er det det du mente? Ret inden du går videre.

### Fase 2: `/plan` — Hvordan bygges det?

Claude researcher din kodebase og laver en teknisk plan med præcise filer, funktioner og trin.

**Din opgave:** Tjek at planens filer eksisterer. Er trinene i den rigtige rækkefølge? Er rollback-strategien fornuftig?

### Fase 3: `/tasks` — Hvad er næste skridt?

Planen nedbrydes til en atomisk tjekliste. Hvert punkt kan gennemføres i én agent-session.

**Din opgave:** Verificér at hvert task er konkret nok. "Implementer login" er for vagt. "Tilføj `check_password()` i `app/auth.py`" er konkret.

### Fase 4: Implementering

Arbejd task for task. Kør verifikationen for hvert task inden du går videre. Commit hyppigt.

**Din opgave:** Du er i kontrol. AI'en udfører — du godkender.

### Fase 5: `/review-spec` — Virker det som specet?

Claude læser dine acceptance criteria og tjekker om implementeringen opfylder dem.

Output: Pass/fail per kriterie med præcis fejlbeskrivelse.

---

## Typiske fejl (og hvordan du undgår dem)

**"Jeg springer plan-fasen over, jeg ved hvad jeg vil gøre"**
Gør det alligevel. Planen tvinger dig til at tænke på edge cases og rækkefølge. Prisen er 5 minutter. Gevinsten er at du ikke debugger i 2 timer.

**"Mine acceptance criteria er vage"**
Dårligt eksempel: `- [ ] Brugeren kan logge ind`
Godt eksempel: `- [ ] POST /login med korrekte credentials returnerer 200 og sætter session-cookie`

Test: Kan du besvare kriteriet med "ja" eller "nej"? Nej → gør det mere konkret.

**"AI'en har ændret noget jeg ikke bad om"**
Det er scope-drift. Tjek CLAUDE.md — agenten må ikke improvisere. Kig på hvad tasks.md sagde og sammenlign med hvad der faktisk skete. Rapportér til din underviser — det er et læringsmoment.

**"Jeg forstår ikke hvad planen siger"**
Spørg Claude om at forklare et specifikt trin. Hvis du ikke forstår planen, bør du ikke godkende den.

---

## Gode vaner

- **Commit efter hvert task** — du kan altid gå tilbage
- **Ét feature ad gangen** — start ikke `/spec` nr. 2 inden nr. 1 er reviewet med `/review-spec`
- **CONSTITUTION.md er dit fundament** — udfyld den grundigt i projektets start
- **Review er din tid** — brug de 2-3 minutter til at læse hvert artefakt. Det er her du lærer.
