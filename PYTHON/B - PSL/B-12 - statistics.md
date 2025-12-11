`import statistics as st`
Moduł **`statistics`** dostarcza podstawowych narzędzi statystyki opisowej dla list liczb.

---
## Średnie
 `mean(data)` - Średnia arytmetyczna.
 `fmean(data)` - Szybsza i bardziej precyzyjna średnia float.
 `geometric_mean(data)` - Średnia geometryczna.

---
## Mediany
 `median(data)` - Mediana (automatycznie obsługuje parzyste i nieparzyste zbiory).
 `median_low(data)` - Przy parzystych — niższy z dwóch środkowych elementów.
 `median_high(data)` - Przy parzystych — wyższy z dwóch środkowych elementów.
 `median_grouped(data, interval=1)` - Przybliżona mediana danych pogrupowanych.

---
## Dominanta (najczęstsza wartość)
 `mode(data)` - Najczęściej występująca wartość. Przy wielu dominantach rzuca wyjątek.
 `multimode(data)` - Lista wszystkich wartości modalnych.

---
## Rozrzut i wariancja
 `variance(data, xbar=None)` - Wariancja **próby**.
 `pvariance(data, xbar=None)` - Wariancja **populacji**.
 `stdev(data, xbar=None)` - Odchylenie standardowe **próby**.
 `pstdev(data, xbar=None)` - Odchylenie standardowe **populacji**.

---
## Ważne uwagi
* Puste listy → `StatisticsError`.
* `mode()` często powoduje wyjątek przy wielu dominantach → używaj `multimode()`.
* Wszystkie funkcje przyjmują iterowalne zbiory liczb (`list`, `tuple`, `set`, itp.).
