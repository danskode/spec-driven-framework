# /spec — Generer Feature Specification

Du er i **Fase 1: Specify**. Din opgave er at hjælpe brugeren med at skrive en klar, verificerbar spec — inden nogen kode skrives.

## Trin 1 — Læs projektkontekst

Læs disse filer inden du begynder:
- `CONSTITUTION.md` — projektstandarder og constraints
- Eksisterende `.specs/` mapper — for at forstå konventioner og scope

Hvis `CONSTITUTION.md` ikke er udfyldt, stop og bed brugeren om at udfylde den først.

Tjek om der eksisterer en `.specs/<feature-name>/user-stories.md` fra et forudgående `/story`-kørsel. Hvis ja: læs den og brug stories og acceptkriterier som primært input til spec.md — de repræsenterer allerede godkendt intent og prioritering. Spring da de fleste af Trin 2's afklarende spørgsmål over.

## Trin 2 — Forstå intent

Identificér først feature-typen ud fra brugerens beskrivelse — det bestemmer hvilke spørgsmål der er vigtigst:

- **CRUD** (opret/læs/rediger/slet data): Hvem har adgang? Hvad valideres? Hvad sker ved duplikater?
- **Auth** (login/logout/signup/reset): Hvad sker med eksisterende sessioner? Hvad er lockout-regler?
- **UI/visning** (lister, detaljesider, dashboards): Hvad vises ved tom tilstand? Hvad er fallback ved load-fejl?
- **Søgning/filtrering**: Hvad returneres ved ingen resultater? Er søgning case-sensitiv?
- **Baggrundsjob/asynkron**: Hvornår kører den? Hvad sker ved fejl? Hvem notificeres?

Stil maksimalt 3 afklarende spørgsmål — vælg dem der er mest kritiske for den identificerede feature-type. Vent på svar inden du fortsætter.

## Trin 3 — Opret spec.md

Generér et feature-navn i kebab-case fra brugerens beskrivelse.

Opret filen: `.specs/<feature-name>/spec.md`

Brug skabelonen fra `.specs/_templates/spec-template.md`. Udfyld alle sektioner:
- **Intent:** Skriv i stakeholder-sprog. Ingen implementeringsdetaljer.
- **Acceptance Criteria:** Minimum 3 verificerbare kriterier. Hvert punkt kan besvares ja/nej.
- **Scope:** Vær eksplicit om hvad der er ude af scope.
- **Constraints:** Henvis til CONSTITUTION.md for relevante begrænsninger.

## Trin 3b — Robusthedstjek (intern selvkritik — spørg ikke brugeren)

Kør dette tjek på de acceptance criteria du netop har skrevet, inden du gemmer filen. For hvert punkt der fejler: tilføj det manglende kriterie automatisk.

### Routes og URLs
- Er der et kriterie for hvad `GET <url>` returnerer (HTTP 200 — ikke 302 eller 404)?
- Hvis ruten kræver login: er login-siden selv dækket af et kriterie der verificerer den er tilgængelig?
- Hvis der er en redirect: er destinations-URL'en dækket af et selvstændigt kriterie?

### Negative stier
- Hvad sker der ved ugyldigt input? Tilføj kriterie for den negative sti hvis det mangler.
- Hvad sker der ved ikke-eksisterende ressource (404-scenarie)? Dæk det eksplicit.
- Hvis auth er i scope: er der et kriterie for den ikke-autentificerede bruger?

### Data state
- For CREATE: er der et kriterie der verificerer at ressourcen *eksisterer* efterfølgende (ikke bare at HTTP 201 returneres)?
- For UPDATE: er der et kriterie der verificerer at den *ændrede* værdi er gemt — ikke bare at 200 returneres?
- For DELETE: er der et kriterie der verificerer at ressourcen *ikke længere* kan hentes efterfølgende?

### Tom tilstand og grænseværdier
- Hvad vises/returneres hvis der ingen data er (tom liste, 0 resultater, ny bruger)? Tilføj et kriterie hvis det mangler.
- Hvis der er inputfelter: er der et kriterie for grænseværdier (maks. længde, tomt felt, specialtegn)?

### Dobbelt-submit og idempotens
- Hvad sker der hvis brugeren indsender formularen to gange (dobbeltklik, back-knap)? Tilføj kriterie hvis operationen ikke er idempotent.
- Hvad sker der hvis den samme ressource oprettes to gange? Er duplikathåndtering dækket?

### Scope og afhængigheder
- Forudsætter et kriterie infrastruktur (database, ekstern service, miljøvariabel) der ikke er nævnt i Scope eller Constraints? Tilføj den manglende forudsætning.
- Forudsætter specen en feature der ikke eksisterer endnu (tjek `.specs/` for eksisterende specs)? Tilføj den som dependency i Constraints.
- Har specen mere end 8 acceptance criteria? Overvej om den bør splittes i to specs — én per ansvarsområde.

### Verificerbarhed
- Kan hvert enkelt kriterie besvares ja/nej uden at køre applikationen? Hvis nej: omskriv til noget verificerbart (konkret HTTP-status, konkret tekstindhold, konkret DB-tilstand).

**Resultat:** Opdater acceptance criteria-listen direkte. Skriv ingen forklaring til brugeren om hvad du tilføjede — gem bare den forbedrede version.

## Trin 4 — Præsenter og afvent godkendelse

Vis spec.md indholdet til brugeren og sig:

> "Spec gemt i `.specs/<feature-name>/spec.md`. Gennemgå den og ret hvis nødvendigt.
> Kør `/plan` når du er klar til næste fase."

## Regler

- Skriv IKKE kode eller implementeringsforslag
- Foreslå IKKE teknisk approach — det er plan-fasen
- Artefakt-sprog: dansk eller engelsk, konsistent med eksisterende specs
