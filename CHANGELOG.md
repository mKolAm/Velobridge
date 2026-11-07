# ?? Changelog (Historia zmian)

Wszystkie istotne zmiany, usprawnienia oraz poprawki wprowadzone w projekcie zosta?y opisane poni?ej.

---

## [1.6.0] - Poprawka mapowania definicji przycisk車w i eliminacja podw車jnej zmiany bieg車w (PRO v1.6)

### ?? Poprawki b??d車w i niezawodno?? sterowania
- **Naprawa zapisywania i wykonywania w?asnych definicji przycisk車w:**
  - Rozwi?zano problem nieskutecznej zmiany definicji akcji przycisk車w w menu diagramu kokpitu. Procedura przypisywania (`_assign_action`) oraz generowania etykiet (`_create_callout`) jest teraz w 100% zsynchronizowana z aktywnym modelem sprz?towym (`selected_hardware_model`), eliminuj?c zapis do niew?a?ciwego profilu (`play` vs `click`).
  - Usuni?to agresywny mechanizm fallback w obs?udze przycisk車w, kt車ry wymusza? redukcj? biegu (`SHIFT_DOWN`) na przyciskach wy??czonych (`NONE`) lub niewyszukanych w profilu domy?lnym. Wybranie opcji `Brak akcji (Wy??czony)` jest teraz ?ci?le respektowane.
- **Eliminacja podw車jnej zmiany bieg車w na prawej manetce (przyciski Y i B):**
  - Wprowadzono sprz?towo-programowy filtr drga里 styk車w (*Debounce* 150 ms) dla ka?dego przycisku fizycznego oraz wewn?trzny bufor czasowy w silniku zmiany bieg車w (`VirtualShifter`), zapobiegaj?c wielokrotnemu rejestrowaniu pojedynczego klikni?cia wywo?anemu mechanicznym rezonansem silniczka wibracyjnego (*Haptic Feedback*) umieszczonego w korpusie prawej manetki.
  - Naprawiono p?tl? rozpakowywania stan車w przycisk車w w pakietach wielostanowych BLE (kod operacji `0x23`), gwarantuj?c niezale?ne przypisanie roli (`left` i `right`) i eliminuj?c sztuczne naprzemienne zdarzenia wci?ni?cia/zwolnienia.
- **Weryfikacja i synchronizacja zmiany bieg車w na lewej manetce:**
  - Zapewniono jednoznaczne mapowanie kierunk車w zmiany bieg車w: Strza?ka w g車r? (↖) -> `SHIFT_UP` (+1 bieg), Strza?ka w d車? (��) oraz ?opatka -> `SHIFT_DOWN` (-1 bieg).
  - Wprowadzono pe?n? swobod? przedefiniowania dowolnego przycisku lewej manetki na sterowanie (skr?t w lewo/prawo, pauza, kamera, etc.).
- **Rozbudowa pakietu test車w jednostkowych (`23/23 test車w OK`):**
  - Dodano `test_virtual_shifter_debounce_prevents_double_shift` (test filtra drga里 styk車w zapobiegaj?cego podw車jnej zmianie bieg車w).
  - Dodano `test_button_redefinition_and_execution_logic` (test separacji profili modeli Play i Click oraz izolacji definicji przycisk車w).

---

## [1.5.0] - Poprawka stanu po??czenia i synchronizacja baterii (PRO v1.5)

### ?? Poprawki b??d車w i synchronizacja UI
- **Eliminacja pozornego stanu po??czenia i fa?szywych warto?ci baterii przy wy??czonych manetkach:**
  - Usuni?to statyczne warto?ci demonstracyjne w oknie ?STAN PO??CZENIA§ oraz g車rnym pasku (`MOST AKTYWNY`, `Po??czono i zaszyfrowano`, `92%`, `88%`).
  - Wprowadzono rzeczywisty stan pocz?tkowy po starcie programu: wska?nik g?車wny `♀ MOST ROZ??CZONY`, wygaszone szare punkty LED (`♀`), etykiety stanu `Roz??czono 〞 naci?nij przycisk` oraz stan baterii `?? --%`.
  - Wprowadzono pe?ne czyszczenie i wygaszanie wska?nik車w baterii (`?? --%`) oraz link車w BLE w momencie utraty sygna?u lub klikni?cia `? Roz??cz`.
  - Zsynchronizowano wy?wietlanie baterii na p?ywaj?cym HUD (`MiniOverlayWindow`) oraz dodano automatyczne od?wie?anie schematu kokpitu (`_render_controller_diagram`) przy ka?dej zmianie stanu po??czenia.
  - Rozszerzono pakiet test車w jednostkowych o weryfikacj? czyszczenia stanu po??czenia i baterii (`test_initial_and_disconnect_state_reset`).

---

## [1.4.0] - Stabilno?? architektury, ujednolicenie zasob車w i dokumentacja

### ? Usprawnienia
- **Centralizacja wersjonowania (`PRO v1.4`) i architektury pakietu:**
  - Ujednolicono identyfikatory aplikacji Windows (`AppUserModelID`), nag?車wki okna g?車wnego oraz okna pomocy technicznej.
  - Zoptymalizowano konfiguracj? kompilacji `PyInstaller` (`VeloBridge.spec` oraz `build_portable.py`) w oparciu o przeno?ne ?cie?ki wzgl?dne.
  - Rozbudowano zestaw automatycznych test車w jednostkowych (`20/20 test車w`) weryfikuj?cych kryptografi? ECDH/AES-CCM, dekodowanie pakiet車w BLE, blokady wsp車?bie?no?ci GATT oraz wyzwalanie Menu/Pauzy w MyWhoosh.

---

## [1.3.0] - Obs?uga przycisku Menu / Pauzy w MyWhoosh oraz stabilno?? po??czenia BLE

### ? Nowe funkcje
- **Wielowarstwowa obs?uga przycisku Menu / Pauzy w MyWhoosh (`Menu / Pauza`, `Pauza treningu`, `Menu + Pauza treningu`):**
  - Automatyczne wybudzanie ukrytego interfejsu HUD w MyWhoosh oraz precyzyjne symulowanie klikni?cia lewego dolnego przycisku **Menu / Pauza** w oknie gry (z natychmiastowym przywr車ceniem oryginalnej pozycji kursora myszy).
  - R車wnoleg?e wysy?anie hybrydowego sygna?u klawisza `Escape` (sprz?towy ScanCode `0x01` + wirtualny klawisz `VK_ESCAPE 0x1B` przez `SendInput` oraz bezpo?redni komunikat `PostMessageW` do okna gry).
  - Dodano nowe akcje do wyboru w mapowaniu przycisk車w:
    - **Menu / Pauza [Menu MyWhoosh + Esc]** 每 wywo?uje menu g?車wne/pauz? podczas swobodnej jazdy (*Free Ride*).
    - **Pauza treningu [Klawisz Spacja]** 每 zatrzymuje aktywny blok w trybie treningu strukturalnego (*Workout*).
    - **Menu + Pauza treningu [Menu + Esc + Spacja]** 每 uniwersalna akcja ??cz?ca oba tryby.
- **Niezawodne przekazywanie fokusu do okna MyWhoosh (`AttachThreadInput`):**
  - Przed wys?aniem skr車tu klawiszowego aplikacja automatycznie przywraca fokus do okna gry MyWhoosh, dzi?ki czemu zmiana bieg車w i pauza dzia?aj? nawet wtedy, gdy u?ytkownik wcze?niej klikn?? poza okno gry.

### ??? Stabilno?? po??czenia Bluetooth Low Energy (BLE)
- **Automatyczne wznawianie po??czenia (Auto-Reconnect):**
  - W przypadku chwilowego zaniku sygna?u lub roz??czenia pojedynczej manetki podczas treningu, silnik BLE automatycznie przechodzi w tryb cichego wznawiania po??czenia w tle (do 8 pr車b z inteligentnym odst?pem czasowym), przywracaj?c szyfrowan? sesj? bez przerywania jazdy.
- **Kaskadowe (sekwencyjne) ??czenie zestawu Zwift Play:**
  - Podczas jednoczesnego ??czenia obu manetek (`CONNECT_BOTH`) wprowadzono 1,2-sekundow? separacj? faz parowania ECDH i odkrywania us?ug GATT mi?dzy prawym a lewym kontrolerem, co eliminuje kolizje pakiet車w na pojedynczym adapterze Bluetooth w systemie Windows.
- **Aktywne podtrzymywanie sesji (BLE Keepalive):**
  - Dodano cykliczny odczyt stanu baterii co 45 sekund utrzymuj?cy aktywny nadz車r ??cza (*Link Supervision Timeout*) i zapobiegaj?cy usypianiu po??czenia przez system Windows podczas d?ugiej jazdy bez zmiany bieg車w.
- **Ochrona zapis車w GATT i debouncing wibracji:**
  - Wprowadzono blokad? wsp車?bie?nych zapis車w (`asyncio.Lock`) oraz ogranicznik cz?stotliwo?ci impuls車w haptycznych (140 ms), zapobiegaj?ce przeci??eniu stosu Bluetooth WinRT przy bardzo szybkiej, wielokrotnej zmianie prze?o?e里.

---

## [1.2.0] - Konfigurator ?cie?ki MyWhoosh i od?wie?ony interfejs

### ? Nowe funkcje i usprawnienia
- **Konfigurator ?cie?ki do gry MyWhoosh (`?? ?cie?ka gry`):**
  - Dodano okno pozwalaj?ce r?cznie wskaza? plik wykonywalny `.exe` lub skr車t `.lnk` do gry (w tym obs?ug? wersji **Microsoft Store**, identyfikator車w `shell:AppsFolder` oraz **MyWhoosh HD**) z trwa?ym zapisem w pliku `VeloBridge_config.json`.
  - Dodano przyciski automatycznego wykrywania instalacji (`?? Wykryj automatycznie`) oraz natychmiastowego testu uruchomienia gry (`?? Testuj uruchomienie`).
  - Rozbudowano wbudowan? instrukcj? oraz dokumentacj? o szczeg車?owy poradnik tworzenia i wyszukiwania skr車t車w dla wersji Microsoft Store oraz MyWhoosh HD.
- **Od?wie?ony uk?ad interfejsu i identyfikacji wizualnej:**
  - Zaktualizowano logo oraz ikon? aplikacji, zast?piono dolny prawy panel estetycznym przyciskiem wsparcia projektu (`? Postaw kaw? na buycoffee.to/mkola`) i uporz?dkowano g車rny pasek narz?dziowy.

---

## [1.1.0] - Obs?uga zestawu Zwift Click (Lewa + Prawa) i ergonomia UI

### ? Nowe funkcje i usprawnienia
- **Pe?na obs?uga dwucz??ciowego zestawu Zwift Click (Lewa + Prawa manetka):**
  - Dodano dekodowanie pakiet車w `0x23` (32-bitowa mapa bitowa przycisk車w dla sparowanych kontroler車w) oraz `0x07` dla obu stron kierownicy:
    - **Prawa manetka:** `Plus (+)` (Bieg w g車r?), `Y`, `Z`, `A`, `B`.
    - **Lewa manetka:** `Minus (?)` (Bieg w d車?), strza?ki kierunkowe `↖`, `��`, `?`, `?`.
  - Obs?uga mostkowego trybu parowania Zwift Click (komunikacja obu przycisk車w przez g?車wne po??czenie BLE prawej manetki bez wymogu osobnego parowania lewego modu?u).
- **Inteligentne prze??czanie widoku i blokada nieaktywnego zestawu:**
  - Automatyczne rozpoznawanie pod??czonego sprz?tu (Zwift Play vs Zwift Click) w momencie po??czenia oraz natychmiastowe prze??czenie wizualizacji kokpitu.
  - Automatyczna blokada prze??cznika nieu?ywanego modelu na czas aktywnego po??czenia i odblokowanie wyboru po roz??czeniu.
- **Filtracja pakiet車w Heartbeat (`0x15` / `0x19`):**
  - Wyeliminowano fa?szywe wyzwalanie akcji przez pakiety podtrzymuj?ce po??czenie wysy?ane cyklicznie przez kontrolery.
