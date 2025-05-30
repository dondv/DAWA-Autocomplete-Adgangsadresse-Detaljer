# DAWA Adresse Autocomplete med Detaljeret Opslag

Dette projekt demonstrerer en integration med Danmarks Adressers Web API (DAWA) for at skabe et inputfelt med adresse-autocomplete funktionalitet. Når en adresse vælges fra autocomplete-listen, hentes og vises yderligere detaljerede oplysninger om den tilhørende adgangsadresse, inklusive dens historik.

Koden er skrevet i ren HTML, CSS og JavaScript og er designet til at køre direkte i browseren. Den kan også nemt integreres i platforme som Salesforce Marketing Cloud Cloud Pages.

## Funktioner

*   **Adresse Autocomplete:** Dynamisk visning af adresseforslag mens brugeren skriver, baseret på DAWA's `/autocomplete` API.
*   **Detaljeret Adgangsadresseopslag:** Efter valg af adresse hentes fulde detaljer for adgangsadressen, inklusiv:
    *   Nuværende data for adgangsadressen.
    *   **Historik:** Tidligere versioner af adgangsadressen med virkningsperioder.
    *   Dette opnås via DAWA's `/adgangsadresser/{id}` API med `historik=true` og `struktur=nestet`.
*   **Fuzzy Search:** Autocomplete understøtter fuzzy matching for at håndtere mindre tastefejl.
*   **Debouncing:** API-kald til autocomplete er "debounced" for at forbedre ydeevnen og reducere antallet af API-kald.
*   **Simpel Visning:** Både autocomplete-resultatet og de detaljerede adgangsadresseoplysninger vises som rå JSON-objekter for nem inspektion.
*   **Klar til SFMC:** Kan indsættes direkte i en Salesforce Marketing Cloud Page.


## Forudsætninger

*   En moderne web-browser med JavaScript aktiveret.
*   Internetforbindelse for at kunne tilgå DAWA API'erne.

## Test

1.  **Åbn i Browser:**
    *   Åbn `dawa.html` filen direkte i din web-browser.

Autocomplete:
  
![Eksempel på DAWA Autocomplete](https://raw.githubusercontent.com/dondv/DAWA-Autocomplete-Adgangsadresse-Detaljer/main/DAWA.jpg)

Output:

![Eksempel på DAWA Autocomplete](https://raw.githubusercontent.com/dondv/DAWA-Autocomplete-Adgangsadresse-Detaljer/main/dawa2.jpg)
## Anvendelse

1.  **Indtast Adresse:** Begynd at skrive en dansk adresse i inputfeltet (f.eks. "Rådhuspladsen 1, København").
2.  **Vælg Forslag:** En liste med adresseforslag vil dukke op under inputfeltet. Klik på det ønskede forslag.
3.  **Se Resultater:**
    *   "Valgt fra Autocomplete": Viser det dataobjekt, der blev returneret direkte fra autocomplete-søgningen.
    *   "Detaljer for Adgangsadresse": Efter et kort øjeblik (mens data hentes), vises de fulde oplysninger for den valgte adgangsadresses ID, inklusive `historik` arrayet, hvis tilgængeligt.

## Vigtige DAWA API Parametre Anvendt

### Autocomplete (`/autocomplete`)

*   `q`: Søgestrengen fra brugeren.
*   `type: 'adresse'`: Specificerer at vi søger efter fulde adresser.
*   `fuzzy: ''`: Aktiverer fuzzy søgning med standardindstillinger.
*   `caretpos`: Brugerens cursorposition for bedre relevans.
*   `per_side`: Antal resultater.

### Adgangsadresse Opslag (`/adgangsadresser/{id}`)

*   `{id}`: ID'et for den valgte adgangsadresse (hentet fra autocomplete-resultatet).
*   `historik: 'true'`: Anmoder om at inkludere historiske versioner af adgangsadressen.
*   `struktur: 'nestet'`: Returnerer data i en nestet JSON-struktur, hvilket kan være nemmere at parse.

## Integration i Salesforce Marketing Cloud (SFMC)

Koden kan indsættes direkte i en "HTML Paste" eller "Code Snippet" Cloud Page i SFMC.

*   **HTTPS:** Alle API-kald til DAWA sker via HTTPS, hvilket er et krav for eksterne kald fra SFMC Cloud Pages.
*   **Content Security Policy (CSP):** Hvis din SFMC-instans har en striks CSP, skal du muligvis whiteliste `https://api.dataforsyningen.dk` i `connect-src` direktivet for at tillade `fetch`-kald.

## Mulige Forbedringer

*   Mere brugervenlig præsentation af adressedetaljer og historik (i stedet for kun rå JSON).
*   Implementering af keyboard-navigation i forslagslisten.
*   Udvidet fejlhåndtering og visuel feedback til brugeren.
*   Mulighed for at konfigurere autocomplete-parametre (f.eks. `type`) via UI.
*   Funktionalitet til at gemme/anvende den valgte adresse (f.eks. sende den til en backend eller gemme i localStorage).

## Licens

Dette projekt er licenseret under MIT Licensen 

## Kreditering

Data leveres af **Datafordeleren** og **Styrelsen for Dataforsyning og Infrastruktur (SDFI)** via **Danmarks Adressers Web API (DAWA)**. Besøg [DAWA Dokumentation](https://dawadocs.dataforsyningen.dk/) for mere information.
