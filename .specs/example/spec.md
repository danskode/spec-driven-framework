# Spec: add-contact-form

> Eksempel på en udfyldt spec. Brug denne som reference når du skriver din første.

## Intent

Brugere på hjemmesiden skal kunne sende en besked til virksomheden via en kontaktformular, så de har en direkte kanal uden at skulle åbne deres e-mail-klient.

Formularen erstatter den eksisterende statiske "kontakt os"-side med en e-mail-adresse.

## Acceptance Criteria

- [ ] Formularen viser felterne: Navn (påkrævet), E-mail (påkrævet, valideret), Besked (påkrævet, min. 10 tegn)
- [ ] Validering sker på klient-siden med feedback inden indsendelse
- [ ] Ved succesfuld indsendelse sendes en e-mail til `kontakt@eksempel.dk`
- [ ] Brugeren ser en bekræftelsesbesked efter indsendelse
- [ ] Ved serverfejl vises en fejlbesked — formularen mister ikke brugerens input
- [ ] Formularen er tilgængelig og kan bruges med tastatur alene

## Scope

**Inkluderet:**
- HTML-formular med klient-side validering
- Server-side endpoint der modtager og videresender besked
- E-mail-afsendelse via eksisterende SMTP-opsætning
- Bekræftelsesbesked til brugeren

**Ekskluderet (eksplicit):**
- Gemning af beskeder i database (kun e-mail)
- Admin-panel til at se indsendte beskeder
- Captcha eller spam-filtrering
- Automatisk svar-e-mail til afsender

## Constraints

- **Tech stack:** Python/Flask, Jinja2 templates, vanilla HTML/CSS (ingen JS-frameworks)
- **E-mail:** Eksisterende Flask-Mail opsætning i `app/mail.py` bruges — opret ikke ny
- **Validering:** Brug WTForms til server-side validering (allerede i requirements.txt)
- **Styling:** Følg eksisterende CSS-konventioner i `static/css/main.css`
