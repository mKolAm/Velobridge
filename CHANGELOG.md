# 📋 Changelog (Historia zmian)

Wszystkie istotne zmiany, usprawnienia oraz poprawki wprowadzone w aplikacji zostały opisane poniżej.

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
