# Mønster: Baggrundsjob / asynkron feature

> Referencedokument til brug i plan-fasen. Bruges af /plan — ikke af brugeren direkte.

## Triggermekanismer

| Type | Hvornår | Eksempel |
|------|---------|---------|
| Cron / tidsbaseret | Fast interval eller tidspunkt | "Kør kl. 02:00 hver nat" |
| Event-drevet | Udløst af en handling i systemet | "Send email når ordre bekræftes" |
| Manuel | Udløst eksplicit af bruger/admin | "Kør rapport på forespørgsel" |
| Queue-baseret | Opgave lagt i kø af andet system | Beskedkø, job queue |

Definer triggermekanisme eksplicit i spec — "kør i baggrunden" er ikke nok.

## Idempotens

Baggrundsjobs **skal** være idempotente: køres jobbet to gange med samme input, skal resultatet være det samme som én kørsel.

Tjek:
- Er der duplicate-beskyttelse? (unik constraint, idempotency key)
- Hvad sker der hvis jobbet afbrydes midt i? (delvis udførelse)
- Kan jobbet genoptages sikkert fra checkpoint?

## Fejlhåndtering og retry

| Beslutning | Valg |
|-----------|------|
| Max antal forsøg | Definer eksplicit (f.eks. 3) |
| Backoff-strategi | Exponential backoff (1s, 2s, 4s...) eller fast interval |
| Dead letter queue | Hvad sker med jobs der aldrig lykkes? |
| Alert ved fejl | Hvem notificeres? Hvordan? |

## Logging og observabilitet

- [ ] Hvert job logger start, slut og antal behandlede records
- [ ] Fejl logges med stack trace og job-ID
- [ ] Varighed logges — basis for performance-monitorering
- [ ] Job-ID er sporbart fra trigger til completion

## Ressourceforbrug

- Hvad er maks. køretid? (timeout)
- Kører jobbet eksklusivt eller parallelt med andre?
- Belaster jobbet databasen? (kør udenfor peak-hours, brug batching)
- Memory: hvad er maks. datasæt der indlæses ad gangen?

## Typiske gotchas

- **Lock contention:** Brug database-lock eller distributed lock hvis kun én instans må køre ad gangen
- **Tidszone:** Cron-tidspunkter — er det UTC eller lokal tid? Definer eksplicit
- **Stille fejl:** Jobs der "lykkes" uden at gøre noget — log antal behandlede records
- **Stor datasæt:** Indlæs ikke alle records i hukommelsen — brug cursor/paginering
- **Email-afsendelse:** Kør afsendelse i job, ikke synkront i request-handling
