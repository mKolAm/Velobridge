# 📋 Changelog (Historia zmian)

Wszystkie istotne zmiany, usprawnienia oraz poprawki wprowadzone w projekcie zostały opisane poniżej.

---

## [1.6.0] - Poprawka mapowania definicji przycisków i eliminacja podwójnej zmiany biegów (PRO v1.6)

### 🐛 Poprawki błędów i niezawodność sterowania
- **Naprawa zapisywania i wykonywania własnych definicji przycisków:**
  - Rozwiązano problem nieskutecznej zmiany definicji akcji przycisków w menu diagramu kokpitu. Procedura przypisywania (`_assign_action`) oraz generowania etykiet (`_create_callout`) jest teraz w 100% zsynchronizowana z aktywnym modelem sprzętowym (`selected_hardware_model`), eliminując zapis do niewłaściwego profilu (`play` vs `click`).
  - Usunięto agresywny mechanizm fallback w obsłudze przycisków, który wymuszał redukcję biegu (`SHIFT_DOWN`) na przyciskach wyłączonych (`NONE`) lub niewyszukanych w profilu domyślnym. Wybranie opcji `Brak akcji (Wyłączony)` jest teraz ściśle respektowane.
- **Eliminacja podwójnej zmiany biegów na prawej manetce (przyciski Y i B):**
  - Wprowadzono sprzętowo-programowy filtr drgań styków (*Debounce* 150 ms) dla każdego przycisku fizycznego oraz wewnętrzny bufor czasowy w silniku zmiany biegów (`VirtualShifter`), zapobiegając wielokrotnemu rejestrowaniu pojedynczego kliknięcia wywołanemu mechanicznym rezonansem silniczka wibracyjnego (*Haptic Feedback*) umieszczonego w korpusie prawej manetki.
  - Naprawiono pętlę rozpakowywania stanów przycisków w pakietach wielostanowych BLE (kod operacji `0x23`), gwarantując niezależne przypisanie roli (`left` i `right`) i eliminując sztuczne naprzemienne zdarzenia wciśnięcia/zwolnienia.
- **Weryfikacja i synchronizacja zmiany biegów na lewej manetce:**
  - Zapewniono jednoznaczne mapowanie kierunków zmiany biegów: Strzałka w górę (▲) -> `SHIFT_UP` (+1 bieg), Strzałka w dół (▼) oraz łopatka -> `SHIFT_DOWN` (-1 bieg).
  - Wprowadzono pełną swobodę przedefiniowania dowolnego przycisku lewej manetki na sterowanie (skręt w lewo/prawo, pauza, kamera, etc.).
- **Rozbudowa pakietu testów jednostkowych (`23/23 testów OK`):**
  - Dodano `test_virtual_shifter_debounce_prevents_double_shift` (test filtra drgań styków zapobiegającego podwójnej zmianie biegów).
  - Dodano `test_button_redefinition_and_execution_logic` (test separacji profili modeli Play i Click oraz izolacji definicji przycisków).

---

## [1.5.0] - Poprawka stanu połączenia i synchronizacja baterii (PRO v1.5)

### 🐛 Poprawki błędów i synchronizacja UI
- **Eliminacja pozornego stanu połączenia i fałszywych wartości baterii przy wyłączonych manetkach:**
  - Usunięto statyczne wartości demonstracyjne w oknie „STAN POŁĄCZENIA” oraz górnym pasku (`MOST AKTYWNY`, `Połączono i zaszyfrowano`, `92%`, `88%`).
  - Wprowadzono rzeczywisty stan początkowy po starcie programu: wskaźnik główny `○ MOST ROZŁĄCZONY`, wygaszone szare punkty LED (`○`), etykiety stanu `Rozłączono — naciśnij przycisk` oraz stan baterii `🔋 --%`.
  - Wprowadzono pełne czyszczenie i wygaszanie wskaźników baterii (`🔋 --%`) oraz linków BLE w momencie utraty sygnału lub kliknięcia `⏹ Rozłącz`.
  - Zsynchronizowano wyświetlanie baterii na pływającym HUD (`MiniOverlayWindow`) oraz dodano automatyczne odświeżanie schematu kokpitu (`_render_controller_diagram`) przy każdej zmianie stanu połączenia.
  - Rozszerzono pakiet testów jednostkowych o weryfikację czyszczenia stanu połączenia i baterii (`test_initial_and_disconnect_state_reset`).

---

## [1.4.0] - Stabilność architektury, ujednolicenie zasobów i dokumentacja

### ✨ Usprawnienia
- **Centralizacja wersjonowania (`PRO v1.4`) i architektury pakietu:**
  - Ujednolicono identyfikatory aplikacji Windows (`AppUserModelID`), nagłówki okna głównego oraz okna pomocy technicznej.
  - Zoptymalizowano konfigurację kompilacji `PyInstaller` (`VeloBridge.spec` oraz `build_portable.py`) w oparciu o przenośne ścieżki względne.
  - Rozbudowano zestaw automatycznych testów jednostkowych (`20/20 testów`) weryfikujących kryptografię ECDH/AES-CCM, dekodowanie pakietów BLE, blokady współbieżności GATT oraz wyzwalanie Menu/Pauzy w MyWhoosh.

---

## [1.3.0] - Obsługa przycisku Menu / Pauzy w MyWhoosh oraz stabilność połączenia BLE

### ✨ Nowe funkcje
- **Wielowarstwowa obsługa przycisku Menu / Pauzy w MyWhoosh (`Menu / Pauza`, `Pauza treningu`, `Menu + Pauza treningu`):**
  - Automatyczne wybudzanie ukrytego interfejsu HUD w MyWhoosh oraz precyzyjne symulowanie kliknięcia lewego dolnego przycisku **Menu / Pauza** w oknie gry (z natychmiastowym przywróceniem oryginalnej pozycji kursora myszy).
  - Równoległe wysyłanie hybrydowego sygnału klawisza `Escape` (sprzętowy ScanCode `0x01` + wirtualny klawisz `VK_ESCAPE 0x1B` przez `SendInput` oraz bezpośredni komunikat `PostMessageW` do okna gry).
  - Dodano nowe akcje do wyboru w mapowaniu przycisków:
    - **Menu / Pauza [Menu MyWhoosh + Esc]** – wywołuje menu główne/pauzę podczas swobodnej jazdy (*Free Ride*).
    - **Pauza treningu [Klawisz Spacja]** – zatrzymuje aktywny blok w trybie treningu strukturalnego (*Workout*).
    - **Menu + Pauza treningu [Menu + Esc + Spacja]** – uniwersalna akcja łącząca oba tryby.
- **Niezawodne przekazywanie fokusu do okna MyWhoosh (`AttachThreadInput`):**
  - Przed wysłaniem skrótu klawiszowego aplikacja automatycznie przywraca fokus do okna gry MyWhoosh, dzięki czemu zmiana biegów i pauza działają nawet wtedy, gdy użytkownik wcześniej kliknął poza okno gry.

### 🛡️ Stabilność połączenia Bluetooth Low Energy (BLE)
- **Automatyczne wznawianie połączenia (Auto-Reconnect):**
  - W przypadku chwilowego zaniku sygnału lub rozłączenia pojedynczej manetki podczas treningu, silnik BLE automatycznie przechodzi w tryb cichego wznawiania połączenia w tle (do 8 prób z inteligentnym odstępem czasowym), przywracając szyfrowaną sesję bez przerywania jazdy.
- **Kaskadowe (sekwencyjne) łączenie zestawu Zwift Play:**
  - Podczas jednoczesnego łączenia obu manetek (`CONNECT_BOTH`) wprowadzono 1,2-sekundową separację faz parowania ECDH i odkrywania usług GATT między prawym a lewym kontrolerem, co eliminuje kolizje pakietów na pojedynczym adapterze Bluetooth w systemie Windows.
- **Aktywne podtrzymywanie sesji (BLE Keepalive):**
  - Dodano cykliczny odczyt stanu baterii co 45 sekund utrzymujący aktywny nadzór łącza (*Link Supervision Timeout*) i zapobiegający usypianiu połączenia przez system Windows podczas długiej jazdy bez zmiany biegów.
- **Ochrona zapisów GATT i debouncing wibracji:**
  - Wprowadzono blokadę współbieżnych zapisów (`asyncio.Lock`) oraz ogranicznik częstotliwości impulsów haptycznych (140 ms), zapobiegające przeciążeniu stosu Bluetooth WinRT przy bardzo szybkiej, wielokrotnej zmianie przełożeń.

---

## [1.2.0] - Konfigurator ścieżki MyWhoosh i odświeżony interfejs

### ✨ Nowe funkcje i usprawnienia
- **Konfigurator ścieżki do gry MyWhoosh (`📁 Ścieżka gry`):**
  - Dodano okno pozwalające ręcznie wskazać plik wykonywalny `.exe` lub skrót `.lnk` do gry (w tym obsługę wersji **Microsoft Store**, identyfikatorów `shell:AppsFolder` oraz **MyWhoosh HD**) z trwałym zapisem w pliku `VeloBridge_config.json`.
  - Dodano przyciski automatycznego wykrywania instalacji (`🔍 Wykryj automatycznie`) oraz natychmiastowego testu uruchomienia gry (`🚀 Testuj uruchomienie`).
  - Rozbudowano wbudowaną instrukcję oraz dokumentację o szczegółowy poradnik tworzenia i wyszukiwania skrótów dla wersji Microsoft Store oraz MyWhoosh HD.
- **Odświeżony układ interfejsu i identyfikacji wizualnej:**
  - Zaktualizowano logo oraz ikonę aplikacji, zastąpiono dolny prawy panel estetycznym przyciskiem wsparcia projektu (`☕ Postaw kawę na buycoffee.to/mkola`) i uporządkowano górny pasek narzędziowy.

---

## [1.1.0] - Obsługa zestawu Zwift Click (Lewa + Prawa) i ergonomia UI

### ✨ Nowe funkcje i usprawnienia
- **Pełna obsługa dwuczęściowego zestawu Zwift Click (Lewa + Prawa manetka):**
  - Dodano dekodowanie pakietów `0x23` (32-bitowa mapa bitowa przycisków dla sparowanych kontrolerów) oraz `0x07` dla obu stron kierownicy:
    - **Prawa manetka:** `Plus (+)` (Bieg w górę), `Y`, `Z`, `A`, `B`.
    - **Lewa manetka:** `Minus (−)` (Bieg w dół), strzałki kierunkowe `▲`, `▼`, `◀`, `▶`.
  - Obsługa mostkowego trybu parowania Zwift Click (komunikacja obu przycisków przez główne połączenie BLE prawej manetki bez wymogu osobnego parowania lewego modułu).
- **Inteligentne przełączanie widoku i blokada nieaktywnego zestawu:**
  - Automatyczne rozpoznawanie podłączonego sprzętu (Zwift Play vs Zwift Click) w momencie połączenia oraz natychmiastowe przełączenie wizualizacji kokpitu.
  - Automatyczna blokada przełącznika nieużywanego modelu na czas aktywnego połączenia i odblokowanie wyboru po rozłączeniu.
- **Filtracja pakietów Heartbeat (`0x15` / `0x19`):**
  - Wyeliminowano fałszywe wyzwalanie akcji przez pakiety podtrzymujące połączenie wysyłane cyklicznie przez kontrolery.
