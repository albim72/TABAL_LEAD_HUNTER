# TABAL Lead Hunter 3.7: poprawki według uwag z 2.11.2026

Wydanie z 2 listopada 2026. To aktualizacja wersji 3.6 (serwer, bez logowania). Dane z 3.1–3.6 wczytują się automatycznie. Statusy, historia, projekty i szkice zostają bez zmian.

Źródło: e-mail J. Moraczewskiego „Uwagi do Lead Huntera z dnia 2.11” (2.11.2026, 12:20).

## Uwagi TABAL i co zrobiono

### 1. Kraj, miasto, województwo i obszar tylko w strategii
- W **Odkrywaniu** nie ma już pól kraju, województwa, miasta, promienia ani własnych miejscowości.
- Formularz kampanii pokazuje obszar ze strategii (tylko do odczytu) i przycisk **„Zmień obszar w strategii”**.
- Obszar zmienia się wyłącznie w **Strategie → Edytuj**, więc kampania nie nadpisze strategii przypadkiem.

### 2. Profile osób — każdy ma swoją sygnaturę
- Nowa zakładka **Profil i źródła → Profile osób**. Profil zawiera:
  - imię i nazwisko, stanowisko, e-mail,
  - nazwę w polu „Od:” (np. „Jędrzej Moraczewski | TABAL”),
  - **własną sygnaturę**: kreator albo wklejenie z poczty, z logo.
- **Wybór osoby:** u góry ekranu jest pole **„Podpisuję jako”**. Każda przeglądarka (każdy komputer) pamięta swoją osobę.
- **Pierwsza wiadomość** dostaje sygnaturę osoby, która ją wysyła. Na karcie firmy widać, kto podpisał.
- **Przypomnienie** podpisuje ta sama osoba, która wysłała pierwszą wiadomość. Rozmowa z firmą prowadzi więc jedna osoba.
- **Adres nadawcy** pozostaje wspólny (skrzynka z zakładki Wysyłka, np. bok@tabal.pl). Odbiorca widzi w „Od:” nazwisko z profilu.
- **Bez profili** wszystko działa jak w 3.6: z firmową sygnaturą.

### 3. Wysyłka do osoby i na jeden adres ogólny
- **Nowe ustawienie** (Profil i źródła → Wiadomości i podpis → „Odbiorcy wiadomości”):
  - **Osoba właściwa + DW na jeden ogólny adres firmy** (domyślnie), np. „Do: zaopatrzeniowiec”, „DW: biuro@”;
  - tylko osoba właściwa (jak w 3.6);
  - tylko jeden ogólny adres firmy.
- **Kilka adresów ogólnych:** wybierany jest **jeden**. Najpierw biuro@ / info@ / kontakt@ / handel@ / zakupy@ z domeny firmy. Adresy serwis@, rekrutacja@, faktury@ i podobne są pomijane.
- **Tylko adres ogólny:** wiadomość idzie wyłącznie na niego.
- **Do sprawdzenia przez TABAL:** zdanie z e-maila można odczytać na dwa sposoby, dlatego tryb jest wyborem w ustawieniach. Prośba o potwierdzenie, który ma być domyślny.

### 4. Przypomnienie po 3 dniach bez odpowiedzi
- **Kiedy:** firma ze statusem SKONTAKTOWANY nie odpowiedziała przez **3 dni** (ustawienie 0–60 dni; 0 = wyłączone). Wtedy na każdej stronie pojawia się pomarańczowe powiadomienie: **„3 firmy nie odpowiedziały od 3 dni — Wyślij przypomnienia →”**.
- **Kreator przypomnień:** Wysyłka → „Przypomnienia do wysłania (N)”. Przycisk na karcie firmy zmienia się wtedy na „Wyślij przypomnienie”.
- **Treść:** krótka i do edycji w ustawieniach, np. „uprzejmie przypominam o mojej wiadomości z 29.10.2026 w sprawie: …”. Pod spodem jest cytat pierwszej wiadomości.
- **Jak wychodzi:** jako **odpowiedź w tym samym wątku** (Re:, nagłówek In-Reply-To), do tych samych adresów (Do i DW).
- **Liczba:** domyślnie **1 przypomnienie** na firmę (ustawienie 1–3). Kolejne przypomnienie liczy się od ostatniej wysyłki.
- **Wykrywanie odpowiedzi:** aplikacja zagląda do **Odebranych** (IMAP, tylko odczyt — nic nie oznacza jako przeczytane). Szuka wiadomości od domeny firmy, a przy Gmailu, WP, Onecie itp. — od dokładnego adresu.
  - Odpowiedź po naszej wiadomości daje status **ODPOWIEDŹ** i zatrzymuje przypomnienia.
  - Automatyczna odpowiedź („jestem na urlopie”) jest tylko zapisywana w historii.
  - Sprawdzanie: przy wejściu na stronę Wysyłka (co 15 min) oraz przyciskiem **„Sprawdź odpowiedzi w poczcie”**.
  - Wymaga działającego IMAP, tego samego co kopia w „Wysłanych”.
- **Karta firmy** ma nową sekcję **Korespondencja**: wysłane wiadomości (z DW), przypomnienia, odpowiedzi i informację, od kiedy można wysłać przypomnienie.
- **Filtry na liście:** „Przypomnienie do wysłania” i „Odpowiedziały (wykryto w poczcie)”.

### 5. Czas wyszukiwania
- **Czas na zadanie:** w Odkrywaniu jest nowe pole **„Czas na zadanie w Work”**: bez limitu, ok. 15 / 30 / 60 / 120 minut. Work kończy po tym czasie i zwraca to, co potwierdził. Niesprawdzone miejscowości trafiają do zadania uzupełniającego.
- **Historia kampanii** ma kolumnę **„Czas Work”**: od przygotowania zadania do importu wyniku.
- **Jak skrócić czas bez utraty wyników:** zadania równoległe (2–6). Każde w osobnym czacie Work, uruchomione w tym samym czasie.
- **Do sprawdzenia przez TABAL:** nie wiemy na pewno, czy chodziło o to, że wyszukiwanie trwa za długo, czy o możliwość ustawienia czasu. Prośba o doprecyzowanie, jeśli chodziło o coś innego.

### 6. „Wyślij tej firmie (kreator)” zmienia status na DO KONTAKTU
- Kliknięcie od razu ustawia status **DO KONTAKTU** (z wpisem w historii) dla firm o statusie NOWY, DO WERYFIKACJI lub ZAAKCEPTOWANY.
- Dla firmy SKONTAKTOWANY z przypomnieniem do wysłania przycisk otwiera kreator przypomnień.

### 7. Grafiki w sygnaturze
- **Przyczyny:**
  1. Outlook i Thunderbird kopiują logo jako lokalny załącznik (`cid:`, `file:`), którego nie da się przenieść.
  2. Logo z adresu internetowego część programów blokuje („pokaż obrazy”).
  3. Podgląd w panelu blokował obrazy z internetu.
- **Teraz:**
  - **„Logo z pliku”** w kreatorze sygnatury (firmowej i w profilu): PNG/JPG do 300 kB. To najpewniejsza droga.
  - **„Osadź obrazy”**: serwer pobiera raz logo z adresu http(s) i zapisuje je w sygnaturze. Po wklejeniu sygnatury z poczty dzieje się to automatycznie.
  - Obrazy z Outlooka, których nie da się przenieść, są wykrywane i panel to mówi: „wczytaj logo z pliku”.
  - **Wysyłka:** obrazy idą jako załączniki w treści (`cid:`), więc logo widać u odbiorcy od razu — w Outlooku, Gmailu i Thunderbirdzie.
  - **Podgląd w panelu** pokazuje obrazy.

### 8. Krótka nazwa firmy w wiadomości
- W temacie i treści zostaje tylko główna część nazwy, np. „ASTOR Sp. z o.o.” → **„ASTOR”**, „P.P.H.U. "DREWMAX" Jan Kowalski” → **„DREWMAX”**, „Meble Kowalski Sp. z o.o. Sp. k.” → **„Meble Kowalski”**.
- Działa także dla szkiców już zapisanych i szkiców z Work: pełna nazwa i nazwa z KRS są zamieniane przy wysyłce.
- Zadanie dla Work każe pisać krótką nazwę.

### 9. Testowanie powtarzania
- **Powtarzanie wyszukiwania** (Strategie → Edytuj → „Powtarzanie: co 7/14/30 dni”): gdy przypada termin, powiadomienie u góry pokazuje „powtórzenie wyszukiwania: …”.
- **Powtarzanie wiadomości** to przypomnienia z punktu 4.
- **Scenariusz do przetestowania przypomnień:**
  1. Profil i źródła → Wiadomości i podpis → „Przypomnij po” = **0**, zapisz.
  2. Ustaw np. **1** i wyślij wiadomość do 2–3 firm na własne adresy.
  3. Następnego dnia sprawdź powiadomienie.
  4. Wyślij przypomnienie i odpisz z jednej skrzynki.
  5. „Sprawdź odpowiedzi w poczcie” — firma powinna mieć status ODPOWIEDŹ.

## Aktualizacja z 3.6
1. Poczekaj, aż kolejka wysyłki będzie pusta. Oczekujące wiadomości z 3.6 zostaną anulowane, bo w 3.7 wiadomość może mieć DW i podpis z profilu. Dodasz je ponownie w kreatorze.
2. W 3.6 pobierz **Kopię JSON**, zatrzymaj serwer, zachowaj `.runtime` i `.env`.
3. Rozpakuj 3.7 i przenieś `.runtime` i `.env`. W Dockerze: ten sam projekt i wolumen, `docker compose up -d --build`, **bez** `down -v`.
4. `/healthz` ma zwracać `3.7.0`, a w menu ma być „WERSJA 3.7”.
5. **Pierwsze kroki:**
   - każda osoba: Profil i źródła → **Profile osób** → „+ Dodaj osobę” → sygnatura z logo z pliku → Zapisz (zaznaczone „To ja”);
   - Wysyłka → „Sprawdź folder Wysłane” (IMAP potrzebny też do wykrywania odpowiedzi).

## Testy
- **Logika:** 162 testy, w tym 8 nowych dla uwag z 2.11:
  - krótka nazwa firmy (10 przykładów),
  - Do + DW, tryby odbiorców,
  - profile i sygnatury,
  - logo z pliku,
  - przypomnienia: termin, limit, Re:, cytat, wyłączenie,
  - wykryte odpowiedzi i autoodpowiedzi,
  - ustawienia i czas Work.
- **Serwer, SMTP i IMAP:** 85 testów, w tym 10 nowych:
  - DW i nagłówki In-Reply-To,
  - osobny klucz kolejki dla przypomnień,
  - nazwa nadawcy z profilu,
  - wyszukiwanie odpowiedzi w Odebranych (tylko odczyt, domena / dokładny adres, data po naszej wiadomości, autoodpowiedź),
  - osadzanie obrazów (limit 500 kB, odrzucenie adresów prywatnych),
  - pełny przepływ przypomnienia i odpowiedzi przez serwer.
- **Przeglądarka:** 33 testy.
- **Pełny scenariusz wysyłki** na lokalnym serwerze SMTP z TLS. Wszystkie kroki przeszły:
  1. 3 pierwsze wiadomości (dla firmy z osobą: Do zaopatrzeniowiec + DW biuro@),
  2. profil „Anna Testowa” z logo z pliku,
  3. przesunięcie dat o 4 dni,
  4. powiadomienie „3 firmy nie odpowiedziały od 3 dni”,
  5. 3 przypomnienia jako Re: z In-Reply-To, „Od: Anna Testowa | TABAL”, podpisem i logo w treści (`cid:`),
  6. powiadomienie znika po wysłaniu.

  Przykładowa wiadomość: `test-results/e2e-mail-v37-przyklad.eml`.

## Granice
- **Prawdziwa skrzynka:** nie testowano na bok@tabal.pl. Wykrywanie odpowiedzi i kopia w Wysłanych wymagają IMAP (port 993) dostępnego z serwera aplikacji.
- **Dopasowanie odpowiedzi:**
  - wykrywane są wiadomości z domeny firmy po dacie naszej wysyłki. Mail od innej osoby z tej firmy też liczy się jako odpowiedź;
  - odpowiedź z prywatnego adresu spoza domeny nie zostanie dopasowana — wtedy ustaw status ręcznie.
- **„Podpisuję jako”** to wybór w przeglądarce, nie logowanie. Wersja bez logowania nie sprawdza, kim jest osoba przy komputerze.
