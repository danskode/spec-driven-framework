# Mønster: UI/visnings-feature

> Referencedokument til brug i plan-fasen. Bruges af /plan — ikke af brugeren direkte.

## De fire tilstande enhver UI-komponent skal håndtere

| Tilstand | Hvornår | Hvad vises |
|----------|---------|-----------|
| **Loading** | Data hentes | Skeleton, spinner eller placeholder |
| **Tom** | Data findes men er tomt | Venlig besked + evt. call-to-action |
| **Fejl** | Request fejlede | Fejlbesked + mulighed for at prøve igen |
| **Populeret** | Data hentet og ikke tomt | Det faktiske indhold |

Alle fire skal defineres i spec. En spec der kun beskriver "populeret"-tilstanden er ufuldstændig.

## Formularer

**Validering:**
- Klient-side: øjeblikkelig feedback uden servekald (format, påkrævede felter)
- Server-side: autoritativ validering (unikhed, forretningsregler)
- Fejlbeskeder: feltspecifikke, placeret tæt på feltet — ikke kun øverst på siden

**Submit-flow:**
- Deaktivér submit-knap under indsendelse (forhindrer dobbelt-submit)
- Vis loading-indikator
- Ved fejl: bevar brugerens input, vis fejlbesked
- Ved success: bekræftelsesbesked eller redirect

## Tilgængelighed (minimum)

- [ ] Alle formularfelter har tilknyttet `<label>`
- [ ] Fejlbeskeder er programmatisk forbundet med feltet (`aria-describedby`)
- [ ] Interaktive elementer kan nås og aktiveres med tastatur alene
- [ ] Farve er ikke det eneste informationsbærende element
- [ ] Billeders `alt`-tekst beskriver indhold — ikke "billede af..."

## Navigation og routing

- Definer eksplicit hvad URL'en er for hver visning
- Hvad sker der ved direkte URL-adgang (deep link)?
- Hvad sker der ved browser back/forward?
- Er der breadcrumbs eller anden kontekstindikator?

## Responsivt design

- Definer mindst to breakpoints: mobil (< 768px) og desktop
- Hvad kollapser eller skjules på mobil?
- Touch targets: minimum 44×44px

## Typiske gotchas

- **Flash of unstyled content:** Vis loading-state fra start, ikke blank side
- **Scroll position:** Bevar scroll ved back-navigation hvis relevant
- **Focus management:** Efter modal/dialog lukkes — hvad får fokus?
- **Lange lister:** Paginering eller infinite scroll? Definer maks. synlige elementer
- **Internationalisation:** Er datoer, tal og valuta formateret til dansk/lokal standard?
