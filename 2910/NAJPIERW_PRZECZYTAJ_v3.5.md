# TABAL Lead Hunter 3.5: zmiany według uwag z 23.10.2026

Wydanie z 28 października 2026. To aktualizacja wersji serwerowej 3.4 (bez logowania). Dane z wersji 3.1–3.4 wczytują się automatycznie. Statusy, notatki, historia, projekty, kampanie i szkice zostają bez zmian.

Źródło: e-mail J. Moraczewskiego „Uwagi po spotkaniu z dnia 22.10” (23.10.2026) i wiadomość na WhatsApp o automatycznej wysyłce SMTP (28.10).

## Uwagi TABAL i co zrobiono

### 1. Plik JSON z GPT 6.1 Sol nie wczytał się po pracy na GPT 5.6 Terra
- **Przyczyna:** import 3.4 przyjmował tylko jeden ściśle określony format. Modele zapisują wynik różnie, np.:
  - `"schema_version": "2"` jako tekst,
  - tekst albo znaczniki ```` ``` ```` wokół JSON,
  - pole `companies` / `results` / `data.leads` zamiast `leads`,
  - źródła jako lista adresów, kontakt jako pojedynczy obiekt, polskie nazwy kryteriów,
  - plik ucięty w połowie rekordu.
  Do tego `batch_id` z innej partii albo z innego czatu blokował cały import.
- **Teraz import jest tolerancyjny.** Wszystkie powyższe warianty wczytują się. Z uciętego pliku aplikacja odzyskuje kompletne rekordy. Działa też format JSON Lines i kilka bloków JSON w jednej odpowiedzi.
- **Podgląd importu pokazuje każdą poprawkę**, np. „Ujednolicono: źródła zapisane jako tekst (12)”, oraz ostrzeżenia, np. „Nieznany batch_id”. Ostrzeżenie pojawia się też wtedy, gdy ta partia była już zaimportowana, bo jest to np. wynik innego modelu. Firmy są wtedy scalane, a duplikaty nie zwiększają licznika.
- Nic nie zapisuje się bez kliknięcia „Zapisz leady w Hunterze”.
- **Prośba do TABAL:** przyślijcie plik z Sol, który się nie wczytał. Sprawdzę go na 3.5, a jeśli będzie trzeba, dopiszę go do testów.

### 2. GPT 5.6 Terra znajduje zbyt mało firm (Sol daje więcej nawet na średnim rozumowaniu)
- **Wybór modelu przy kampanii** (Odkrywanie → „Model w ChatGPT Work”). Zalecany jest GPT-6.1 Sol.
- **Terra = tryb wyczerpujący.** W zadaniu Work musi:
  - wykonać wszystkie zapytania z planu,
  - dla każdej miejscowości sprawdzić wyszukiwarkę / mapy, jeden katalog firm i CEIDG,
  - przed oddaniem pliku porównać wykonane zapytania z planem.
  Po wybraniu Terry panel proponuje też 3 zadania równoległe (mniejszy obszar na jedno zadanie).
- **Pokrycie planu po imporcie**, np. „wykonane zapytania: 18 z 140 (13%)”. Poniżej 50% panel ostrzega, że wynik jest niepełny.
- **Zadanie uzupełniające obejmuje tylko niesprawdzone miejscowości**, czyli te bez wykonanych zapytań albo wpisane przez Work w `remaining_areas`. Nie powtarza całego planu.
- **Raport pilota: „Porównanie modeli Work”.** Pokazuje importy, nowe firmy, firmy ≥ próg na zadanie i % wykonanych zapytań dla każdego modelu. Model pochodzi z pliku (nowe pole `model`) albo z wyboru przy kampanii.
- Uczciwie: aplikacja nie zmieni tego, ile firm znajdzie sam model. Dopilnowuje za to, żeby słabszy model nie kończył po kilku firmach bez śladu, i pokazuje dokładnie, co pominął.

### 3. Link do katalogu produktu w wiadomościach
- Produkty → Edytuj → **„Link do katalogu (PDF lub strona)”**. Link trafia automatycznie do każdej wiadomości o tym produkcie, przed pozdrowieniem, np. „Katalog — Ogrodzenia aluminiowe: https://…”.
- Gdy pole jest puste, link prowadzi do strony produktu na tabal.pl.
- Włącza się i wyłącza w ustawieniach, tam też zmienisz etykietę.
- **Do uzupełnienia przez TABAL:** adresy katalogów PDF. Nie wpisywałem ich na ślepo.

### 4. Wiadomość testowa nie przeszła na porcie 465 ani 993
- **Port 993 to IMAP (odbiór poczty), nie wysyłka.** Panel odrzuca go z wyjaśnieniem: do wysyłki służy 465 (SSL/TLS) albo 587 (STARTTLS). Szyfrowanie dobiera się samo do portu.
- Najczęstsze powody błędu na 465 w wersji 3.4:
  - host nie był dopuszczony w `TABAL_SMTP_HOSTS`, więc panel odrzucał zapis,
  - certyfikat serwera jest wystawiony na nazwę serwera hostingu, a nie na mail.tabal.pl.
- **„Wykryj ustawienia automatycznie”** sprawdza dla domeny nadawcy rekordy MX, mail./smtp., nazwę z certyfikatu i porty 465/587. Rozpoznaje też Microsoft 365, Google i OVH, a po podaniu hasła sprawdza logowanie.
- **„Sprawdź połączenie (i wyślij test)”** przechodzi krok po kroku przez: DNS → port → certyfikat → logowanie dostępne → logowanie → wiadomość testowa na dowolny adres. Przy każdym błędzie panel podaje, co poprawić, a przy złym certyfikacie proponuje właściwą nazwę hosta (przycisk „Zastosuj podpowiedź”).
- **Hosty bez udziału administratora:**
  - hosty w domenie nadawcy (np. mail.tabal.pl),
  - znani dostawcy (home.pl, nazwa.pl, cyber_Folks, OVH, Microsoft 365, Google…),
  - hosty znalezione przez wykrywanie.
  `TABAL_SMTP_HOSTS` działa jak dotąd. `TABAL_SMTP_ALLOW_ANY=1` zdejmuje ograniczenie.
- **Ustawienia przetrwają restart** (plik `smtp.json` w katalogu danych). Hasło jest zapisywane tylko po zaznaczeniu „Zapamiętaj hasło na serwerze” (uprawnienia 600).
- **Opcjonalnie: kopia w folderze „Wysłane”** (IMAP, port 993 — tu ten port się przydaje) albo kopia UDW na adres nadawcy.

### 5. Automatyczna sygnatura e-mail
- Profil i źródła → Wiadomości i podpis → **„Sygnatura firmowa dodawana automatycznie”**:
  - wersja tekstowa,
  - wersja HTML z logo i formatowaniem — wystarczy **wkleić (Ctrl+V) sygnaturę skopiowaną z e-maila** w Outlooku, Gmailu albo Thunderbirdzie, albo **wczytać plik** .htm (Outlook: %APPDATA%\Microsoft\Signatures; polskie znaki z kodowania Windows-1250 są obsługiwane),
  - podgląd.
- HTML jest oczyszczany: bez skryptów, bez zdarzeń `on…` i bez linków `javascript:`.
- Każda wysłana wiadomość ma sygnaturę na końcu. Jeśli jest sygnatura HTML, wiadomość wychodzi w dwóch wersjach: tekstowej i HTML.
- Stary podpis na końcu szkiców (np. „Zespół TABAL”) jest zastępowany, a nie dublowany. Szkice lokalne i szkice z Work kończą się teraz słowem „Pozdrawiam”.

### 6. Wybór właściwego kontaktu (np. zaopatrzeniowiec)
- Produkty → **„Osoby właściwe dla produktu”**. Domyślnie dla ogrodzeń: zaopatrzeniowiec, zaopatrzenie, zakupy, kierownik zakupów, właściciel, kierownik handlowy. Profile, balustrady, hurtownia, tablice i waterjet mają własne listy.
- Kontakt z pasującą funkcją (także adres działu typu zakupy@, zaopatrzenie@) zostaje **głównym odbiorcą**. Na liście i karcie widać „✉ Główny odbiorca wiadomości” i „rola: zaopatrzeniowiec”.
- Na karcie można **ręcznie ustawić głównego odbiorcę** albo wrócić do wyboru automatycznego.
- Zadanie dla Work każe szukać w pierwszej kolejności osób na tych stanowiskach i wpisywać ich funkcję w `role`.

### 7. Automatyczne odznaczanie firm
- Po zmianie statusu (zbiorczej, z karty, archiwizacji, usunięcia, przywrócenia) firma odznacza się na liście. Komunikat podaje, ile firm odznaczono.
- Po dodaniu do kolejki wysyłki zaznaczenie też jest czyszczone.

### 8. Wyniki spoza promienia (np. Bielawa przy „Warszawa +50 km”)
- **Przyczyna:** aplikacja nie sprawdzała adresu firm zwróconych przez Work.
- **Teraz liczy odległość po miejscowości i kodzie pocztowym** (rejestr miast i gmin TERYT, z tolerancją 10%). Oddział w obszarze wystarcza.
- Przy strategii wojewódzkiej sprawdza województwo.
- Firma poza obszarem:
  - region = NIEZGODNE,
  - kara −50 pkt (do ustawienia),
  - blokada kwalifikacji,
  - przy imporcie trafia do archiwum razem z wynikami poniżej progu.
  Podgląd importu wymienia te firmy, np. „Bielawa (woj. dolnośląskie) — ok. 349 km od Warszawa”.
- Karta firmy ma kafel „Obszar strategii (adres)”, a lista leadów filtr „Poza obszarem strategii”.
- **Bez fałszywych odrzuceń.** Wieś o tej samej nazwie z kodem pocztowym z właściwego regionu (np. Bielawa 05-520 k. Konstancina) dostaje „nie sprawdzono — sprawdź adres”, bez kary.
- Zadanie dla Work wymaga adresu z kodem pocztowym. Zabrania dodawania firm, które tylko „obsługują” obszar, i firm z miejscowości o tej samej nazwie w innym województwie.

### 9. Ręczna wysyłka ok. 120 wiadomości zajmowała ok. 6–7 godzin
**Kreator wysyłki do wielu firm:**
1. Leady → filtr „Gotowe do wysyłki (jest e-mail)” → zaznacz firmy (do 100 na stronie) → **„✉ Wyślij e-mail do zaznaczonych”**.
2. Kreator dla każdej firmy:
   - wybiera odbiorcę według roli,
   - przygotowuje brakujący szkic (powód kontaktu, jeden element oferty, pytanie),
   - dodaje link do katalogu i sygnaturę,
   - pokazuje podgląd i pomija firmy, do których nie można pisać (podaje powód: brak e-maila, już wysłano, blokada…).
3. Adresy „do weryfikacji” można świadomie zatwierdzić jedną opcją. Decyzja zapisuje się w historii każdej firmy.
4. **„Zatwierdź i wyślij”:**
   - firmy dostają status DO KONTAKTU,
   - wiadomości trafiają do kolejki (do 300 w jednym kroku, więcej w kolejnych),
   - wysyłka rusza od razu i **działa także po zamknięciu panelu**,
   - po przyjęciu wiadomości przez SMTP status zmienia się na SKONTAKTOWANY, z wpisem w historii.

**Bezpieczeństwo kolejki:**
- **Przed każdą wiadomością serwer porównuje ją z aktualnymi danymi firmy.** Zmieniony status, adres albo treść = wiadomość anulowana, nie wysłana.
- Odstęp od 10 s (domyślnie 20 s: 120 wiadomości ≈ 40 min bez pracy ręcznej) i **limit dobowy** (domyślnie 200; po jego osiągnięciu kolejka czeka sama).
- Panel pokazuje: liczbę w kolejce, szacowany czas, wysłane w ciągu 24 h, „Anuluj oczekujące”.
- Odświeżenie strony albo druga osoba z otwartym panelem nie blokuje już kolejki (w 3.4 przez 15 s pojawiał się komunikat „Kolejkę obsługuje inna karta”). Karty w tle tylko podglądają kolejkę, a kliknięcie „dodaj / uruchom” przejmuje ją bezpiecznie.
- Zapis danych jest możliwy w trakcie wysyłki (w 3.4 kolizja z wysyłaną wiadomością dawała błąd zapisu).
- Na karcie firmy jest przycisk „✉ Wyślij tej firmie (kreator)”.

## Nowe ustawienia
- **Wiadomości i podpis:**
  - sygnatura tekstowa i HTML, automatyczna sygnatura (tak),
  - link do katalogu (tak) i jego etykieta,
  - domyślne osoby właściwe,
  - wysyłka na adresy zatwierdzone przez operatora (tak).
- **Produkty:** link do katalogu i osoby właściwe.
- **Scoring:** kara za adres poza obszarem (50).
- **Wysyłka:**
  - odstęp i limit dobowy,
  - kopia w „Wysłanych” (IMAP) albo kopia UDW,
  - zapamiętanie hasła.

## Aktualizacja z 3.4
1. W 3.4 pobierz **Kopię JSON**, zatrzymaj serwer, zachowaj katalog `.runtime` (albo wolumen Docker) i plik `.env`.
2. Rozpakuj 3.5 i przenieś `.runtime` i `.env` do nowego katalogu. W Dockerze: ten sam projekt Compose i wolumen, `docker compose up -d --build`, **bez** `down -v`.
3. Uruchom serwer. W menu ma być „WERSJA 3.5”, a `/healthz` ma zwracać `3.5.0`.
4. Wysyłka → wpisz hasło → „Wykryj ustawienia” → „Sprawdź połączenie” z adresem do testu → „Zapisz”. Zaznacz „Zapamiętaj hasło”, jeśli tak zdecydujecie.
5. Profil i źródła: wklej sygnaturę i uzupełnij linki do katalogów w Produktach.
6. Uwaga: leady z 3.4 z adresem poza obszarem swojej strategii pokażą teraz karę i blokadę. Nie są archiwizowane automatycznie — widać je w filtrze „Poza obszarem strategii”.

## Testy
- **Logika:** 142 testy (w tym 15 nowych dla uwag z 23.10).
- **Serwer i SMTP:** 64 testy, w tym 17 nowych:
  - porty,
  - diagnostyka na prawdziwym lokalnym serwerze TLS: certyfikat dla innej nazwy, zła para port/szyfrowanie, logowanie i test,
  - wysyłka po zamknięciu panelu z ponownym sprawdzeniem danych,
  - limit dobowy,
  - HTML,
  - kopia IMAP,
  - zapis ustawień,
  - przejęcie kolejki.
- **Przeglądarka:** 33 testy.
- **Pełny scenariusz wysyłki** (`npm run test:e2e-mail`) na lokalnym serwerze SMTP z TLS:
  1. ustawienie katalogu i sygnatury HTML w panelu,
  2. port 993 odrzucony z wyjaśnieniem,
  3. „Sprawdź połączenie” z wiadomością testową,
  4. zaznaczenie 3 firm i kreator,
  5. zamknięcie panelu w trakcie wysyłki.

  Wynik: 3 wiadomości wysłane. Zaopatrzeniowiec dostał wiadomość zamiast biura, w każdej jest link do katalogu, sygnatura i wersja HTML, a statusy zmieniły się na SKONTAKTOWANY. Przykładowa wiadomość: `test-results/e2e-mail-v35-przyklad.eml`.

## Granice
- Nie testowano na rzeczywistej skrzynce bok@tabal.pl ani na prawdziwych plikach z Work (Terra / Sol). Formaty importu odtworzono na podstawie typowych odchyleń modeli.
- Wykrywanie ustawień zależy od DNS i otwartych portów serwera aplikacji. Jeśli dostawca VPS blokuje wychodzący SMTP, panel to pokaże, ale tego nie obejdzie.
- Microsoft 365: logowanie hasłem SMTP (Basic Auth) Microsoft wyłącza domyślnie od końca 2026 r. Jeśli TABAL jest na M365, trzeba to zaplanować (włączenie przez administratora albo OAuth).
- Przyjęcie wiadomości przez SMTP nie oznacza doręczenia. Odpowiedzi i odbicia trafiają do skrzynki bok@.
