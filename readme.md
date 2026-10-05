# 🚴 VeloBridge Pro — Virtual Shifting dla MyWhoosh
### Kompleksowy podręcznik użytkownika, instrukcja konfiguracji i opis techniczny

---

## 📖 Spis treści
1. [O projekcie VeloBridge](#1-o-projekcie-velobridge)
2. [Kluczowe możliwości](#2-kluczowe-możliwości)
3. [Wymagania systemowe i sprzętowe](#3-wymagania-systemowe-i-sprzętowe)
4. [Szybki start (Krok po kroku)](#4-szybki-start-krok-po-kroku)
5. [Konfiguracja ścieżki do gry MyWhoosh (Microsoft Store & MyWhoosh HD)](#5-konfiguracja-ścieżki-do-gry-mywhoosh-microsoft-store--mywhoosh-hd)
6. [Domyślne sterowanie i mapa przycisków](#6-domyślne-sterowanie-i-mapa-przycisków)
7. [Plik konfiguracyjny (VeloBridge_config.json)](#7-plik-konfiguracyjny-velobridge_configjson)
8. [Rozwiązywanie problemów (FAQ)](#8-rozwiązywanie-problemów-faq)
9. [Wsparcie projektu](#9-wsparcie-projektu)

---

## 1. O projekcie VeloBridge

**VeloBridge** to lekka, samodzielna aplikacja typu **Portable (bez konieczności instalacji)** dla systemu Windows, która łączy bezprzewodowe kontrolery **Zwift Play** oraz **Zwift Click** z grą **MyWhoosh**.

Aplikacja implementuje pełny protokół kryptograficzny BLE kontrolerów Zwift i tłumaczy stany przycisków oraz łopatek na bezpośrednie kody sprzętowe **DirectInput** (Hardware Scan Codes). Pozwala to na natychmiastowe korzystanie z wirtualnej zmiany biegów (**Virtual Shifting / MyShift**) oraz pełnego sterowania rowerzystą (skręcanie, zawracanie, kamera, powerupy, menu) w silniku Unreal Engine gry MyWhoosh.

---

## 2. Kluczowe możliwości

- ⚡ **Wirtualna zmiana biegów (MyShift 30 przełożeń):** Zmiana biegów za pomocą łopatek lub przycisków `+` / `–` kontrolerów (bieg startowy: 15).
- 🔒 **Bezpieczna komunikacja Bluetooth Low Energy (BLE):**
  - Wymiana kluczy **ECDH** na krzywej eliptycznej `SECP256R1` (NIST P-256).
  - Derywacja kluczy symetrycznych **HKDF-SHA256**.
  - Dwukierunkowe szyfrowanie pakietów **AES-256-CCM** z 4-bajtowym tagiem uwierzytelniającym MIC.
- 🎮 **Automatyczne rozpoznawanie sprzętu:**
  - Podłączenie kontrolerów **Zwift Play** automatycznie aktywuje widok i mapowanie Zwift Play oraz blokuje profil Click.
  - Podłączenie kontrolerów **Zwift Click** automatycznie przełącza widok i mapowanie na Zwift Click oraz blokuje profil Play.
- ⌨️ **DirectInput 60Hz Hardware Scan Codes:** Symulacja fizycznych wciśnięć klawiszy (Windows `SendInput`), działająca w 100% bezkolizyjnie z grami 3D.
- 🔊 **Realistyczny dźwięk kliku manetki:** Mechaniczny odgłos indeksowanej przerzutki rowerowej generowany w tle przy każdej zmianie biegu.
- 📳 **Haptyka (wibracje):** Sygnalizacja zmiany przełożenia wibracją manetki Zwift.
- 🪟 **Pływający mini-HUD:** Zawsze widoczne, półprzezroczyste okienko z aktualnym biegiem i poziomem baterii obu manetek podczas jazdy w MyWhoosh.
- 🎨 **Interaktywny kokpit Rider's POV:** Wizualizacja kierownicy i manetek z perspektywy rowerzysty z podświetlaniem klikanych przycisków w czasie rzeczywistym.

---

## 3. Wymagania systemowe i sprzętowe

- **System operacyjny:** Windows 10 (64-bit) lub Windows 11.
- **Moduł Bluetooth:** Wbudowany moduł Bluetooth 4.0+ lub adapter Bluetooth USB BLE.
- **Kontrolery:**
  - **Zwift Play** (lewa + prawa manetka montowane na kierownicy szosowej), LUB
  - **Zwift Click** (pojedyncza manetka lub zestaw lewa + prawa montowane na obejmach).
- **Gra:** MyWhoosh (wersja ze sklepu Microsoft Store lub instalator standalone MyWhoosh HD).
- **Trenażer (opcjonalnie):** Dowolny trenażer smart lub zestaw z pojedynczą zębatką Zwift Cog (14T).

---

## 4. Szybki start (Krok po kroku)

1. **Uruchomienie:**
   - Pobierz i uruchom plik `VeloBridge.exe`.
   - Program jest w 100% portable – nie wymaga instalatora ani uprawnień administratora.
2. **Wybudzenie kontrolerów:**
   - Naciśnij dowolny przycisk na manetkach Zwift Play lub Zwift Click.
   - Diody LED na kontrolerach zaczną migać, sygnalizując gotowość do połączenia BLE.
3. **Wykrycie i sparowanie:**
   - Jeśli uruchamiasz program po raz pierwszy, kliknij **„Skaner BLE”** w karcie stanu połączenia.
   - Kliknij **„🔍 Skanuj BLE”** – program wykryje Twoje manetki.
   - Przypisz znalezione urządzenia jako Lewą i Prawą manetkę, a następnie kliknij **„✓ Zapisz i zastosuj”**.
   - Adresy MAC zapiszą się w pliku `VeloBridge_config.json`.
4. **Połączenie i start gry:**
   - Kliknij duży przycisk **„⚡ Połącz i Uruchom MyWhoosh”**.
   - Po 1–2 sekundach diody stanu połączenia zapalą się na zielono (**POŁĄCZONO I ZASZYFROWANO**), a obok pojawi się poziom naładowania baterii.
   - VeloBridge automatycznie uruchomi grę MyWhoosh i aktywuje DirectInput.
5. **Trening:**
   - Rozpocznij jazdę w MyWhoosh. Wirtualne biegi MyShift działają od razu!

---

## 5. Konfiguracja ścieżki do gry MyWhoosh (Microsoft Store & MyWhoosh HD)

VeloBridge posiada zaawansowany silnik automatycznego wykrywania gry MyWhoosh. Jednak w zależności od wersji systemu Windows i sposobu instalacji (Microsoft Store lub MyWhoosh HD), możesz chcieć wskazać własną ścieżkę lub skrót.

W sekcji stanu połączenia obok napisu *MyWhoosh (Gra)* kliknij przycisk **„📁 Ścieżka gry”**. Otworzy się dedykowane okno konfiguracji:

### A. Wersja ze sklepu Microsoft Store (UWP / MSIX)
Aplikacje instalowane z Microsoft Store znajdują się w zabezpieczonym folderze systemowym `C:\Program Files\WindowsApps`, do którego eksplorator plików blokuje bezpośredni dostęp. Aby wskazać grę w VeloBridge:

- **Metoda 1 — Przeciągnięcie z Menu Start (Najprostsza):**
  1. Otwórz **Menu Start** w systemie Windows.
  2. Wyszukaj lub znajdź na liście aplikacji pozycję **MyWhoosh**.
  3. Kliknij i przytrzymaj lewym przyciskiem myszy ikonę MyWhoosh, a następnie **przeciągnij ją na Pulpit**.
  4. System Windows automatycznie utworzy na Pulpicie skrót `.lnk` do gry.
  5. W oknie VeloBridge kliknij **„📁 Przeglądaj...”** i wskaż utworzony skrót z Pulpitu!
- **Metoda 2 — Folder powłoki shell:AppsFolder:**
  1. Naciśnij skrót klawiaturowy `Win + R`.
  2. Wpisz `shell:AppsFolder` i naciśnij `Enter` (otworzy się lista wszystkich zainstalowanych aplikacji Store).
  3. Znajdź kafelek **MyWhoosh**, kliknij go prawym przyciskiem myszy i wybierz **„Utwórz skrót”** (Windows zapyta o umieszczenie go na pulpicie – zatwierdź).
  4. Wskaż ten skrót w VeloBridge.
- **Metoda 3 — Pełna automatyczna detekcja:**
  - Jeśli nie wskażesz żadnej ścieżki (pole puste), VeloBridge automatycznie odpyta system Windows poprzez zapytanie PowerShell `Get-StartApps` o identyfikator aplikacji Store i uruchomi ją bezpośrednio przez protokół powłoki.

### B. Wersja MyWhoosh HD / Standalone (Instalator ze strony www)
Jeśli pobrałeś grę jako instalator tradycyjny:
1. W oknie VeloBridge kliknij **„📁 Przeglądaj...”**.
2. Przejdź do folderu instalacji, np.:
   - `C:\Program Files\MyWhoosh\MyWhoosh.exe`
   - `C:\Program Files\MyWhoosh HD\MyWhoosh.exe`
   - `C:\Users\<Twój_Użytkownik>\AppData\Local\MyWhoosh\...`
3. Wybierz plik `.exe` gry (lub wskaż skrót `.lnk` z Pulpitu) i kliknij **„✓ Zapisz i zastosuj”**.

> **Wskazówka:** W oknie konfiguracji ścieżki możesz w każdej chwili kliknąć przycisk **„🚀 Przetestuj uruchomienie”**, aby sprawdzić, czy gra otwiera się poprawnie, lub **„🔄 Wykrywaj automatycznie”**, aby przywrócić domyślne wyszukiwanie.

---

## 6. Domyślne sterowanie i mapa przycisków

### 🎮 Zestaw Zwift Play (Montowane na baranku kierownicy)

| Kontroler | Przycisk / Element | Domyślna akcja VeloBridge | Klawisz w MyWhoosh (DirectInput) |
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

> **Własne mapowanie:** W oknie głównym aplikacji kliknij dowolny dymek z nazwą akcji na schemacie manetek, aby zmienić przypisanie na dowolną inną funkcję (np. zmiana kamery `C`, emotki `1`–`5`, podwójna redukcja biegów).

---

## 7. Plik konfiguracyjny (VeloBridge_config.json)

Wszystkie ustawienia programu są trwale przechowywane w czytelnym pliku formatu JSON o nazwie **`VeloBridge_config.json`**.

### Lokalizacja pliku:
1. **Tryb przenośny (Portable):** Bezpośrednio w katalogu z plikiem `VeloBridge.exe`.

### Przykładowa zawartość:
```json
{
    "device_mode": "Zestaw Zwift Play (Lewy + Prawy)",
    "mac_right": "F5:9D:F1:42:AA:92",
    "mac_left": "D7:A3:71:29:77:EC",
    "min_gear": 1,
    "max_gear": 30,
    "initial_gear": 15,
    "sound_enabled": true,
    "haptic_enabled": true,
    "mywhoosh_path": "C:\\Program Files\\MyWhoosh\\MyWhoosh.exe",
    "mappings": { ... }
}
```

---

## 8. Rozwiązywanie problemów (FAQ)

### ❓ Manetki nie chcą się połączyć, a dioda LED cały czas miga:
- **Przyczyna:** Manetki mogą być połączone z inną aplikacją w pobliżu (np. aplikacją Zwift lub Zwift Companion na smartfonie/tablecie).
- **Rozwiązanie:** Wyłącz na chwilę Bluetooth w telefonie lub zamknij aplikację Zwift. Manetki BLE mogą utrzymywać aktywne połączenie tylko z jednym odbiornikiem jednocześnie.

### ❓ Gra MyWhoosh nie reaguje na wciskanie przycisków zmiany biegów:
- **Przyczyna:** Okno gry MyWhoosh nie jest oknem aktywnym na pierwszym planie Windows.
- **Rozwiązanie:** Kliknij myszką wewnątrz okna gry MyWhoosh. VeloBridge wysyła kody DirectInput bezpośrednio do aktywnego okna.

### ❓ Prawa manetka redukuje biegi, a lewa wrzuca twardszy (odwrotne działanie):
- **Rozwiązanie:** W karcie „STAN POŁĄCZENIA BLE” w stopce kliknij przycisk **„⇄ Zamień L ↔ P”**. Program natychmiast zamieni role kontrolerów bez potrzeby ponownego parowania.

### ❓ Jak zresetować ustawienia do fabrycznych?
- Usuń plik `VeloBridge_config.json` z katalogu programu i uruchom VeloBridge ponownie.

---

## 9. Wsparcie projektu

Projekt **VeloBridge** jest tworzony i rozwijany z pasji do kolarstwa wirtualnego, treningu na trenażerze i inżynierii wstecznej Bluetooth Low Energy.

Jeśli program jest dla Ciebie przydatny i uprzyjemnia Ci treningi w MyWhoosh, możesz postawić autorowi wirtualną kawę:

☕ **[Postaw kawę na buycoffee.to/mkola](https://buycoffee.to/mkola)**

Dziękuję za każde wsparcie i do zobaczenia na wirtualnych trasach! 🚴💨
