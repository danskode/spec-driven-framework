# Tasks: add-contact-form

**Plan-reference:** `.specs/add-contact-form/plan.md`

## Tasks

- [ ] **Task 1:** Opret `ContactForm` klasse i `app/forms.py` med felterne name, email og message inkl. WTForms-validatorer
  - Verificering: `python -c "from app.forms import ContactForm; print('OK')"` returnerer OK uden fejl

- [ ] **Task 2:** Opret `app/routes/contact.py` med GET og POST handlers — POST kalder eksisterende `send_email()` fra `app/mail.py`
  - Verificering: `flask routes | grep contact` viser to ruter

- [ ] **Task 3:** Registrer contact blueprint i `app/__init__.py`
  - Verificering: `flask routes | grep /contact` viser `/contact [GET, POST]`

- [ ] **Task 4:** Opret `app/templates/contact.html` der extender `base.html`, viser formular med feltfejl og label/aria-attributter
  - Verificering: `curl -s http://localhost:5000/contact | grep '<form'` returnerer form-tag

- [ ] **Task 5:** Opret `app/templates/contact_success.html` med bekræftelsesbesked og link til forsiden
  - Verificering: Visuel check — POST valid form → redirect til success-side med besked

- [ ] **Task 6:** Kør integrationstest — POST ugyldig e-mail skal give feltfejl, POST valid data skal sende e-mail og redirecte
  - Verificering: `pytest tests/test_contact.py -v` — alle tests grønne

## Done

Alle tasks markeret ✓ OG `/review-spec` giver pass på alle 6 acceptance criteria i `.specs/add-contact-form/spec.md`.
