# 🚴 VeloBridge Pro — Virtual Shifting dla MyWhoosh (Windows & macOS)
### Kompleksowy podręcznik użytkownika, instrukcja konfiguracji i opis techniczny

---

## 📖 Spis treści
1. [O projekcie VeloBridge](#1-o-projekcie-velobridge)
2. [Kluczowe możliwości](#2-kluczowe-możliwości)
3. [Wymagania systemowe i sprzętowe](#3-wymagania-systemowe-i-sprzętowe)
4. [Szybki start (Krok po kroku)](#4-szybki-start-krok-po-kroku)
   - [4.1. System Windows](#41-system-windows)
   - [4.2. System macOS (Instalacja i pierwsze uruchomienie)](#42-system-macos-instalacja-i-pierwsze-uruchomienie)
   - [4.3. Wymagane uprawnienia w systemie macOS](#43-wymagane-uprawnienia-w-systemie-macos-bardzo-ważne)
   - [4.4. Połączenie z manetkami i jazda w MyWhoosh](#44-połączenie-z-manetkami-i-jazda-w-mywhoosh)
5. [Konfiguracja ścieżki do gry MyWhoosh](#5-konfiguracja-ścieżki-do-gry-mywhoosh)
   - [A. Windows (Microsoft Store)](#a-wersja-ze-sklepu-microsoft-store-uwp--msix)
   - [B. Windows (MyWhoosh HD / Standalone)](#b-wersja-mywhoosh-hd--standalone-instalator-ze-strony-www)
   - [C. macOS (Mac App Store & /Applications)](#c-system-macos-mac-app-store--katalog-applications)
6. [Domyślne sterowanie i mapa przycisków](#6-domyślne-sterowanie-i-mapa-przycisków)
7. [Plik konfiguracyjny (VeloBridge_config.json)](#7-plik-konfiguracyjny-velobridge_configjson)
8. [Rozwiązywanie problemów (FAQ)](#8-rozwiązywanie-problemów-faq)
9. [Historia zmian (Changelog)](#9-historia-zmian-changelog)
10. [Wsparcie projektu](#10-wsparcie-projektu)

---

## 1. O projekcie VeloBridge

**VeloBridge** to lekka, samodzielna aplikacja typu **Portable (bez konieczności tradycyjnej instalacji)** dla systemów **Windows** oraz **macOS**, która łączy bezprzewodowe kontrolery **Zwift Play** oraz **Zwift Click** z grą **MyWhoosh**.

Aplikacja implementuje pełny protokół kryptograficzny BLE kontrolerów Zwift i tłumaczy stany przycisków oraz łopatek na bezpośrednie, niskopoziomowe zdarzenia wejścia:
- **Windows:** kody sprzętowe **DirectInput** (Hardware Scan Codes przez `SendInput`).
- **macOS:** niskopoziomowe zdarzenia HID frameworku Quartz / CoreGraphics (`CGEventCreateKeyboardEvent` / `CGEventPost`).

Pozwala to na natychmiastowe korzystanie z wirtualnej zmiany biegów (**Virtual Shifting / MyShift**) oraz pełnego sterowania rowerzystą (skręcanie, zawracanie, kamera, powerupy, menu) w silniku Unreal Engine gry MyWhoosh na obu platformach.

---

## 2. Kluczowe możliwości

- ⚡ **Wirtualna zmiana biegów (MyShift 30 przełożeń):** Płynna zmiana przełożeń za pomocą łopatek lub przycisków `+` / `–` kontrolerów (bieg startowy: 15).
- 🔒 **Bezpieczna komunikacja Bluetooth Low Energy (BLE):**
  - Wymiana kluczy **ECDH** na krzywej eliptycznej `SECP256R1` (NIST P-256).
  - Derywacja kluczy symetrycznych **HKDF-SHA256**.
  - Dwukierunkowe szyfrowanie pakietów **AES-256-CCM** z 4-bajtowym tagiem uwierzytelniającym MIC.
- 🎮 **Automatyczne rozpoznawanie sprzętu:**
  - Podłączenie kontrolerów **Zwift Play** automatycznie aktywuje widok i mapowanie Zwift Play oraz blokuje profil Click.
  - Podłączenie kontrolerów **Zwift Click** automatycznie przełącza widok i mapowanie na Zwift Click oraz blokuje profil Play.
  - Sprzętowa inspekcja unikalnych charakterystyk GATT zapobiega myleniu kontrolerów z powodu błędów w nazwach fabrycznych firmware.
- ⌨️ **Sprzętowe przekazywanie sygnałów do gry:** Symulacja fizycznych wciśnięć klawiszy bezkolizyjna z grami 3D (Win32 SendInput / macOS Quartz).
- 🔊 **Realistyczny dźwięk kliku manetki:** Mechaniczny odgłos indeksowanej przerzutki rowerowej generowany w tle przy każdej zmianie biegu (`winsound` / `afplay`).
- 📳 **Haptyka (wibracje):** Sygnalizacja zmiany przełożenia wibracją manetki Zwift z wbudowanym zabezpieczeniem przed drganiem styków (*Debounce*).
- 🪟 **Pływający mini-HUD:** Zawsze widoczne, półprzezroczyste okienko (*Always-on-Top*) z aktualnym biegiem i poziomem baterii obu manetek podczas jazdy w MyWhoosh.
- 🎨 **Interaktywny kokpit Rider's POV:** Wizualizacja kierownicy i manetek z perspektywy rowerzysty z podświetlaniem klikanych przycisków w czasie rzeczywistym.

---

## 3. Wymagania systemowe i sprzętowe

- **System operacyjny:**
  - **Windows:** Windows 10 (64-bit) lub Windows 11.
  - **macOS:** macOS 12 (Monterey), macOS 13 (Ventura), macOS 14 (Sonoma), macOS 15 (Sequoia) lub nowszy. Działa natywnie na procesorach **Apple Silicon (M1, M2, M3, M4)** oraz **Intel**.
- **Moduł Bluetooth:**
  - Windows: Wbudowany moduł Bluetooth 4.0+ lub adapter USB BLE (WinRT Bluetooth API).
  - macOS: Wbudowany moduł Bluetooth (CoreBluetooth API).
- **Kontrolery:**
  - **Zwift Play** (lewa + prawa manetka montowane na baranku kierownicy), LUB
  - **Zwift Click** (pojedyncza manetka lub zestaw lewa + prawa montowane na obejmach).
- **Gra:**
  - Windows: MyWhoosh (wersja Microsoft Store lub instalator MyWhoosh HD).
  - macOS: MyWhoosh (ze sklepu Mac App Store lub strona mywhoosh.com zainstalowana w `/Applications/MyWhoosh.app`).
- **Trenażer (opcjonalnie):** Dowolny trenażer smart lub zestaw z pojedynczą zębatką Zwift Cog (14T).

---

## 4. Szybki start (Krok po kroku)

### 4.1. System Windows
1. **Pobranie i uruchomienie:**
   - Pobierz i uruchom plik `dist\VeloBridge.exe` (lub użyj skryptu `Uruchom_VeloBridge.bat`).
   - Program jest w 100% portable – nie wymaga instalatora ani uprawnień administratora.

---

### 4.2. System macOS (Instalacja i pierwsze uruchomienie)

#### A. Pobranie z GitHub Releases:
1. Ze strony repozytorium GitHub przejdź do zakładki **Releases** i pobierz archiwum **`VeloBridge-macOS.zip`**.
2. Rozpakuj pobrany plik zip (dwuklik w Finderze). Otrzymasz aplikację **`VeloBridge.app`**.
3. Przeciągnij **`VeloBridge.app`** do katalogu **`/Applications`** (Programy).

#### B. Pierwsze uruchomienie a mechanizm zabezpieczeń macOS (Gatekeeper):
Ponieważ aplikacja jest projektem open-source i nie posiada płatnego certyfikatu Apple Developer ID ($99/rok), system macOS przy próbie zwykłego dwukliku wyświetli ostrzeżenie:  
> *„Nie można otworzyć programu VeloBridge, ponieważ pochodzi od niezidentyfikowanego dewelopera”* lub *„Program jest uszkodzony i nie może być otwarty”*.

**Jak jednorazowo odblokować aplikację (metoda zalecana):**
1. Otwórz folder **Programy (`/Applications`)** w programie Finder.
2. Kliknij na aplikację **VeloBridge** **prawym przyciskiem myszy** (lub przytrzymaj klawisz **Control** i kliknij lewym).
3. Z menu kontekstowego wybierz **Otwórz**.
4. W oknie ostrzeżenia pojawi się dodatkowy przycisk **Otwórz** — kliknij go. macOS trwale zapamięta Twój wybór!

> [!TIP]
> **Szybkie odblokowanie przez Terminal (jeśli aplikacja nadal zgłasza błąd kwarantanny):**
> Otwórz Terminal i wklej poniższe polecenie:
> ```bash
> xattr -cr /Applications/VeloBridge.app
> chmod +x /Applications/VeloBridge.app/Contents/MacOS/VeloBridge
> ```

#### C. Uruchomienie ze źródeł (dla programistów):
Jeśli pobrałeś pełny kod źródłowy z GitHuba, w głównym katalogu kliknij dwukrotnie plik:
```bash
./Uruchom_VeloBridge.command
```
*(Skrypt samoczynnie utworzy środowisko Python `.venv`, doinstaluje wymagane biblioteki i uruchomi aplikację).*

---

### 4.3. Wymagane uprawnienia w systemie macOS (BARDZO WAŻNE!)

Aby VeloBridge mógł połączyć się przez Bluetooth i przesyłać wciśnięcia klawiszy zmiany biegów do okna gry MyWhoosh, system macOS wymaga nadania dwóch jednorazowych uprawnień:

#### 1. Dostępność (Accessibility) — Klawisze w grze:
System macOS blokuje aplikacjom programowe symulowanie wciśnięć klawiatury (`CGEventPost`) bez wyraźnej zgody użytkownika. **Bez tego uprawnienia manetki połączą się z programem, ale gra MyWhoosh nie zareaguje na zmianę biegów!**
- Otwórz: **`Ustawienia systemowe -> Prywatność i ochrona -> Dostępność`**.
- Kliknij ikonę kłódki / autoryzuj się hasłem lub Touch ID.
- Znajdź na liście **VeloBridge** i włącz przełącznik obok nazwy.
- *(Jeśli uruchamiasz z Terminala lub skryptu `.command`, włącz uprawnienie dla programu **Terminal**).*

#### 2. Dostęp do Bluetooth:
- Przy pierwszym skanowaniu system wyświetli standardowy monit: *„VeloBridge chce korzystać z Bluetooth”*.
- Kliknij **Zezwól**.

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
   > Ze względów ochrony prywatności system macOS nie ujawnia fizycznych adresów MAC (`AA:BB:CC:DD:EE:FF`), lecz zastępuje je unikalnymi identyfikatorami UUID (np. `68840428-6604-4348-8C64-44DC7A8CE771`). Jest to całkowicie normalne — VeloBridge w pełni obsługuje te identyfikatory.
3. **Połączenie i start gry:**
   - Kliknij duży przycisk **„⚡ Połącz i Uruchom MyWhoosh”**.
   - Po 1–2 sekundach diody stanu połączenia zapalą się na zielono (**POŁĄCZONO I ZASZYFROWANO**), a obok pojawi się stan baterii.
   - VeloBridge automatycznie wybudzi lub uruchomi grę MyWhoosh.
4. **Trening:**
   - Rozpocznij jazdę. Zmiana biegów oraz sterowanie działają natychmiast!

---

## 5. Konfiguracja ścieżki do gry MyWhoosh

VeloBridge posiada zaawansowany silnik automatycznego wykrywania gry MyWhoosh na obu systemach. W razie potrzeby możesz wskazać własną ścieżkę klikając przycisk **„📁 Ścieżka gry”**:

### A. Wersja ze sklepu Microsoft Store (UWP / MSIX)
- W oknie ścieżki kliknij **„📁 Przeglądaj...”** i wskaż skrót `.lnk` do gry przeciągnięty z Menu Start na Pulpit, LUB:
- Pozostaw pole puste – VeloBridge automatycznie odpyta powłokę Windows (`Get-StartApps`) o identyfikator aplikacji Store.

### B. Wersja MyWhoosh HD / Standalone (Instalator ze strony www)
- Wskaż bezpośredni plik wykonywalny gry, np. `C:\Program Files\MyWhoosh\MyWhoosh.exe` lub `C:\Program Files\MyWhoosh HD\MyWhoosh.exe`.

### C. System macOS (Mac App Store & katalog /Applications)
- Na komputerach Mac gra MyWhoosh domyślnie instaluje się do głównego katalogu aplikacji:
  `/Applications/MyWhoosh.app`
- Jeśli gra znajduje się w innej lokalizacji, VeloBridge automatycznie odnajdzie ją za pomocą indeksu Spotlight (`mdfind kMDItemCFBundleIdentifier == "com.mywhoosh.*"`).
- W oknie konfiguracji możesz również kliknąć **„📁 Przeglądaj...”** i wybrać plik **`MyWhoosh.app`**.

---

## 6. Domyślne sterowanie i mapa przycisków

### 🎮 Zestaw Zwift Play (Montowane na baranku kierownicy)

| Kontroler | Przycisk / Element | Domyślna akcja VeloBridge | Klawisz w MyWhoosh (DirectInput / macOS) |
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

> **Personalizacja przycisków:** W oknie głównym aplikacji kliknij dowolny dymek z nazwą akcji na schemacie kierownicy, aby przypisać inną funkcję (np. zmiana kamery `C`, emotki `1`–`5`, nawrót `U`).

---

## 7. Plik konfiguracyjny (VeloBridge_config.json)

Wszystkie ustawienia, sparowane adresy manetek oraz własne mapowania przycisków są przechowywane w pliku JSON o nazwie **`VeloBridge_config.json`**.

### Lokalizacja pliku w zależności od systemu:
1. **Windows:**
   - W trybie portable: bezpośrednio w katalogu z plikiem `VeloBridge.exe`.
   - W trybie użytkownika: `%APPDATA%\VeloBridge\VeloBridge_config.json`.
2. **macOS:**
   - Domyślna trwała lokalizacja: `~/Library/Application Support/VeloBridge/VeloBridge_config.json`.
   - Plik w tej lokalizacji jest zachowywany przy ponownej instalacji lub aktualizacji paczki `.app`.

---

## 8. Rozwiązywanie problemów (FAQ)

### ❓ [macOS] Wyświetla się komunikat: *„Program VeloBridge nie może zostać otwarty, ponieważ pochodzi od niezidentyfikowanego dewelopera”*:
- **Rozwiązanie:** Kliknij na plik `VeloBridge.app` w Finderze **prawym przyciskiem myszy (lub Control + klik)** i wybierz **Otwórz**, a następnie zatwierdź przyciskiem **Otwórz**.

### ❓ [macOS] Wyświetla się błąd: *„Program jest uszkodzony i nie może być otwarty”*:
- **Przyczyna:** System macOS oznaczył pobrany z Internetu plik atrybutem kwarantanny.
- **Rozwiązanie:** Otwórz program Terminal i usuń flagę kwarantanny poleceniem:
  ```bash
  xattr -cr /Applications/VeloBridge.app
  ```

### ❓ [macOS] Manetki połączyły się (zielony status), ale gra MyWhoosh nie reaguje na wciskanie przycisków:
- **Przyczyna:** Brak wymaganego uprawnienia systemowego Dostępności (*Accessibility*).
- **Rozwiązanie:** Otwórz `Ustawienia systemowe -> Prywatność i ochrona -> Dostępność` i upewnij się, że przełącznik przy **VeloBridge** (lub Terminal) jest włączony. Po zmianie uprawnień zrestartuj aplikację.

### ❓ Manetki nie chcą się połączyć, a dioda LED cały czas miga:
- **Przyczyna:** Manetki mogą być połączone z inną aplikacją w pobliżu (np. aplikacją Zwift lub Zwift Companion na smartfonie/tablecie).
- **Rozwiązanie:** Wyłącz na chwilę Bluetooth w telefonie lub zamknij aplikację Zwift. Kontrolery BLE mogą utrzymywać połączenie tylko z jednym odbiornikiem jednocześnie.

### ❓ [Windows] Gra MyWhoosh nie reaguje na wciskanie przycisków zmiany biegów:
- **Przyczyna:** Okno gry MyWhoosh nie jest oknem aktywnym na pierwszym planie Windows.
- **Rozwiązanie:** Kliknij myszką wewnątrz okna gry MyWhoosh. VeloBridge wysyła kody DirectInput bezpośrednio do aktywnego okna.

### ❓ Prawa manetka redukuje biegi, a lewa wrzuca twardszy (odwrotne działanie):
- **Rozwiązanie:** W karcie „STAN POŁĄCZENIA BLE” w stopce kliknij przycisk **„⇄ Zamień L ↔ P”**. Program natychmiast zamieni role kontrolerów bez potrzeby ponownego parowania.

### ❓ Jak zresetować ustawienia do fabrycznych?
- **Windows:** Usuń plik `VeloBridge_config.json` z katalogu programu lub z `%APPDATA%\VeloBridge\`.
- **macOS:** Usuń plik `~/Library/Application Support/VeloBridge/VeloBridge_config.json`.

---

## 9. Historia zmian (Changelog)

### Wersja 1.6
- **Pełna kompatybilność z systemem macOS (Apple Silicon M1–M4 & Intel):**
  - Obsługa niskopoziomowego wysyłania zdarzeń przez macOS Quartz / CoreGraphics (`CGEventPost`).
  - Dźwięki przerzutki odtwarzane przez systemowy silnik `afplay`.
  - Weryfikacja i asystent uprawnień systemowych Dostępności (`AXIsProcessTrusted`).
  - Automatyczne wykrywanie instalacji gry MyWhoosh przez indeks Spotlight (`mdfind`).
- **Eliminacja błędnej identyfikacji modeli (Zwift Click vs Play):**
  - Sprzętowa inspekcja serwisu GATT `0000fc82` i charakterystyk `00000100`–`00000102` rozwiązująca błąd firmware Nordic nRF, który w charakterystyce `0x2A00` fałszywie zgłaszał nazwę `"Zwift Play"`.
  - Trwałe zapamiętywanie wybranego profilu kontrolerów na obu platformach.
- **Niezawodne definiowanie przycisków & eliminacja podwójnej zmiany biegów (`PRO v1.6`):**
  - Zsynchronizowano zapis i odczyt własnych akcji w menu diagramu kokpitu.
  - Sprzętowo-programowy filtr drgań styków (*Debounce* 150 ms) zapobiegający podwójnym kliknięciom wywołanym wibracjami silniczka haptycznego.

---

## 10. Wsparcie projektu

Projekt **VeloBridge** jest tworzony i rozwijany z pasji do kolarstwa wirtualnego, treningu na trenażerze i inżynierii wstecznej Bluetooth Low Energy.

Jeśli program jest dla Ciebie przydatny i uprzyjemnia Ci treningi w MyWhoosh, możesz postawić autorowi wirtualną kawę:

☕ **[Postaw kawę na buycoffee.to/mkola](https://buycoffee.to/mkola)**

Dziękuję za każde wsparcie i do zobaczenia na wirtualnych trasach! 🚴💨
