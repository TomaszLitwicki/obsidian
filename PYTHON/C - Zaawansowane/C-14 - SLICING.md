czyli cięcie sekwencji (listy, stringi, tuple…)
odpowiednik [[C-13 - Moduł itertools]]

Ważne różnice względem #islice:

`lista[start:stop:step]` - działa tylko na sekwencjach (takich, które znają długość i mają indeksy).
`islice(iterable, start, stop, step)` -  działa też na iteratorach/generatorach (które nie mają indeksów ani długości).

Czyli:
Masz listę? – oba sposoby działają.
Masz generator? – tylko islice.

`lista[start:stop:step]

start – indeks, od którego zaczynamy (domyślnie 0)
stop – indeks, na którym się zatrzymujemy (ale nie wchodzi do wyniku)
step – krok (domyślnie 1)

# Przykłady

`L = [10, 20, 30, 40, 50, 60, 70]

Podstawowe cięcie
`print(L[2:5])   # [30, 40, 50]`   (od indeksu 2 do 4)

Od początku
`print(L[:3])    # [10, 20, 30]`   (domyślnie start = 0)

Do końca
`print(L[4:])    # [50, 60, 70]`

Całość (kopiowanie listy)
`print(L[:])     # [10, 20, 30, 40, 50, 60, 70]

Co drugi element
`print(L[::2])   # [10, 30, 50, 70]

Od końca
`print(L[::-1])  # [70, 60, 50, 40, 30, 20, 10]`   (czyli odwrócona lista)

Fragment od końca
`print(L[-3:])   # [50, 60, 70]`   (ostatnie 3 elementy)
`print(L[:-3])   # [10, 20, 30, 40]` (wszystko oprócz ostatnich 3)

| Wyrażenie na liście | Odpowiednik z `islice`        | Wynik                          | Komentarz                                                                 |
| ------------------- | ----------------------------- | ------------------------------ | ------------------------------------------------------------------------- |
| `L[:3]`             | `list(islice(L, 3))`          | `[10, 20, 30]`                 | od początku do indeksu 3 (bez 3)                                          |
| `L[2:5]`            | `list(islice(L, 2, 5))`       | `[30, 40, 50]`                 | od indeksu 2 do 4                                                         |
| `L[4:]`             | `list(islice(L, 4, None))`    | `[50, 60, 70]`                 | od indeksu 4 do końca                                                     |
| `L[::2]`            | `list(islice(L, 0, None, 2))` | `[10, 30, 50, 70]`             | co 2 krok                                                                 |
| `L[1:6:2]`          | `list(islice(L, 1, 6, 2))`    | `[20, 40, 60]`                 | od 1 do 5, co 2                                                           |
| `L[-3:]`            | ✖ brak prostego odpowiednika  | `[50, 60, 70]`                 | `islice` nie zna długości iteratora, więc nie obsługuje indeksów ujemnych |
| `L[::-1]`           | ✖ brak prostego odpowiednika  | `[70, 60, 50, 40, 30, 20, 10]` | `islice` nie odwraca iteratorów                                           |

