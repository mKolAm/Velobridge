# 🚴 VeloBridge - Virtual Shifting dla MyWhoosh (Windows & macOS)
### Kompleksowy podręcznik użytkownika, instrukcja konfiguracji i opis możliwości

**VeloBridge** to lekka, samodzielna aplikacja typu **portable (bez konieczności instalacji)** dla systemów **Windows** oraz **macOS**, która pozwala wykorzystać bezprzewodowe kontrolery **Zwift Play** oraz **Zwift Click** do wirtualnej zmiany biegów (**Virtual Shifting / MyShift**) oraz pełnego sterowania w grze **MyWhoosh**.

---

## 📖 Spis treści
1. [O programie VeloBridge](#1-o-programie-velobridge)
2. [Kluczowe możliwości](#2-kluczowe-możliwości)
3. [Wymagania systemowe i sprzętowe](#3-wymagania-systemowe-i-sprzętowe)
4. [Szybki start (Krok po kroku)](#4-szybki-start-krok-po-kroku)
   - [4.1. System Windows](#41-system-windows)
   - [4.2. System macOS (Instalacja i odblokowanie)](#42-system-macos-instalacja-i-odblokowanie)
   - [4.3. Wymagane uprawnienia w systemie macOS](#43-wymagane-uprawnienia-w-systemie-macos-bardzo-ważne)
   - [4.4. Połączenie z manetkami i jazda w MyWhoosh](#44-połączenie-z-manetkami-i-jazda-w-mywhoosh)
   - [4.5. Manetki Zwift Click v2 — Wymóg autoryzacji (Procedura 24h Unlock)](#45-manetki-zwift-click-v2--wymóg-autoryzacji-procedura-24h-unlock)
5. [Konfiguracja ścieżki do gry MyWhoosh](#5-konfiguracja-ścieżki-do-gry-mywhoosh)
   - [A. Windows (Microsoft Store)](#a-wersja-ze-sklepu-microsoft-store-uwp--msix)
   - [B. Windows (MyWhoosh HD / Standalone)](#b-wersja-mywhoosh-hd--standalone)
   - [C. macOS (Mac App Store & /Applications)](#c-system-macos-mac-app-store--katalog-applications)
6. [Domyślne sterowanie i mapa przycisków](#6-domyślne-sterowanie-i-mapa-przycisków)
   - [Zestaw Zwift Play](#-zestaw-zwift-play-montowane-na-baranku-kierownicy)
   - [Zestaw Zwift Click](#-zestaw-zwift-click-obejmy-na-kierownicę)
   - [Własna personalizacja przycisków](#-personalizacja-przycisków)
7. [Wskazówki dla zestawu Zwift Cog](#7-wskazówki-dla-zestawu-zwift-cog)
8. [Plik konfiguracyjny (VeloBridge_config.json)](#8-plik-konfiguracyjny-velobridge_configjson)
9. [Rozwiązywanie problemów (FAQ)](#9-rozwiązywanie-problemów-faq)
10. [Historia zmian (Changelog)](#10-historia-zmian-changelog)
11. [Wsparcie projektu](#11-wsparcie-projektu)

---

## 1. O programie VeloBridge

Aplikacja implementuje bezpieczny protokół komunikacyjny kontrolerów Zwift (wymiana kluczy ECDH, szyfrowanie AES-256-CCM) i tłumaczy stany przycisków oraz łopatek na bezpośrednie, natywne zdarzenia wejścia:
- **Windows:** sprzętowe kody klawiatury DirectInput.
- **macOS:** niskopoziomowe zdarzenia wejścia frameworku Quartz / CoreGraphics.

Dzięki temu możesz natychmiast korzystać z wirtualnej zmiany biegów (**Virtual Shifting / MyShift**) oraz pełnego sterowania rowerzystą (skręcanie, zawracanie, zmiana widoku kamery, powerupy, menu) w silniku 3D gry MyWhoosh na obu platformach.

---

## 2. Kluczowe możliwości

- ⚡ **Wirtualna zmiana biegów (MyShift 30 przełożeń):** Zmiana przełożeń za pomocą łopatek lub przycisków `+` / `–` kontrolerów (bieg startowy: 15).
- 🔒 **Bezpieczna komunikacja Bluetooth Low Energy (BLE):** Pełna obsługa szyfrowania i autoryzacji kontrolerów Zwift.
- 🎮 **Automatyczne rozpoznawanie sprzętu:**
  - Podłączenie kontrolerów **Zwift Play** automatycznie aktywuje widok i mapowanie Zwift Play oraz blokuje profil Click.
  - Podłączenie kontrolerów **Zwift Click** automatycznie przełącza widok i mapowanie na Zwift Click oraz blokuje profil Play.
  - Sprzętowa identyfikacja profilu zapobiega pomyłkom i trwale zapamiętuje ustawienia po restarcie programu.
- ⌨️ **Sprzętowe przekazywanie sygnałów do gry:** Brak opóźnień i bezkolizyjność z oknem gry MyWhoosh.
- 🔊 **Realistyczny dźwięk kliku manetki:** Mechaniczny odgłos indeksowanej przerzutki rowerowej generowany w tle przy każdej zmianie biegu.
- 📳 **Haptyka (wibracje):** Sygnalizacja zmiany przełożenia wibracją manetki ze sprzętowym filtrem eliminującym podwójne zmiany biegów.
- 🪟 **Pływający mini-HUD:** Zawsze widoczne, półprzezroczyste okienko (*Always-on-Top*) z aktualnym biegiem i poziomem baterii podczas jazdy.
- 🎨 **Interaktywny kokpit Rider's POV:** Wizualizacja kierownicy i manetek z perspektywy rowerzysty z podświetlaniem klikanych przycisków na żywo.

---

## 3. Wymagania systemowe i sprzętowe

- **System operacyjny:**
  - **Windows:** Windows 10 (64-bit) lub Windows 11.
  - **macOS:** macOS 12 (Monterey), macOS 13 (Ventura), macOS 14 (Sonoma), macOS 15 (Sequoia) lub nowszy. Działa natywnie na procesorach **Apple Silicon (M1, M2, M3, M4)** oraz **Intel**.
- **Moduł Bluetooth:**
  - Windows: Wbudowany moduł Bluetooth 4.0+ lub adapter USB BLE.
  - macOS: Wbudowany moduł Bluetooth (CoreBluetooth).
- **Kontrolery:**
  - **Zwift Play** (para kontrolerów na baranku kierownicy szosowej), LUB
  - **Zwift Click** (pojedynczy pod lub para na obejmach).
- **Gra:**
  - Windows: MyWhoosh (wersja Microsoft Store lub instalator MyWhoosh HD).
  - macOS: MyWhoosh (ze sklepu Mac App Store lub strona mywhoosh.com zainstalowana w `/Applications/MyWhoosh.app`).
- **Trenażer (opcjonalnie):** Dowolny trenażer smart lub zestaw z pojedynczą zębatką Zwift Cog (14T).

---

## 4. Szybki start (Krok po kroku)

### 4.1. System Windows
1. Pobierz plik **`VeloBridge.exe`** z zakładki [Releases](../../releases).
2. Uruchom pobrany plik `VeloBridge.exe`.
3. Program jest aplikacją przenośną (portable) — nie wymaga instalatora ani uprawnień administratora.

---

### 4.2. System macOS (Instalacja i odblokowanie)

1. **Pobranie aplikacji:**
   - Ze strony repozytorium przejdź do zakładki [Releases](../../releases) i pobierz archiwum **`VeloBridge-macOS.zip`**.
   - Rozpakuj pobrany plik zip (dwuklik w Finderze). Otrzymasz aplikację **`VeloBridge.app`**.
   - Przeciągnij **`VeloBridge.app`** do katalogu **`/Applications`** (Programy).

2. **Pierwsze uruchomienie a blokada Gatekeepera (jednorazowo):**
   - Ponieważ aplikacja jest darmowym programem bez płatnego certyfikatu Apple Developer ($99/rok), macOS przy zwykłym dwukliku może wyświetlić komunikat:  
     *„Program pochodzi od niezidentyfikowanego dewelopera”*.
   - **Jak odblokować:**
     1. Otwórz folder **Programy (`/Applications`)** w programie Finder.
     2. Kliknij na aplikację **VeloBridge** **prawym przyciskiem myszy** (lub przytrzymaj klawisz **Control** i kliknij lewym).
     3. Z menu kontekstowego wybierz **Otwórz**.
     4. W oknie ostrzeżenia kliknij przycisk **Otwórz**. macOS trwale zapamięta Twój wybór!

---

### 4.3. Wymagane uprawnienia w systemie macOS (BARDZO WAŻNE!)

Aby VeloBridge mógł łączyć się przez Bluetooth i przesyłać wciśnięcia klawiszy zmiany biegów do okna gry MyWhoosh, system macOS wymaga nadania dwóch uprawnień:

#### 1. Dostępność (Accessibility) — Klawisze w grze:
macOS ze względów bezpieczeństwa blokuje aplikacjom symulowanie wciśnięć klawiatury bez wyraźnej zgody użytkownika. **Bez tego uprawnienia manetki połączą się z programem, ale gra MyWhoosh nie zareaguje na zmianę biegów!**
- Otwórz: **`Ustawienia systemowe -> Prywatność i ochrona -> Dostępność`**.
- Znajdź na liście **VeloBridge** i włącz przełącznik obok nazwy.

#### 2. Dostęp do Bluetooth:
- Przy pierwszym skanowaniu system zapyta o dostęp do Bluetooth — kliknij **Zezwól**.

---

### 4.4. Połączenie z manetkami i jazda w MyWhoosh

1. **Wybudzenie kontrolerów:**
   - Naciśnij dowolny przycisk na manetkach Zwift Play lub Zwift Click. Diody LED zaczną pulsować.
2. **Wykrycie i sparowanie:**
   - W oknie VeloBridge kliknij **„Zarządzaj manetkami & Skaner BLE”**.
   - Kliknij **„🔍 Skanuj BLE”** – program przeszuka pasmo i wykryje Twoje manetki.
   - Przypisz znalezione urządzenia (w przypadku Zwift Click wystarczy przypisać jedno urządzenie) i kliknij **„✓ Zapisz i zastosuj”**.
   > [!NOTE]
   > **Specyfika macOS (Identyfikatory UUID zamiast MAC):**  
   > System macOS nie ujawnia fizycznych adresów MAC (`AA:BB:CC:DD:EE:FF`), lecz zastępuje je unikalnymi identyfikatorami UUID (np. `68840428-6604-4348-8C64-44DC7A8CE771`). VeloBridge automatycznie rozpoznaje i obsługuje te identyfikatory.
3. **Połączenie i start gry:**
   - Kliknij duży przycisk **„⚡ Połącz i Uruchom MyWhoosh”**.
   - Po 1–2 sekundach diody stanu połączenia zapalą się na zielono (**POŁĄCZONO I ZASZYFROWANO**), a obok pojawi się stan baterii.
   - VeloBridge automatycznie wybudzi lub uruchomi grę MyWhoosh.
4. **Trening:**
   - Rozpocznij jazdę. Wirtualne biegi oraz sterowanie działają natychmiast!

---

<a id="45-manetki-zwift-click-v2--wymóg-autoryzacji-procedura-24h-unlock"></a>
<a id="45-manetki-zwift-click-v2"></a>
### 4.5. Manetki Zwift Click v2 — Wymóg autoryzacji (Procedura 24h Unlock)

W manetkach **Zwift Click v2** producent (Zwift) wprowadził w oprogramowaniu układowym (firmware) sprzętowe zabezpieczenie autoryzacyjne oparte o kryptografię asymetryczną.

> [!IMPORTANT]
> **Zależność od wersji firmware:**  
> We wczesnych partiach sprzętu pojedyncza prawa manetka mogła działać autonomicznie bez kontaktu ze Zwiftem. W nowszych wersjach firmware producent wprowadził restrykcyjny timer sprzętowy (watchdog) — bez autoryzacji manetka rozłącza połączenie BLE (LL_TERMINATE_IND) dokładnie po **60 sekundach**, niezależnie od tego, czy używasz jednej, czy obu manetek.

#### Procedura autoryzacji (24h Unlock):
Aby manetki nie wyłączały się po minucie, raz na dobę należy wykonać szybką autoryzację:
1. Uruchom oficjalną aplikację **Zwift** (na komputerze lub telefonie; wystarczy bezpłatne konto).
2. Na ekranie urządzeń połącz manetki **Zwift Click v2** na **15–30 sekund** (aż pojawi się zielony znacznik połączenia).
3. **Zamknij całkowicie aplikację Zwift** (upewnij się, że proces nie działa w tle).
4. Uruchom **VeloBridge** — manetki zachowują ważność autoryzacji przez 24 godziny i nie rozłączą się podczas treningu!

> [!NOTE]
> **Dlaczego VeloBridge nie może sam udawać serwera Zwift?**  
> Zwift zabezpieczył zestaw manetek kryptografią asymetryczną z cyfrowym podpisem (PKI). W procesorze manetki zaszyty jest klucz publiczny Zwifta, a podpis generowany jest przez prywatny klucz na zabezpieczonych serwerach w chmurze secure.zwift.com. Żaden program zewnętrzny (w tym OpenBikeControl, indieVelo itp.) nie posiada klucza prywatnego Zwifta, dlatego jednorazowa 15-sekundowa autoryzacja w oficjalnej aplikacji jest jedynym sposobem na odblokowanie manetek.

---

## 5. Konfiguracja ścieżki do gry MyWhoosh

VeloBridge automatycznie wykrywa grę MyWhoosh na obu systemach. W razie potrzeby możesz wskazać własną ścieżkę klikając przycisk **„📁 Ścieżka gry”**:

### A. Wersja ze sklepu Microsoft Store (UWP / MSIX)
- W oknie ścieżki kliknij **„📁 Przeglądaj...”** i wskaż skrót `.lnk` do gry przeciągnięty z Menu Start na Pulpit, LUB:
- Pozostaw pole puste – VeloBridge automatycznie odpyta system Windows o identyfikator aplikacji Store.

### B. Wersja MyWhoosh HD / Standalone
- Wskaż bezpośredni plik wykonywalny gry, np. `C:\Program Files\MyWhoosh\MyWhoosh.exe` lub `C:\Program Files\MyWhoosh HD\MyWhoosh.exe`.

### C. System macOS (Mac App Store & katalog /Applications)
- Na komputerach Mac gra domyślnie instaluje się do głównego katalogu aplikacji: `/Applications/MyWhoosh.app`.
- VeloBridge automatycznie odnajdzie grę w systemie. Możesz także kliknąć **„📁 Przeglądaj...”** i wskazać plik **`MyWhoosh.app`**.

---

## 6. Domyślne sterowanie i mapa przycisków

### 🎮 Zestaw Zwift Play (Montowane na baranku kierownicy)

| Kontroler | Przycisk / Element | Domyślna akcja VeloBridge | Klawisz w MyWhoosh |
| :--- | :--- | :--- | :--- |
| **Prawa manetka** | Łopatka górna / włącznik | **Bieg W GÓRĘ (+1)** | Klawisz `K` |
| **Prawa manetka** | Przycisk Y (góra) | **Bieg W GÓRĘ (+1)** | Klawisz `K` |
| **Prawa manetka** | Przycisk B (dół) | **Bieg W DÓŁ (−1)** | Klawisz `I` |
| **Prawa manetka** | Przycisk A (prawo) | **Zatwierdź / Wybór** | Klawisz `Enter` |
| **Prawa manetka** | Przycisk Z (lewo) | **Menu / Pauza** | Klawisz `Escape` |
| **Prawa manetka** | Przycisk boczny (Power) | **Akcja / Powerup** | `Spacja` |
| **Prawa manetka** | Joystick poziomy | **Kierunek w prawo / w lewo** | Strzałki `Prawo` / `Lewo` |
| **Lewa manetka** | Łopatka dolna / włącznik | **Bieg W DÓŁ (−1)** | Klawisz `I` |
| **Lewa manetka** | Przycisk Y (góra) | **Bieg W GÓRĘ (+1)** | Klawisz `K` |
| **Lewa manetka** | Przycisk B (dół) | **Bieg W DÓŁ (−1)** | Klawisz `I` |
| **Lewa manetka** | D-Pad góra / dół | **Wybór trasy** | Strzałki `Góra` / `Dół` |
| **Lewa manetka** | D-Pad lewo / prawo | **Wybór kierunku** | Strzałki `Lewo` / `Prawo` |
| **Lewa manetka** | Przycisk boczny (Power) | **Zawracanie (U-Turn)** | Klawisz `U` |

### 🔘 Zestaw Zwift Click (Obejmy na kierownicę)

| Manetka | Przycisk | Domyślna akcja VeloBridge | Klawisz w MyWhoosh |
| :--- | :--- | :--- | :--- |
| **Prawa manetka** | Duży przycisk **`+`** | **Bieg W GÓRĘ (+1)** | Klawisz `K` |
| **Prawa manetka** | Przycisk **`Y`** (góra) | **Bieg W GÓRĘ (+1)** | Klawisz `K` |
| **Prawa manetka** | Przycisk **`B`** (dół) | **Bieg W DÓŁ (−1)** | Klawisz `I` |
| **Prawa manetka** | Przycisk **`A`** (prawo) | **Zatwierdź (Enter)** | Klawisz `Enter` |
| **Prawa manetka** | Przycisk **`Z`** (lewo) | **Menu / Pauza (Esc)** | Klawisz `Escape` |
| **Lewa manetka** | Duży przycisk **`–`** | **Bieg W DÓŁ (−1)** | Klawisz `I` |
| **Lewa manetka** | Strzałka **Góra** | **Bieg W GÓRĘ (+1)** | Klawisz `K` |
| **Lewa manetka** | Strzałka **Dół** | **Bieg W DÓŁ (−1)** | Klawisz `I` |
| **Lewa manetka** | Strzałka **Lewo** | **Kierunek w lewo** | Strzałka `Lewo` |
| **Lewa manetka** | Strzałka **Prawo** | **Kierunek w prawo** | Strzałka `Prawo` |

### 🎨 Personalizacja przycisków
W oknie głównym aplikacji kliknij dowolny dymek z nazwą akcji na interaktywnym schemacie kierownicy, aby przypisać inną funkcję (np. zmianę kamery `C`, emotki `1`–`5`, nawrót `U`).

---

## 7. Wskazówki dla zestawu Zwift Cog

- **Jedna fizyczna zębatka (Single Cog):** Ponieważ na trenażerze zamontowana jest jedna zębatka (14T), łańcuch pozostaje w idealnej linii, eliminując zużycie napędu i hałas.
- **Wirtualne przełożenie:** Wszystkie zmiany biegów odbywają się programowo w MyWhoosh (funkcja MyShift).
- **Zalecana tarcza z przodu:** Ustaw łańcuch na dużej tarczy (np. 50T lub 52T) na rowerze szosowym lub średniej/dużej tarczy na rowerze gravel/MTB. Zapewnia to optymalną prędkość obrotową koła zamachowego trenażera.

---

## 8. Plik konfiguracyjny (VeloBridge_config.json)

Wszystkie ustawienia, sparowane adresy manetek oraz własne mapowania przycisków są przechowywane w pliku JSON o nazwie **`VeloBridge_config.json`**.

### Lokalizacja pliku:
1. **Windows:** Bezpośrednio obok pliku `VeloBridge.exe` (lub w `%APPDATA%\VeloBridge\`).
2. **macOS:** W trwałym folderze użytkownika: `~/Library/Application Support/VeloBridge/VeloBridge_config.json` (plik jest zachowywany przy aktualizacjach aplikacji).

---

## 9. Rozwiązywanie problemów (FAQ)

### ❓ [Zwift Click v2] Manetka wyłącza się po około 1 minucie (60 sekundach) jazdy:
- **Przyczyna:** W nowszych wersjach oprogramowania (firmware) Zwift Click v2 wbudowany jest 60-sekundowy watchdog sprzętowy, który odcina połączenie BLE, jeśli manetka nie została autoryzowana w oficjalnej aplikacji Zwift w ciągu ostatnich 24 godzin (dotyczy to zarówno pojedynczej, jak i obu manetek).
- **Rozwiązanie:** Przeprowadź szybką procedurę [**24h Unlock** (punkt 4.5)](#45-manetki-zwift-click-v2--wymóg-autoryzacji-procedura-24h-unlock): połącz manetki na 15–30 sekund w oficjalnej aplikacji Zwift, zamknij aplikację Zwift i wróć do VeloBridge. Przez kolejne 24 godziny połączenie nie zostanie przerwane.


### ❓ [macOS] Wyświetla się komunikat: *„Program VeloBridge nie może zostać otwarty, ponieważ pochodzi od niezidentyfikowanego dewelopera”*:
- **Rozwiązanie:** Kliknij na plik `VeloBridge.app` w Finderze **prawym przyciskiem myszy (lub Control + klik)** i wybierz **Otwórz**, a następnie zatwierdź przyciskiem **Otwórz**.

### ❓ [macOS] Manetki połączyły się (zielony status), ale gra MyWhoosh nie reaguje na wciskanie przycisków:
- **Przyczyna:** Brak wymaganego uprawnienia systemowego Dostępności (*Accessibility*).
- **Rozwiązanie:** Otwórz `Ustawienia systemowe -> Prywatność i ochrona -> Dostępność` i upewnij się, że przełącznik przy **VeloBridge** jest włączony. Po zmianie uprawnień zrestartuj aplikację.

### ❓ Manetki nie chcą się połączyć, a dioda LED cały czas miga:
- **Przyczyna:** Manetki mogą być połączone z inną aplikacją w pobliżu (np. aplikacją Zwift lub Zwift Companion na smartfonie/tablecie).
- **Rozwiązanie:** Wyłącz na chwilę Bluetooth w telefonie lub zamknij aplikację Zwift. Kontrolery BLE mogą utrzymywać połączenie tylko z jednym odbiornikiem jednocześnie.

### ❓ [Windows] Gra MyWhoosh nie reaguje na wciskanie przycisków zmiany biegów:
- **Przyczyna:** Okno gry MyWhoosh nie jest oknem aktywnym na pierwszym planie Windows.
- **Rozwiązanie:** Kliknij myszką wewnątrz okna gry MyWhoosh. VeloBridge wysyła sygnały bezpośrednio do aktywnego okna.

### ❓ Prawa manetka redukuje biegi, a lewa wrzuca twardszy (odwrotne działanie):
- **Rozwiązanie:** W karcie „STAN POŁĄCZENIA BLE” w stopce kliknij przycisk **„⇄ Zamień L ↔ P”**. Program natychmiast zamieni role kontrolerów bez potrzeby ponownego parowania.

### ❓ Jak zresetować ustawienia do fabrycznych?
- **Windows:** Usuń plik `VeloBridge_config.json` z folderu programu.
- **macOS:** Usuń plik `~/Library/Application Support/VeloBridge/VeloBridge_config.json`.

---

## 10. Historia zmian (Changelog)

### Wersja 1.6.1 (Najnowsza)
- **Poprawka symulacji klawiatury na macOS w trybie pełnoekranowym:** Bezpośrednie przekazywanie zdarzeń do procesu MyWhoosh (CGEventPostToPid), przejście na kolejkę sesji (kCGSessionEventTap) oraz dołączanie znaków Unicode dla silnika Unreal Engine.
- **Asystent uprawnień Dostępności (Accessibility):** Automatyczna detekcja i okno pomocnicze na macOS ułatwiające nadanie oraz odświeżenie uprawnień TCC.

### Wersja 1.6
- **Pełna kompatybilność z systemem macOS (Apple Silicon M1–M4 & Intel):**
  - Gotowa aplikacja `VeloBridge.app` zintegrowana z systemem dźwiękowym i powłoką macOS.
  - Automatyczne wykrywanie instalacji gry MyWhoosh (`/Applications/MyWhoosh.app`).
- **Eliminacja błędnej identyfikacji modeli (Zwift Click vs Play):**
  - Sprzętowa identyfikacja profilu manetek i trwałe zapamiętywanie wybranego modelu po ponownym uruchomieniu aplikacji.
- **Niezawodne definiowanie przycisków & eliminacja podwójnej zmiany biegów:**
  - Pełna swoboda personalizacji akcji dla wszystkich przycisków manetek w graficznym kokpicie.
  - Sprzętowy filtr drgań styków zapobiegający przypadkowym podwójnym kliknięciom wywołanym wibracjami.

### Wersja 1.5
- Wyeliminowano pozorny stan połączenia przy wyłączonych manetkach. Prawidłowy start w stanie rozłączenia i automatyczna synchronizacja wskaźników baterii z pływającym HUD.

### Wersja 1.4
- Zwiększona stabilność połączeń Bluetooth Low Energy, ujednolicony interfejs kokpitu i zoptymalizowana obsługa profili urządzeń.

### Wersja 1.3
- Wielowarstwowa obsługa przycisku Menu / Pauzy w grze MyWhoosh (automatyczne wybudzanie paska gry i klikanie menu).
- Automatyczne przekazywanie fokusu do okna MyWhoosh podczas jazdy.
- Automatyczne wznawianie zerwanych połączeń BLE (*Auto-Reconnect*).

---

## 11. Wsparcie projektu

Projekt **VeloBridge** jest tworzony i rozwijany z pasji do kolarstwa wirtualnego i treningu na trenażerze.

Jeśli program jest dla Ciebie przydatny i uprzyjemnia Ci treningi w MyWhoosh, możesz postawić autorowi wirtualną kawę:

☕ **[buycoffee.to/mkola](https://buycoffee.to/mkola)**

Dziękuję za każde wsparcie i do zobaczenia na wirtualnych trasach! 🚴💨
