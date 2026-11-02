# TABAL Lead Hunter 3.6: poprawki według uwag z 29–30.10.2026

Wydanie z 1 listopada 2026, na spotkanie 2.11 o 8:00. To aktualizacja wersji 3.5 (serwer, bez logowania). Dane z 3.1–3.5 wczytują się automatycznie. Statusy, notatki, historia, projekty i szkice zostają bez zmian.

Źródło: uwagi J. Moraczewskiego z 29.10 (WhatsApp „AI dla TABAL”), przykład wiadomości z 30.10 (ASTOR Sp. z o.o.) i pytanie P. Frankiewicza z 30.10.

## Uwagi TABAL i co zrobiono

### 1. „Nadal w mailach nie wyświetla się sygnatura z poczty”
- **Przyczyna:** aplikacja nie ma dostępu do sygnatury ustawionej w programie pocztowym. Outlook, Thunderbird i poczta w przeglądarce dodają sygnaturę po swojej stronie, a Lead Hunter wysyła przez serwer SMTP z pominięciem programu pocztowego. W ustawieniach nie było wklejonej sygnatury, więc aplikacja wstawiała domyślny podpis „Zespół TABAL”. Tak wyglądał przykład z 30.10.
- **Teraz:**
  - **Kreator sygnatury** (Profil i źródła → Wiadomości i podpis). Wpisujesz imię i nazwisko, stanowisko, telefon, opcjonalnie adres logo, a kreator tworzy wersję HTML i wersję tekstową. Dane firmy są już wpisane: TABAL Sp. z o.o., ul. Energetyków 14, 20-468 Lublin, bok@tabal.pl, tabal.pl.
  - Sygnaturę z Outlooka lub Thunderbirda nadal można wkleić (Ctrl+V) albo wczytać z pliku.
  - **Bez prawdziwej sygnatury wysyłka jest wstrzymana.** Na stronie Wysyłka i w ustawieniach widać czerwony komunikat z przyciskiem „Ustaw sygnaturę”. Wymóg można świadomie wyłączyć.
  - Logo wklejone razem z sygnaturą (obrazek osadzony w treści) jest wysyłane jako załącznik w treści wiadomości (`cid:`). Gmail i Outlook nie pokazują obrazków osadzonych w inny sposób.

### 2. Wygląd wiadomości (przykład z 30.10)
- **Przyczyna:** wiadomość bez sygnatury HTML wychodziła wyłącznie jako zwykły tekst. Thunderbird i część poczt w przeglądarce pokazują taki tekst czcionką maszynową, stąd wygląd na zrzucie ekranu.
- **Teraz każda wiadomość wychodzi w dwóch wersjach:** HTML (zwykła czcionka, klikalne linki, sygnatura) i tekstowej (dla starszych programów pocztowych). W kreatorze wysyłki podgląd pokazuje wiadomość tak, jak zobaczy ją odbiorca.

### 3. „Program nadal wybiera pierwszy lepszy kontakt z firmy”
- **Przyczyna:** gdy żaden kontakt nie pasował do listy „osoby właściwe” produktu, aplikacja wybierała pierwszą osobę z nazwiskiem, np. inżyniera aplikacji, zamiast działu zakupów lub sprzedaży.
- **Teraz program sam ustala, do kogo pisać, w tej kolejności:**
  1. odbiorca wskazany ręcznie na karcie,
  2. osoby właściwe dla produktu, w kolejności z listy,
  3. zakupy / zaopatrzenie (także adresy zakupy@, zaopatrzenie@),
  4. dział handlowy / sprzedaż,
  5. właściciel / zarząd,
  6. ogólny adres firmy (biuro@, info@),
  7. produkcja / technologia (chyba że produkt ma je na liście, np. konstruktor przy profilach),
  8. osoba o nieznanej funkcji.

  Marketing, kadry, rekrutacja, księgowość i serwis trafiają na sam koniec.
- Na karcie firmy i w kreatorze widać **„dlaczego”**, np. „rola: zakupy”. Gdy w firmie nie ma osoby z zakupów ani ze sprzedaży, pojawia się żółte ostrzeżenie: „biuro / adres ogólny — brak osoby z zakupów/sprzedaży”.
- Zadanie dla Work każe szukać osób w tej samej kolejności i zawsze wpisywać ich funkcję. Bez funkcji program nie wie, do kogo pisać.

### 4. „Poznań +50 km — program wyszukał firmy z Rudy Śląskiej czy Gliwic”
- **Przyczyny:**
  1. W 3.5 do obszaru wystarczał dowolny oddział, więc firma z Rudy Śląskiej z biurem w Poznaniu przechodziła.
  2. Zadanie dla Work kazało wpisywać jako główny adres oddział **najbliższy TABAL (Lublin)**, a nie siedzibę. Przy wyszukiwaniu w Poznaniu adres główny pokazywał więc śląski lub krakowski oddział.
- **Teraz:**
  - **Liczy się siedziba firmy** (Scoring i źródła → „Co musi leżeć w obszarze strategii”: „Siedziba firmy” jest ustawieniem domyślnym; „Siedziba albo dowolny oddział” można wybrać).
  - Siedziba to adres z KRS/CEIDG, a gdy go brak — adres główny. Firma z siedzibą poza obszarem dostaje „POZA OBSZAREM”, karę punktową i blokadę kwalifikacji. Przy imporcie trafia do archiwum, a podgląd importu ją wymienia.
  - **Nie da się do niej wysłać wiadomości:** kreator ją pomija i podaje powód.
  - Notatka na karcie podaje szczegóły, np. „Kraków (woj. małopolskie, ok. 334 km od Poznań) — siedziba poza obszarem Poznań +50 km. W obszarze jest tylko oddział (Poznań)”.
  - Gdy program się myli, operator na karcie klika **„Siedziba jest w obszarze”** albo „Poza obszarem”. Decyzja jest zapisywana w historii.
  - Zadanie dla Work każe podawać **adres siedziby**, oddziały wpisywać osobno, a firm z siedzibą poza obszarem nie dodawać. Kontakty z oddziału w obszarze mają pierwszeństwo.
- **Firmy zaimportowane wcześniej** (np. z 29.10) są przeliczane od razu po aktualizacji. Na liście Leady pokazuje je filtr „Poza obszarem strategii”: zaznacz wszystkie → Archiwizuj.

### 5. „Wiadomości dochodzą do firm, ale nie widać ich w folderze Wysłane”
- **Przyczyna:** wiadomości wysyła serwer aplikacji, nie program pocztowy, więc same z siebie nie trafiają do „Wysłanych”. Opcja kopii przez IMAP istniała w 3.5, ale była wyłączona i schowana w zwiniętej sekcji. Błąd zapisu kopii nie był nigdzie widoczny.
- **Teraz:**
  - Kopia w „Wysłanych” jest **włączona domyślnie**. Ustawienia zapisane w 3.5 zostaną przełączone raz, chyba że wybrano kopię UDW.
  - Sekcja „Kopia w folderze Wysłane” jest widoczna w kroku 1 i ma przycisk **„Sprawdź folder Wysłane”**. Przycisk łączy się ze skrzynką (IMAP, port 993, ten sam login i hasło), znajduje folder („Wysłane”, „Sent”, „INBOX.Sent”, „Elementy wysłane”) i **zapisuje w nim wiadomość testową**. Wystarczy zajrzeć do poczty i sprawdzić, czy test tam jest. Jeśli host IMAP jest inny niż SMTP, aplikacja sprawdza też imap.tabal.pl i mail.tabal.pl.
  - W kroku 3 widać stan kopii: „działa ✓” albo **czerwony komunikat**, gdy ostatnia kopia się nie zapisała.

## Na spotkaniu 2.11 — kolejność (ok. 20 min)
1. Wgrać 3.6 (patrz „Aktualizacja” niżej), sprawdzić „WERSJA 3.6” w menu.
2. Profil i źródła → Wiadomości i podpis → wkleić sygnaturę z poczty **albo** wypełnić kreator → Zapisz.
3. Wysyłka → wpisać hasło → „Sprawdź folder Wysłane” → sprawdzić w poczcie wiadomość testową → „Zapisz ustawienia”.
4. Leady → filtr „Poza obszarem strategii” → przejrzeć i zarchiwizować firmy z Poznań +50 km spoza obszaru.
5. Wysłać 2–3 wiadomości na własne adresy i sprawdzić je w Thunderbirdzie, Gmailu i w „Wysłanych”.
6. Uruchomić nowe zadanie Work dla Poznań +50 km (nowa treść zadania: adres siedziby, kolejność ról).

## Aktualizacja z 3.5
1. Zaczekaj, aż kolejka wysyłki będzie pusta. Wiadomości oczekujące z 3.5 zostaną anulowane, bo w 3.6 wiadomość ma też wersję HTML. Dodasz je ponownie w kreatorze.
2. W 3.5 pobierz **Kopię JSON**, zatrzymaj serwer, zachowaj katalog `.runtime` (albo wolumen Docker) i plik `.env`.
3. Rozpakuj 3.6 i przenieś `.runtime` i `.env`. W Dockerze: ten sam projekt i wolumen, `docker compose up -d --build`, **bez** `down -v`.
4. `/healthz` ma zwracać `3.6.0`.

## Testy
- **Logika:** 154 testy, w tym 12 nowych dla uwag z 29.10, m.in.:
  - Poznań +50 km: Ruda Śląska i Gliwice poza obszarem, Swarzędz w obszarze,
  - siedziba w Krakowie z oddziałem w Poznaniu,
  - siedziba z KRS ważniejsza niż adres oddziału,
  - kolejność odbiorców (zakupy przed handlowcem, zarządem, biurem i inżynierem; marketing na końcu),
  - wymóg sygnatury i kreator sygnatury,
  - HTML w każdej wiadomości.
- **Serwer i SMTP/IMAP:** 75 testów, w tym 11 nowych:
  - test folderu Wysłane z polską nazwą (`Wys&AUI-ane`),
  - próba kolejnego hosta IMAP,
  - błędne logowanie,
  - jednorazowe włączenie kopii dla ustawień z 3.5,
  - widoczny błąd kopii,
  - logo jako `cid:`.
- **Przeglądarka:** 33 testy.
- **Pełny scenariusz wysyłki na lokalnym serwerze SMTP z TLS:** 3 wiadomości z katalogiem, sygnaturą i wersją HTML, bez „Zespół TABAL”. Przykładowa wiadomość: `test-results/e2e-mail-v36-przyklad.eml`.

## Granice
- Nadal nie testowano na prawdziwej skrzynce bok@tabal.pl. Kopia w „Wysłanych” wymaga, żeby skrzynka miała włączony IMAP, a serwer aplikacji mógł łączyć się na port 993. Przycisk „Sprawdź folder Wysłane” pokaże, który krok nie działa.
- Siedzibę program ocenia po adresie z KRS/CEIDG albo adresie głównym. Przy starych rekordach (z 3.5) adres główny bywa adresem oddziału — wtedy pomaga przycisk na karcie albo ponowne uzupełnienie danych przez Work.
- Agent dyrektorski to osobny projekt — nie wchodzi w tę paczkę.
