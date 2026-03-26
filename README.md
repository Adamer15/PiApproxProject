# PiApproxProject

Symulacja wizualna aproksymacji liczby π metodą zderzeń sprężystych dwóch bloków, zainspirowana filmem [3Blue1Brown](https://www.youtube.com/watch?v=jsYwFizhncE).

## Zasada działania

Dwa bloki poruszają się po osi poziomej i zderzają się ze sobą oraz ze ścianą. Całkowita liczba zderzeń (blok–blok oraz blok–ściana) przybliża liczbę π:

```
liczba_kolizji ≈ π × 10^k
```

gdzie `k` zależy od stosunku mas bloków. Jeśli masa małego bloku wynosi `m = 1`, a masa dużego `M = N × 100^k`, to liczba kolizji daje kolejne cyfry π.

**Przykłady:**

| Masa dużego bloku (N) | Liczba kolizji |
|-----------------------|----------------|
| 1                     | 3              |
| 100                   | 31             |
| 10 000                | 314            |
| 1 000 000             | 3 141          |

### Skąd bierze się π?

Stan układu w zmiennych pędu `(v₁√m₁, v₂√m₂)` leży na okręgu (zasada zachowania energii kinetycznej). Każde zderzenie odpowiada odbiciu na tym okręgu. Liczba odbić potrzebna do opuszczenia ćwiartki układu współrzędnych wynosi `⌊π / arctan(√(m/M))⌋`, co dla `m/M → 0` dąży do π × 10^k.

## Funkcje

- **Animacja 2D** — wizualizacja ruchu bloków w czasie rzeczywistym (Swing, ~60 FPS)
- **Licznik kolizji** — wyświetlany na ekranie na bieżąco
- **Wykres fazowy** — diagram `v₁√m₁` vs `v₂√m₂` rysujący kolejne punkty stanu układu (pomarańczowe punkty połączone żółtymi liniami tworzą łuk okręgu)
- **Prędkości bloków** — wyświetlane w czasie rzeczywistym
- **Eksport danych** — każde zderzenie jest zapisywane do pliku `.txt` ze znacznikiem czasu

## Wymagania

- **Java 21** (JDK 21+)
- **IntelliJ IDEA** (projekt skonfigurowany jako moduł `.iml`) lub dowolne środowisko obsługujące Javę 21

## Uruchomienie

### IntelliJ IDEA

1. Otwórz folder `PiApproxProject` jako projekt w IntelliJ IDEA.
2. Upewnij się, że JDK 21 jest skonfigurowane jako SDK projektu (`File → Project Structure → SDK`).
3. Uruchom klasę `Main` (`src/Main.java`).

### Wiersz poleceń

```bash
cd PiApproxProject
javac -d out src/*.java
java -cp out Main
```

## Sposób użycia

Po uruchomieniu aplikacja wyświetla okno dialogowe z prośbą o podanie **wartości początkowej** — jest to masa dużego bloku (liczba całkowita większa od 0).

| Pole      | Wartość                                      |
|-----------|----------------------------------------------|
| Mały blok | masa = 1, rozmiar = 75 px, prędkość = 0      |
| Duży blok | masa = N (podana przez użytkownika), rozmiar = 100 px, prędkość = −100 (w lewo) |

Symulacja startuje automatycznie po zatwierdzeniu wartości.

## Dane wyjściowe

Po każdym zderzeniu do pliku `Collisions_DD-MM-YYYY_HH-mm-ss.txt` (tworzonym w katalogu roboczym) dopisywana jest linia:

```
Nr kolizji    Prędkość po kolizji (blok 1)    Masa 1    Prędkość po kolizji (blok 2)    Masa 2
         1                            100.00        1                          -100.00   10000
         2                             ...
```

## Struktura projektu

```
PiApproxProject/
└── src/
    ├── Main.java              # Punkt wejścia — uruchamia SimulationFrame w wątku Swing
    ├── SimulationFrame.java   # Główne okno JFrame; pobiera masę od użytkownika
    ├── SimulationPanel.java   # Pętla symulacji (Timer 16 ms), zarządza stanem i rysowaniem
    ├── PhysicsEngine.java     # Obliczenia: zderzenie sprężyste, wykrywanie kolizji ze ścianą
    ├── Square.java            # Blok: pozycja, prędkość, masa, ruch, rysowanie
    ├── Draw.java              # Klasa abstrakcyjna dla wszystkich obiektów rysowanych
    ├── Line.java              # Linia (Graphics2D)
    ├── Point.java             # Punkt na wykresie fazowym
    ├── CoordinateSystem.java  # Układ współrzędnych wykresu fazowego
    └── WriteOn.java           # Zapis danych kolizji do pliku .txt
```

## Fizykę kolizji

Prędkości po zderzeniu sprężystym (zasada zachowania pędu i energii kinetycznej):

```
v₁' = ((m₁ - m₂) / (m₁ + m₂)) × v₁  +  (2m₂ / (m₁ + m₂)) × v₂
v₂' = (2m₁ / (m₁ + m₂)) × v₁        +  ((m₂ - m₁) / (m₁ + m₂)) × v₂
```

Odbicie od ściany: `v₁' = −v₁`

## Licencja

Projekt edukacyjny — brak formalnej licencji.
