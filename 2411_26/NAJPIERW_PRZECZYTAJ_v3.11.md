# TABAL Lead Hunter 3.11: tryb API bez limitu ChatGPT, koniec z odrzucaniem przez „starą stronę”, jasny koniec puli

Wydanie z 24 listopada 2026. Aktualizacja wersji 3.10. Wszystko, co działało w 3.10, działa bez zmian: e-maile ze stron firm, kolejka z limitem dobowym, profile i wysyłka. Opis 3.10 jest w `docs/NAJPIERW_PRZECZYTAJ_v3.10_HISTORYCZNY.md`.

Źródła uwag: raport J. Moraczewskiego z 20.11 (wielkopolskie, pomorskie), uwagi z 19.11 oraz wiadomości P. Frankiewicza w grupie „AI dla TABAL” (23–24.11).

## Diagnoza: dlaczego 500 maili dziennie, a nie 2000

| Liczby z raportów 19–20.11 | Wartość |
|---|---|
| Jedno wyszukiwanie (100 firm) | 10–26 minut |
| Zużycie 5-godzinnego limitu ChatGPT na jedno wyszukiwanie | 14–46% (średnio ok. 29%) |
| Wyszukiwań na jedno okno limitu | ok. 3 |
| Leady odrzucone przez regułę „stara strona” | 17–23 na każde 100 |
| Wynik dzienny | ok. 500 wiadomości / 7 h |

Wniosek: **sufitem jest limit subskrypcji ChatGPT, a nie sama aplikacja.** Poprawki w aplikacji odzyskują ok. 20% leadów z każdego wyszukiwania (pkt 2). Do 2000+ dziennie potrzebny jest **tryb API** (pkt 1), w którym TABAL płaci za faktyczne zużycie (tokeny) zamiast czekać na odnowienie limitu.

## 1. Tryb API — wyszukiwanie na serwerze, bez limitu ChatGPT (nowe)

**Jak działa:** to samo zadanie, które dotąd kopiowało się do ChatGPT Work, serwer wysyła przez API dostawcy AI. Model robi research z wyszukiwaniem w sieci, a wynik **importuje się sam**: te same reguły, deduplikacja i scoring, a potem automatyczne szukanie e-maili na stronach firm (3.10).

- **Bez 5-godzinnego okna.** Płacicie za tokeny, a limitem jest tylko budżet konta API.
- **Bez kopiowania.** Nie trzeba wklejać zadania ani pobierać i wczytywać JSON-a.
- **Równolegle.** Domyślnie 4 zadania naraz (`TABAL_AI_PARALLEL`, do 12). Przy „Zadania równoległe: 4” obszar dzieli się na 4 części i wszystkie idą jednocześnie.
- **W tle.** Zadania działają dalej po zamknięciu panelu. Wyniki zaimportują się po jego ponownym otwarciu.
- **Kontrola kosztu.** Panel pokazuje czas, tokeny i liczbę wyszukiwań przy każdym zadaniu. Po wpisaniu cennika (`TABAL_AI_PRICE_IN/OUT`) pokazuje też koszt.

**Gdzie:** Odkrywanie → sekcja **„2B · Tryb API”**:
- **„▶ Uruchom bieżące zadanie przez API”**,
- **„▶▶ Uruchom wszystkie zadania kampanii przez API”**,
- przy zadaniach równoległych przycisk **„▶ API”** obok „Kopiuj”.

Pole „Zapisuj wyniki automatycznie” jest domyślnie włączone. Po wyłączeniu każdy wynik najpierw trafia do podglądu.

**Konfiguracja (administrator TABAL):**
1. Załóżcie konto API u dostawcy (np. platform.openai.com) i **doładujcie je**. To osobna płatność, niezależna od subskrypcji ChatGPT Plus / Pro / Business. **Ustawcie miesięczny limit wydatków na koncie API.**
2. W pliku `.env` wpiszcie `TABAL_AI_API_KEY` i `TABAL_AI_MODEL` (model z wyszukiwaniem w sieci, nazwa dokładnie jak w panelu dostawcy). Pozostałe ustawienia są opisane w `.env.example`.
3. Uruchomcie serwer ponownie. W panelu pojawi się „Model: …, klucz …abcd”.
   - Doraźnie klucz można wpisać w panelu. Jest trzymany tylko w pamięci serwera do restartu.
   - Klucz **nigdy** nie trafia do przeglądarki, do kopii JSON ani do logów.

**Pierwszy test:** kampania z pulą 10, „▶ Uruchom bieżące zadanie przez API”. Sprawdźcie czas, tokeny i koszt w tabeli, a dopiero potem uruchamiajcie pełne pule.

**Komunikaty błędów:**
- „API 401”: zły klucz.
- „API 404”: nieznany model.
- „API 429”: limit konta albo **brak środków — doładujcie konto**.
- „limit czasu”: zadanie trwało dłużej niż `TABAL_AI_MAX_MINUTES`.

ChatGPT Work działa dalej jak w 3.10. Oba sposoby można stosować zamiennie, a import jest ten sam.

## 2. „Stara strona” nie odrzuca już leadów (raport 20.11, pkt 3)

**Przyczyna:** reguła „strona nieaktualizowana od ponad 2 lat” w 3.10 robiła trzy rzeczy naraz:
- zabierała wszystkie punkty za źródło i doliczała karę −15 pkt,
- blokowała kwalifikację,
- przez to spychała firmę poniżej progu 65, a lokalny szkic nie powstawał („Brak aktualnego, sprawdzonego źródła”).

Efekt: 22 leady w jednym wyszukiwaniu odrzucone ręcznie.

**Zmiana:**
- **Stara strona to tylko ostrzeżenie na karcie** („Strona firmy nieaktualizowana od 2021 r. — to nie wyklucza leada…”). Nie ma kary ani blokady.
- **Zadanie dla Work / API** mówi wprost, że stara strona NIE wyklucza firmy. Odrzucana jest tylko firma wykreślona, zawieszona albo z wyraźną informacją o zakończeniu działalności.
- **Szkic powstaje dla każdej firmy, która ma jakiekolwiek źródło** (także wizytówkę z portalu).
- Dawną regułę można przywrócić: Profil i źródła → Scoring → „Stara strona blokuje lead”. Domyślnie jest wyłączona.

Firmy odłożone wcześniej do archiwum „poniżej progu” można przywrócić filtrem „Wszystkie, także archiwum” → „Przywróć”. Po przeliczeniu dostaną wyższy wynik.

## 3. Jasna odpowiedź: „koniec puli” czy „szukaj dalej” (19.11, pkt 1 i 8)

Po każdym imporcie postęp kampanii pokazuje jeden werdykt:
- **✔ Obszar wyczerpany — zakończ wyszukiwanie.** Model wykonał co najmniej 80% planu zapytań i potwierdził wyczerpanie albo dwa kolejne importy nic nie dodały. Kolejne zadanie w tym obszarze nic nie da; trzeba poszerzyć obszar lub zmienić typ klienta.
- **↻ Wynik niepełny — pula NIE jest wyczerpana.** Podana jest przyczyna, np. „wykonano tylko 29% zaplanowanych zapytań” albo „limit czasu”. Zadanie uzupełniające obejmie tylko niesprawdzone miejscowości.
- **✔ Cel kampanii osiągnięty.**

Czyli wynik 85/100 nie jest już zagadką: albo „koniec, więcej nie ma”, albo „niepełne, uruchom uzupełnienie”.

## 4. Lista obecnych klientów i archiwum wysłanych (19.11, pkt 3)

Profil i źródła → Dane i kopie → **„Obecni klienci i archiwum wysłanych”**:
- Wklejcie albo wczytajcie listę (CSV / TXT, np. eksport z CRM lub programu pocztowego). Aplikacja rozpozna **e-maile, strony / domeny, NIP-y**, a pozostałe wiersze uzna za **nazwy firm** (bez „Sp. z o.o.” itp.).
- **„Dodaj wszystkie adresy, na które już wysłano z Huntera”**: jednym kliknięciem.
- Firma z listy jest **blokowana w scoringu i w kreatorze wysyłki** z podanym powodem, także gdy Work / API znajdzie ją ponownie.
- Darmowe skrzynki (gmail, wp, o2…) nie blokują innych firm z tą samą domeną.
- Lista jest w kopii JSON i w profilu. Panel pokazuje, ile firm w bazie obejmuje.

## 5. Strategia łączona: dwa produkty, dwa linki (19.11, pkt 4)

W strategii jest nowe pole **„Drugi produkt — strategia łączona”**, np. ogrodzenia aluminiowe + panelowe.
- Wiadomość dostaje linki do ofert i katalogów **obu** produktów.
- Zadanie dla Work / API zawiera opis obu produktów i informację, że pasuje każdy z nich.

## Wysyłka 2000 dziennie: do ustalenia po stronie TABAL

- Przy obecnym odstępie 30 s jedna skrzynka wyśle maks. ok. 120 wiadomości na godzinę. 2000 dziennie z samego bok@tabal.pl to ryzyko blokady u dostawcy poczty i trafiania do spamu.
- Zalecenie: kilka skrzynek (profile osób już to obsługują, 3.9) albo osobna domena kampanijna, poprawne SPF / DKIM / DMARC i stopniowe zwiększanie wolumenu.
- Masowa korespondencja handlowa wymaga oceny prawnej po stronie TABAL (zgody na informację handlową, RODO).

## Aktualizacja z 3.10

1. Poczekajcie, aż kolejka wysyłki będzie pusta. Pobierzcie Kopię JSON, zatrzymajcie serwer, zachowajcie `.runtime` i `.env`.
2. Rozpakujcie 3.11, przenieście `.runtime` i `.env`, dopiszcie do `.env` sekcję `TABAL_AI_*` z `.env.example` i uruchomcie serwer. W Dockerze: `docker compose up -d --build`, **bez** `down -v`.
3. `/healthz` ma zwracać `3.11.0`, a w menu ma być „WERSJA 3.11”.
4. Test: Odkrywanie → kampania z pulą 10 → „▶ Uruchom bieżące zadanie przez API” → po kilku minutach leady pojawiają się same.

## Testy

| Zakres | Wynik |
|---|---|
| Logika | 194 testy (11 nowych), wszystkie przechodzą |
| Serwer, SMTP, IMAP, strony firm, tryb API | 119 testów (13 nowych), wszystkie przechodzą |
| Przeglądarka | 37 testów, wszystkie przechodzą |
| Tryb API w przeglądarce (kampania → API → automatyczny import, lista wykluczeń, dwa produkty) | 5 scenariuszy, OK |
| Pełny scenariusz wysyłki (lokalny SMTP z TLS) | OK |

Tryb API był testowany na lokalnej atrapie API dostawcy, zgodnej z formatem Responses API: kolejka, praca w tle, odpytywanie, błędy 401 / 429, anulowanie, limit czasu, wznowienie po restarcie serwera. **Pierwsze uruchomienie na prawdziwym koncie róbcie na puli 10.** Jeśli dostawca używa innej nazwy narzędzia wyszukiwania albo modelu, wystarczy zmienić `TABAL_AI_SEARCH_TOOL` / `TABAL_AI_MODEL` w `.env`.
