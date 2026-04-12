# Plan: add-contact-form

**Spec-reference:** `.specs/add-contact-form/spec.md`

## Approach

Tilføjer en WTForms-baseret kontaktformular til Flask-applikationen. Valgte WTForms frem for manuel validering fordi det allerede er en dependency og giver konsistent validering på tværs af client og server. Flask-Mail bruges til afsendelse (eksisterende opsætning).

**Fravalgt:** JavaScript-baseret validering (spec kræver vanilla, og WTForms giver tilstrækkelig server-side validering med god UX via Jinja2).

## Files to Change

| Fil | Handling | Detalje |
|-----|----------|---------|
| `app/forms.py` | Opret | `ContactForm` klasse med WTForms felter |
| `app/routes/contact.py` | Opret | `GET /contact` og `POST /contact` routes |
| `app/__init__.py` | Rediger | Registrer contact blueprint |
| `app/templates/contact.html` | Opret | Formular-template med fejlvisning |
| `app/templates/contact_success.html` | Opret | Bekræftelsesside |
| `app/mail.py` | Læs (ingen ændring) | Forstå eksisterende `send_email()` signatur |

## Steps

1. **Opret ContactForm i `app/forms.py`**
   - Tilføj `ContactForm(FlaskForm)` med felterne: `name` (StringField, Required), `email` (EmailField, Required + Email validator), `message` (TextAreaField, Required, Length(min=10))
   - Verificering: `python -c "from app.forms import ContactForm; print('OK')"`

2. **Opret contact blueprint i `app/routes/contact.py`**
   - `GET /contact`: Renderer `contact.html` med tomt form
   - `POST /contact`: Validerer form → kalder `send_email()` → redirect til success eller re-render med fejl
   - Verificering: `flask routes | grep contact`

3. **Registrer blueprint i `app/__init__.py`**
   - Tilføj `from app.routes.contact import contact_bp` og `app.register_blueprint(contact_bp)`
   - Verificering: `flask routes | grep contact` viser `/contact`

4. **Opret `app/templates/contact.html`**
   - Extends `base.html`, viser form med feltfejl, submit-knap
   - Tilgængelighed: `<label>` per felt, `aria-describedby` på fejlbeskeder
   - Verificering: `curl -s http://localhost:5000/contact | grep '<form'`

5. **Opret `app/templates/contact_success.html`**
   - Bekræftelsesbesked med link tilbage til forsiden
   - Verificering: Visuel check i browser

6. **Manuelle integrationstest**
   - Verificering: Se Test Commands

## Test Commands

```bash
# Enhedstest (kræver eksisterende test-setup)
pytest tests/test_contact.py -v

# Kør server og manuel test
flask run &
curl -s http://localhost:5000/contact | grep '<form'

# POST med valide data
curl -X POST http://localhost:5000/contact \
  -d "name=Test&email=test@test.dk&message=Dette er en testbesked på 10+ tegn" \
  -L | grep "Tak"

# POST med invalid e-mail (skal fejle)
curl -X POST http://localhost:5000/contact \
  -d "name=Test&email=ikkeenemail&message=Testbesked" \
  | grep "ugyldig\|invalid\|error"
```

## Rollback

- **Trin 1-2 fejler:** Slet de oprettede filer, ingen ændringer i eksisterende kode
- **Trin 3 fejler (blueprint-registrering):** `git checkout app/__init__.py`
- **E-mail virker ikke:** Tjek `app/mail.py` og SMTP-konfiguration i `.env` — contact-koden er uafhængig
