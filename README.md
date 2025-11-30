# Kolej linowa – opis projektu i testy


## 1. Opis projektu
Celem projektu jest symulacja działania krzesełkowej kolei linowej, obsługującej pieszych i rowerzystów w okresie letnim. Kolej składa się z **72 krzesełek** (4-osobowych), przy czym jednocześnie zajętych może być maksymalnie **36**.

**Zasady wejścia i biletów:**
- Turyści przybywają losowo. Wstęp wymaga zakupu biletu (jednorazowy, czasowy, dzienny).
- **Zniżki (25%):** dzieci <10 lat oraz seniorzy >65 lat.
- **Opieka:** Dzieci <8 lat muszą być pod opieką dorosłego. Jeden dorosły może opiekować się max. dwojgiem dzieci w wieku 4–8 lat.
- **VIP (ok. 1%):** wchodzą na teren stacji bez kolejki (używając karnetu).

**Struktura stacji i przepływ:**
1. **Wejście na stację:** 4 bramki (kontrola biletów). Obowiązuje limit **N** osób przebywających na dolnej stacji (między wejściem a peronem).
2. **Wejście na peron:** 3 bramki obsługiwane przez **Pracownika1**.
3. **Zajmowanie miejsc:**
   - Max. 2 rowerzystów,
   - 1 rowerzysta + 2 pieszych,
   - 4 pieszych.
4. **Zjazdy:** Rowerzyści po wjeździe zjeżdżają jedną z trzech tras o czasach **T1, T2, T3** (T1<T2<T3).
5. **Wyjście:** Na górnej stacji (obsługa: **Pracownik2**) odbywa się dwoma drogami.

**Czas pracy i sterowanie:**
- Kolej działa w godzinach **Tp – Tk**.
- Po godzinie **Tk**: bramki nie działają, osoby z peronu są wywożone, po 3 sek. symulacja kończy się.
- **Sytuacje awaryjne:** Pracownik może zatrzymać kolej (**sygnał1**). Wznowienie (**sygnał2**) następuje po komunikacji między pracownikami.
- Każde użycie karnetu jest logowane, a na koniec dnia generowany jest raport zjazdów.

---

## 2. Plan testów

### Test 1 – Weryfikacja limitów krzesełek i zasad sadowienia
**Cel:** Sprawdzenie, czy na linii znajduje się maksymalnie 36 zajętych krzesełek oraz czy przestrzegane są reguły łączenia pasażerów.
**Scenariusz:**
- Duże natężenie turystów (piesi i rowerzyści).
- Sprawdzenie, czy `Pracownik1` nie wpuszcza więcej niż 2 rowerzystów na krzesełko.
- Weryfikacja, czy 1 dorosły nie zabiera więcej niż 2 dzieci (4-8 lat).
**Oczekiwany wynik:** Brak przepełnienia krzesełek, poprawne konfiguracje pasażerów, płynny ruch do limitu 36 krzesełek.

### Test 2 – Działanie limitu N i obsługa VIP
**Cel:** Sprawdzenie blokady wejścia po osiągnięciu limitu osób na stacji oraz priorytetu VIP.
**Scenariusz:**
- Ustawienie małego limitu `N` (np. 10 osób) przy dużym napływie turystów.
- Wprowadzenie turystów VIP w momencie pełnej kolejki.
**Oczekiwany wynik:** Turyści czekają przed 1. bramkami, gdy w środku jest N osób. Osoba VIP omija kolejkę oczekujących i wchodzi jako następna, gdy tylko zwolni się miejsce w strefie N.

### Test 3 – Zatrzymanie awaryjne i wznowienie (Sygnały)
**Cel:** Weryfikacja obsługi sygnałów i komunikacji między pracownikami.
**Scenariusz:**
- W trakcie pracy kolei wysłanie `sygnału1` (zatrzymanie).
- Próba wejścia nowych osób w trakcie awarii.
- Wymiana komunikatów między procesami pracowników i wysłanie `sygnału2` (wznowienie).
**Oczekiwany wynik:** Kolej staje (brak nowych krzesełek wyjeżdżających), a po procedurze wznowienia wraca do normalnej pracy bez utraty pasażerów.

### Test 4 – Zakończenie dnia pracy (Czas Tk)
**Cel:** Sprawdzenie poprawności zamykania systemu.
**Scenariusz:**
- Symulacja osiąga czas `Tk`.
- Na peronie wciąż znajdują się oczekujący pasażerowie.
**Oczekiwany wynik:**
- Bramki wejściowe natychmiast przestają wpuszczać (karnety nieaktywne).
- Osoby będące już na peronie zostają obsłużone (wywiezione na górę).
- Po obsłużeniu ostatnich osób i upływie 3 sekund wirtualnych procesy kończą działanie, a raport zostaje zapisany.
