# Kwadrat Studio – sandbox Akademii Integratora

Jednostronicowa strona fikcyjnej firmy wykończeniowej „Kwadrat Studio”. Służy jako środowisko do nagrań: podpinanie GTM, konwersje z formularzy, zbieranie UTM i zdarzenia `dataLayer`.

Jeden plik `index.html` (HTML + Tailwind z CDN + czysty JS). Bez buildu, działa na GitHub Pages, VPS albo po otwarciu z dysku.

## Gdzie wkleić GTM

- `<!-- GTM HEAD PLACEHOLDER -->` na górze `<head>`: fragment `<script>` kontenera.
- `<!-- GTM BODY PLACEHOLDER -->` zaraz po `<body>`: fragment `<noscript>`.

## Formularze

| Formularz | `id` | Zdarzenie `dataLayer` | Pola w zdarzeniu |
|---|---|---|---|
| Szybka wycena w 24h (hero) | `quick-lead-form` | `generate_lead_quick` | `form_type: quick_estimate`, `apartment_size`, `finish_standard`, `user_email`, `user_phone` |
| Konsultacja z architektem | `contact-form` | `generate_lead_contact` | `form_type: architect_consultation`, `handover_date`, `user_email`, `user_phone` |

Oba zdarzenia dostają też `utm_source`, `utm_medium`, `utm_campaign`, `utm_content`, `utm_term`, `gclid`, `fbclid` (gdy są w sesji).

Wysyłka działa bez przeładowania strony. Formularz chowa się, pokazuje podziękowanie i przycisk „Wypełnij ponownie (reset do testów)”. Żadne dane nie trafiają na serwer.

Wartości selectów:

- `apartment_size`: `do_45`, `45_70`, `70_100`, `powyzej_100`
- `finish_standard`: `komfort`, `premium`, `indywidualny`
- `handover_date`: `juz_odebrane`, `1_3_miesiace`, `ponad_3_miesiace`

## UTM i click ID

Przy wejściu skrypt czyta z adresu `utm_source`, `utm_medium`, `utm_campaign`, `utm_content`, `utm_term`, `gclid`, `fbclid`, zapisuje je w `sessionStorage` (klucz `kwadrat_attribution`) i wpisuje do ukrytych pól obu formularzy. Nowe parametry w URL nadpisują zapisane.

Adres testowy:

```
index.html?utm_source=facebook&utm_medium=cpc&utm_campaign=wykonczenia_poznan&utm_content=premium_video&fbclid=TEST123
```

## Pasek „Tryb Demo Akademii”

Na dole ekranu:

- **Wypełnij testowymi danymi**: uzupełnia oba formularze (Jan Kowalski, jan.kowalski@example.com, +48 500 600 700, 45 – 70 m², Premium, zgoda RODO).
- **Podgląd dataLayer**: pokazuje ostatnie zdarzenia wysłane przez formularze (to samo trafia do konsoli).
- Obok nazwy trybu widać atrybucję zapisaną w bieżącej sesji.

Strona ma `noindex, nofollow`, żeby nie trafiła do wyszukiwarki.
