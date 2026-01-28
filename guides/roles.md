---
title: OS2rollekatalog rettighedsmodel
layout: default
nav_order: 1
parent: Guides
has_children: false
---
# Guide til roller i OS2rollekatalog

## Overgang til ny rettighedsmodel

OS2rollekatalog har fået en ny rettighedsmodel, hvor adgangen til systemet er blevet opdelt i forskellige sektioner. Dine rettigheder styres nu af de roller, du har fået tildelt i systemet.

I forbindelse med overgangen er alle eksisterende roller i OS2rollekatalog blevet automatisk migreret til den nye model. Det betyder, at du ikke skulle miste nogen af dine nuværende adgange til at arbejde i rollekataloget. Dine rettigheder er blevet overført, så du kan fortsætte dit arbejde som hidtil.

## Hvordan fungerer den nye rettighedsmodel?

Rollekataloget er nu funktionelt opdelt i forskellige sektioner:
- Rollebuketter
- Jobfunktionsroller
- IT-systemer
- Enheder
- Brugere
- Rapporter
- Log
- Advisering
- Leder
- Konfiguration
- Attestation

For at få adgang til en sektion skal du have en rolle, der giver den rette tilladelse til netop den sektion. Nogle roller kan desuden være **begrænset til specifikke organisationsenheder eller it-systemer**, hvilket betyder at du kun kan se og arbejde med de enheder eller systemer, som din rolle giver dig adgang til.

## Begrænsninger på roller

Mange roller kan begrænses, så du kun kan arbejde med bestemte dele af organisationen eller specifikke it-systemer.

### Begrænsning til organisationsenheder
Når din rolle er begrænset til en eller flere organisationsenheder, kan du kun se og arbejde med:
- Den specifikke enhed
- Alle underenheder til denne enhed (hvis begrænsningen inkluderer hierarki)
- Brugere tilknyttet disse enheder

**Eksempel:** Hvis du har rollen "Bruger - opdater" begrænset til "Socialforvaltningen" med hierarki, kan du kun se og redigere brugere i Socialforvaltningen og alle dens underafdelinger. Hvis begrænsningen er uden hierarki, kan du kun se brugere direkte tilknyttet Socialforvaltningen.

### Begrænsning til it-systemer
Når din rolle er begrænset til et eller flere it-systemer, kan du kun se og arbejde med:
- Jobfunktionsroller for disse systemer
- Rollebuketter, hvor alle indeholdte roller er fra disse systemer

**Eksempel:** Hvis du har rollen "Jobfunktionsrolle - opdater" begrænset til "Acadre", kan du kun ændre jobfunktionsroller, der hører til Acadre-systemet.

## Oversigt over systemroller

Nedenstående tabel viser alle systemroller i OS2rollekatalog, hvilke sektioner de giver adgang til, og om de kan begrænses.

| Rolle | Adgang til sektioner | Kan begrænses |
|-------|---------------------|---------------|
| **Administrator** | Fuld adgang til alle sektioner | Nej |
| **Administration** | Konfiguration (læse, oprette, ændre, slette) | Nej |
| **Adviser** | Advisering (læse, oprette, ændre, slette) | Nej |
| **Anmod/Godkend Bemyndiget** | Afhængig af opsætning kan denne rolle give adgang til at oprette og godkende rolleanmodninger | Ja - organisationsenheder og it-systemer |
| **Attesterings Administrator** | Attestation | Nej |
| **Auditlog** | Log (læse, oprette, ændre, slette) | Nej |
| **Bruger - læs** | Brugere (læse) | Ja - organisationsenheder |
| **Bruger - opdater** | Brugere (læse, oprette, ændre, slette) | Ja - organisationsenheder |
| **Enhed - læs** | Enheder (læse) | Ja - organisationsenheder |
| **Enhed - opdater** | Enheder (læse, oprette, ændre, slette) | Ja - organisationsenheder |
| **Global rolletildeler** | Log (læse), Jobfunktionsroller (læse), Rollebuketter (læse), IT-systemer (læse), Brugere (læse), Enheder (læse) + kan tildele roller | Nej |
| **IT System - læs** | IT-systemer (læse) | Ja - it-systemer |
| **IT System - opdater** | IT-systemer (læse, ændre) | Ja - it-systemer |
| **IT System - opret** | IT-systemer (læse, oprette) | Nej |
| **IT System - slet** | IT-systemer (læse, slette) | Ja - it-systemer |
| **Jobfunktionsrolle - læs** | Jobfunktionsroller (læse) | Ja - it-systemer |
| **Jobfunktionsrolle - opdater** | Jobfunktionsroller (læse, ændre) | Ja - it-systemer |
| **Jobfunktionsrolle - opret** | Jobfunktionsroller (læse, oprette) | Ja - it-systemer |
| **Jobfunktionsrolle - slet** | Jobfunktionsroller (læse, slette) | Ja - it-systemer |
| **KLE administrator** | KLE-funktionalitet | Nej |
| **Ledere - administrer** | Leder (læse, oprette, ændre, slette) | Ja - organisationsenheder |
| **Ledere - læs** | Leder (læse) | Ja - organisationsenheder |
| **Læseadgang** | Jobfunktionsroller (læse), Rollebuketter (læse), IT-systemer (læse), Brugere (læse), Enheder (læse) | Ja - it-systemer og/eller organisationsenheder |
| **Rapport adgang** | Rapporter (læse, oprette, ændre, slette) | Nej |
| **Rollebuket - læs** | Rollebuketter (læse) | Ja - it-systemer (kun buketter hvor alle roller er fra begrænsede systemer) |
| **Rollebuket - opdater** | Rollebuketter (læse, ændre) | Ja - it-systemer (kun buketter hvor alle roller er fra begrænsede systemer) |
| **Rollebuket - opret** | Rollebuketter (læse, oprette) | Nej |
| **Rollebuket - slet** | Rollebuketter (læse, slette) | Ja - it-systemer (kun buketter hvor alle roller er fra begrænsede systemer) |
| **Rolletildeler** | Jobfunktionsroller (læse), Rollebuketter (læse), IT-systemer (læse), Brugere (læse), Enheder (læse) + kan tildele roller | Nej |

### Særlige bemærkninger til roller

**Ledere - administrer:** Brugere, der er registreret som ledere eller stedfortrædere for en organisationsenhed, får automatisk denne rolle begrænset til deres egen enhed.

**Rolletildeler og Global rolletildeler:** Begge roller giver mulighed for at tildele og fjerne roller til/fra brugere. Forskellen er, at Global rolletildeler også har adgang til auditloggen.

## Ofte stillede spørgsmål

**Hvorfor kan jeg ikke se nogen jobfunktionsroller, selvom jeg har rollen "Rolletildeler"?**
Rollen "Rolletildeler" giver kun ret til at tildele roller - ikke til at se dem. For at kunne se jobfunktionsroller og rollebuketter skal brugeren også have læserollerne:
- "Jobfunktionsrolle - læs" for at se jobfunktionsroller
- "Rollebuket - læs" for at se rollebuketter
- Alternativt "Læseadgang" som giver adgang til at se jobfunktionsroller, rollebuketter, it-systemer, brugere og enheder

**Hvorfor kan jeg ikke se alle it-systemer?**
Rollen er sandsynligvis begrænset til kun at vise bestemte it-systemer. Tjek om der er sat begrænsninger på rollen.

**Hvorfor kan jeg se en jobfunktionsrolle, men ikke ændre den?**
Brugeren har sandsynligvis rollen "Jobfunktionsrolle - læs" for det pågældende system, men ikke "Jobfunktionsrolle - opdater". Opdateringsrollen er nødvendig for at kunne foretage ændringer.

**Hvad er forskellen på "Rolletildeler" og "Global rolletildeler"?**
De har samme grundlæggende rettigheder til at tildele roller, men "Global rolletildeler" har også adgang til auditloggen, så de kan se, hvem der har foretaget hvilke rolletildelinger.

## Sådan tildeler du roller

For at tildele roller til en bruger skal du selv have enten rollen "Rolletildeler" eller "Global rolletildeler". Husk at brugeren også skal have de nødvendige læseroller for at kunne se det indhold, de skal arbejde med.