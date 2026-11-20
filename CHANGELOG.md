# 📋 Changelog (Historia zmian)

Wszystkie istotne zmiany, usprawnienia oraz poprawki wprowadzone w aplikacji zostały opisane poniżej.

---

## [1.8.6] - Diagnostyka BLE Click v2 oraz aktualizacja instrukcji 24h Unlock

### ✨ Usprawnienia i dokumentacja
- **Aktualizacja instrukcji 24h Unlock dla Zwift Click v2:** Doprecyzowano w `README.md`, `POMOC_I_INSTRUKCJA_GITHUB.md` oraz w oknie `❓ Pomoc`, aby podczas autoryzacji na darmowym koncie Zwift poklikać przez 15–30 s przyciski w menu gry (np. `B` jako Wstecz) na docelowym komputerze oraz że zgaśnięcie diod LED po chwili jest normalnym oszczędzaniem baterii.
- **Usprawnienia diagnostyki BLE (`src/ble_worker.py`):** Wykrywanie cichych rozłączeń w `_keepalive_loop`, cykliczne odświeżanie `RideOn` dla Click v2, logowanie `button_rx_count` / `heartbeat_rx_count` / `handshake_resp_hex` oraz zapis nieznanych pakietów `PACKET_UNPARSED` i wersji aplikacji w widocznym logu.

---

## [1.8.0] - Kalibracja przycisków HUD MyWhoosh (Menu, Resume Ride, Kamera, Rozjazdy, Emotki)

### ✨ Nowe funkcje
- **System Kalibracji HUD (`🎯 Kalibracja HUD`):**
  - Dodano okno `HudCalibrationDialog` (unoszące się zawsze na wierzchu nad oknem gry) umożliwiające wskazanie jednym kliknięciem pozycji elementów interfejsu MyWhoosh na ekranie (`🎯 Wskaż`) oraz ich natychmiastowe przetestowanie (`▶ Test`).
  - Pozycje zapisywane są proporcjonalnie do rozmiaru okna gry w sekcji `hud_points` w `VeloBridge_config.json`, dzięki czemu działają niezależnie od rozdzielczości i trybu okna.
- **Nowe makra klikające HUD (`HUD Klik: ...`) do przypisania pod dowolny przycisk manetki:**
  - **Menu / Pauza (`HUD_MENU`)** oraz **Wznów jazdę / Resume Ride (`HUD_RESUME`)** na ekranie pauzy.
  - **Trzy kreski `☰` (`HUD_THREE_BARS`)** oraz **2-etapowa zmiana kamery (`HUD_CAMERA`)** — automatyczne rozwinięcie paska `☰` i kliknięcie wybranego widoku po 250 ms.
  - **Rozjazdy na skrzyżowaniach:** **Skręt w LEWO (`HUD_TURN_LEFT`)** oraz **Skręt w PRAWO (`HUD_TURN_RIGHT`)**.
  - **Nawrót / U-Turn (`HUD_UTURN`)**, **Otwarcie koła emotek (`HUD_EMOTE`)** oraz **2-etapowe Emotki (`HUD_EMOTE_1`, `HUD_EMOTE_2`)** — otwarcie koła emotek i automatyczne kliknięcie wybranej reakcji.

### 🧹 Uporządkowanie interfejsu i poprawki
- **Przejrzysta lista akcji manetek:** Usunięto z menu przypisywania przycisków niedziałające na PC skróty klawiaturowe MyWhoosh — pozostały wyłącznie działające opcje zmiany biegów (`+1`, `-1`, `+2`, `-2`) oraz makra `HUD Klik: ...`.
- **Niezawodne wykrywanie okna MyWhoosh (`find_mywhoosh_window`):** Wykluczono przeglądarki internetowe (np. karty z napisem „MyWhoosh”) oraz okna pomocnicze mniejsze niż `200×200 px`.
- **Poprawka kliknięć w Unreal Engine:** Zastosowano `SendInput` (`INPUT_MOUSE` + `MOUSEEVENTF_ABSOLUTE`) z czasem wybudzenia HUD (`120 ms`), bez blokującego `AttachThreadInput`, oraz wykonywaniem makr HUD w głównym wątku GUI.

---

## [1.7.1] - Szyfrowane logi diagnostyczne (.vblog) oraz poprawka Auto-Reconnect

### ✨ Nowe funkcje
- **Szyfrowane logowanie problemów (`🐞 Log problemu`):**
  - Dodano przycisk `🐞 Log problemu` w panelu **OPCJE & LIVE MONITOR** oraz okno eksportu zaszyfrowanych raportów diagnostycznych `.vblog` (AES-256-GCM + kompresja `zlib`).
  - Aplikacja stale buforuje w pamięci szczegółową telemetrię połączeń BLE: wykryte modele i wersje kontrolerów, usługi/charakterystyki GATT, czas pracy przed rozłączeniem, błędy `keepalive`, próby `Auto-Reconnect`, poziom baterii i siłę sygnału `RSSI`.
  - Dodano instrukcję zgłaszania problemów w oknie Pomocy (`❓ Pomoc`), `README.md` oraz `POMOC_I_INSTRUKCJA_GITHUB.md`.

### 🐛 Poprawki błędów i stabilność BLE
- **Naprawa pętli `Auto-Reconnect`:** Nieudana próba ponownego połączenia (`is_reconnect=True`) nie anuluje już zadania `_auto_reconnect`, dzięki czemu aplikacja cierpliwie wznawia próby aż do powrotu lub wybudzenia manetki.
- **Sekwencyjne łączenie pary kontrolerów:** Druga manetka czeka na zakończenie fazy GATT/ECDH pierwszej (z limitem 5 s), co zapobiega przeciążeniu pojedynczego radia Bluetooth w Windows i błędom `BleakDeviceNotFoundError` przy starcie.

---

## [1.7.0] - Zwift Click v1 oraz Uniwersalny Moduł Kontrolerów Bluetooth (Cycplus BC2 / Piloty HID)

### ✨ Nowe funkcje i wsparcie sprzętowe
- **Pełna, automatyczna obsługa Zwift Click v1 (2 przyciski `+` / `−`):**
  - Dodano dekoder nieszyfrowanych pakietów Protobuf `0x37` (`ClickKeyPadStatus`) oraz czysty handshake `RideOn` (bez zbędnego oczekiwania na szyfrowanie ECDH i bez wymogu procedury 24h Unlock w Zwift).
  - Automatyczne rozpoznawanie generacji Click v1 vs Click v2 na podstawie struktury GATT oraz przychodzących pakietów (z dwukierunkową autokorektą).
  - Automatyczne przełączanie kokpitu na 2-przyciskowy schemat **Zwift Click v1 (`+` / `−`)** oraz skrócenie kreatora diagnostyki z 10 do 2 kroków.
- **Uniwersalny Moduł Kontrolerów Bluetooth (`🎛️ Inny BT / Pilot`):**
  - **Tryb 1 — Manetki BLE GATT (Tryb Nauki):** obsługa kontrolerów innych producentów, takich jak **Cycplus BC2**, **ThinkRider VS200** oraz urządzeń zgodnych ze standardem **OpenBikeControl** (`d273f680-...`). Aplikacja automatycznie subskrybuje kanały Notify/Indicate i pozwala w kilka sekund „nauczyć” dowolny przycisk BLE (zapamiętuje parę `UUID + payload hex`).
  - **Tryb 2 — Pilot BT / HID (Tryb Nauki):** obsługa tanich pilotów multimedialnych Bluetooth na kierownicę, pierścieni selfie/TikTok oraz klawiatur BT sparowanych z systemem Windows.
  - **Tłumienie klawiszy systemowych (Windows):** opcjonalna blokada oryginalnego działania przechwyconego klawisza (np. `Volume Up`/`Volume Down` na pilocie BT zmienia wyłącznie biegi w MyWhoosh i nie podgłaśnia systemu Windows).

---

## [1.6.1] - Poprawka przekazywania klawiszy w trybie pełnoekranowym i asystent uprawnień macOS

### 🐛 Poprawki błędów i usprawnienia macOS
- **Naprawa wysyłania zdarzeń klawiatury w trybie pełnoekranowym na macOS:**
  - Przejście na standardowy kanał sesji użytkownika (kCGSessionEventTap = 1) oraz bezpośrednią iniekcję do kolejki procesu MyWhoosh (CGEventPostToPid). Rozwiązuje to problem blokowania sygnałów w grze uruchomionej na pełnym ekranie bez zewnętrznego monitora.
  - Dołączanie ciągów znaków Unicode (CGEventKeyboardSetUnicodeString) do zdarzeń klawiszy K i I, co gwarantuje pełne rozpoznanie akcji przez silnik 3D Unreal Engine gry MyWhoosh.
  - Zastosowanie natywnego profilu sprzętowego kCGEventSourceStateCombinedSessionState.
- **Asystent uprawnień Dostępności w interfejsie graficznym:**
  - Dodano automatyczne okno dialogowe informujące użytkownika o braku uprawnień Dostępności (Accessibility) oraz wyjaśniające konieczność odświeżenia suwaka po aktualizacji programu.

---

## [1.6.0] - Obsługa macOS, poprawka mapowania przycisków i eliminacja podwójnej zmiany biegów

### ✨ Nowe funkcje i wsparcie platform
- **Pełna obsługa systemu macOS (Apple Silicon M1–M4 oraz Intel):**
  - Gotowa, natywna aplikacja `VeloBridge.app` dla komputerów Mac zoptymalizowana pod ciemny motyw.
  - Niskopoziomowy silnik zdarzeń Quartz / CoreGraphics gwarantujący 100% kompatybilności z grą MyWhoosh.
  - Automatyczne wykrywanie i uruchamianie gry ze standardowej lokalizacji `/Applications/MyWhoosh.app` oraz przez indeks Spotlight.
  - Odgłosy zmiany biegów odtwarzane przez systemowy silnik dźwiękowy macOS.
- **Sprzętowa identyfikacja modelu kontrolerów (Zwift Click vs Play):**
  - Wprowadzono sprzętową identyfikację profilu manetek eliminującą błąd fabrycznego oprogramowania Zwift Click.
  - Trwałe zapamiętywanie wybranego modelu kontrolera i sparowanych urządzeń po restarcie aplikacji.

### 🐛 Poprawki błędów i niezawodność sterowania
- **Naprawa zapisywania i wykonywania własnych definicji przycisków:**
  - Rozwiązano problem nieskutecznej zmiany definicji akcji przycisków w menu diagramu kokpitu. Wybór użytkownika jest w 100% zsynchronizowany z aktywnym modelem sprzętowym.
  - Usunięto niepożądany fallback w obsłudze przycisków – wybranie opcji `Brak akcji (Wyłączony)` jest teraz ściśle respektowane.
- **Eliminacja podwójnej zmiany biegów na prawej manetce (przyciski Y i B):**
  - Wprowadzono sprzętowy filtr eliminujący przypadkowe podwójne zmiany biegów (+2 / -2) wywołane drganiami silniczka wibracyjnego w korpusie manetki.
  - Zapewniono stabilne, niezależne rejestrowanie stanów obu manetek.
- **Weryfikacja i synchronizacja zmiany biegów na lewej manetce:**
  - Zapewniono jednoznaczne mapowanie kierunków zmiany biegów: Strzałka w górę (▲) -> Bieg w górę (+1), Strzałka w dół (▼) oraz łopatka -> Bieg w dół (-1).
  - Wprowadzono pełną swobodę przedefiniowania dowolnego przycisku lewej manetki na sterowanie (skręt w lewo/prawo, pauza, kamera, etc.).

---

## [1.5.0] - Poprawka stanu połączenia i synchronizacja baterii

### 🐛 Poprawki błędów i synchronizacja UI
- **Eliminacja pozornego stanu połączenia i fałszywych wartości baterii przy wyłączonych manetkach:**
  - Wprowadzono rzeczywisty stan początkowy po starcie programu: wskaźnik główny `○ MOST ROZŁĄCZONY`, wygaszone szare punkty LED (`○`), etykiety stanu `Rozłączono — naciśnij przycisk` oraz stan baterii `🔋 --%`.
  - Wprowadzono pełne czyszczenie i wygaszanie wskaźników baterii oraz linków BLE w momencie utraty sygnału lub kliknięcia `⏹ Rozłącz`.
  - Zsynchronizowano wyświetlanie baterii na pływającym HUD oraz dodano automatyczne odświeżanie schematu kokpitu przy każdej zmianie stanu połączenia.

---

## [1.4.0] - Stabilność połączeń i ujednolicenie interfejsu

### ✨ Usprawnienia
- Ujednolicono identyfikatory aplikacji Windows, nagłówki okna głównego oraz okna pomocy technicznej.
- Zoptymalizowano obsługę profili kontrolerów i stabilność komunikacji BLE.

---

## [1.3.0] - Obsługa przycisku Menu / Pauzy w MyWhoosh oraz stabilność połączenia BLE

### ✨ Nowe funkcje
- **Wielowarstwowa obsługa przycisku Menu / Pauzy w MyWhoosh:**
  - Automatyczne wybudzanie ukrytego interfejsu HUD w MyWhoosh oraz klikanie przycisku **Menu / Pauza** w oknie gry.
  - Dodano nowe akcje do wyboru w mapowaniu przycisków:
    - **Menu / Pauza [Menu MyWhoosh + Esc]** – wywołuje menu główne/pauzę podczas swobodnej jazdy (*Free Ride*).
    - **Pauza treningu [Klawisz Spacja]** – zatrzymuje aktywny blok w trybie treningu strukturalnego (*Workout*).
    - **Menu + Pauza treningu [Menu + Esc + Spacja]** – uniwersalna akcja łącząca oba tryby.
- **Niezawodne przekazywanie fokusu do okna MyWhoosh:**
  - Przed wysłaniem skrótu klawiszowego aplikacja automatycznie przywraca fokus do okna gry MyWhoosh, dzięki czemu zmiana biegów i pauza działają nawet wtedy, gdy użytkownik wcześniej kliknął poza okno gry.

### 🛡️ Stabilność połączenia Bluetooth Low Energy (BLE)
- **Automatyczne wznawianie połączenia (Auto-Reconnect):**
  - W przypadku chwilowego zaniku sygnału kontrolera podczas treningu, silnik BLE automatycznie wznawia połączenie w tle bez przerywania jazdy.
- **Kaskadowe łączenie zestawu Zwift Play:**
  - Bezpieczna separacja faz łączenia manetek zapobiegająca kolizjom pakietów na adapterze Bluetooth.
- **Aktywne podtrzymywanie sesji (Keepalive):**
  - Cykliczny nadzór łącza zapobiegający usypianiu połączenia podczas długiej jazdy bez zmiany biegów.

---

## [1.2.0] - Konfigurator ścieżki MyWhoosh i odświeżony interfejs

### ✨ Nowe funkcje i usprawnienia
- **Konfigurator ścieżki do gry MyWhoosh (`📁 Ścieżka gry`):**
  - Dodano okno pozwalające ręcznie wskazać plik gry lub skrót (w tym obsługę wersji Microsoft Store oraz MyWhoosh HD).
  - Dodano przyciski automatycznego wykrywania instalacji oraz natychmiastowego testu uruchomienia gry.
- **Odświeżony układ interfejsu:**
  - Nowe logo i ikona aplikacji, przycisk wsparcia projektu oraz uporządkowany górny pasek narzędziowy.

---

## [1.1.0] - Obsługa zestawu Zwift Click (Lewa + Prawa) i ergonomia UI

### ✨ Nowe funkcje i usprawnienia
- **Pełna obsługa zestawu Zwift Click (Lewa + Prawa manetka):**
  - Obsługa przycisków zmiany biegów `+` / `−` oraz dodatkowych przycisków funkcyjnych.
- **Inteligentne przełączanie widoku i blokada nieaktywnego zestawu:**
  - Automatyczne rozpoznawanie podłączonego sprzętu (Zwift Play vs Zwift Click) w momencie połączenia oraz natychmiastowe przełączenie wizualizacji kokpitu.
