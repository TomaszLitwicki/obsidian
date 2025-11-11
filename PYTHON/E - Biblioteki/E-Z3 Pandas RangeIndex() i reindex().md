## `RangeIndex()`

`RangeIndex` to specjalny, bardzo lekki typ indeksu w Pandas, działający podobnie do wbudowanej funkcji `range()` w Pythonie.

### Składnia

```python
pd.RangeIndex(start, stop=None, step=1, name=None)
```

### Parametry

* **start** – pierwszy element zakresu (włącznie)
* **stop** – ostatni element *nie włącznie*
* **step** – krok (domyślnie `1`)
* **name** – opcjonalna nazwa indeksu

### Działanie

Tworzy sekwencję liczbową, np. lat, dni czy ID rekordów, którą można przypisać jako indeks do `Series` lub `DataFrame`.

### Przykład

```python
import pandas as pd

idx = pd.RangeIndex(start=2000, stop=2005)
print(idx)
```

Wynik:

```
RangeIndex(start=2000, stop=2005, step=1)
```

### Przykład z reindex

```python
data = pd.Series([5, 3, 8], index=[2001, 2003, 2005])
full_index = pd.RangeIndex(2000, 2006)
filled = data.reindex(full_index, fill_value=0)
```

Wynik:

```
2000    0
2001    5
2002    0
2003    3
2004    0
2005    8
```

---

## `reindex()`

`reindex()` dopasowuje dane do nowego zestawu indeksów lub kolumn, dodając brakujące elementy i wypełniając je zadaną wartością.

### Składnia

```python
Series.reindex(index=None, fill_value=None, method=None)
DataFrame.reindex(index=None, columns=None, fill_value=None, method=None)
```

### Najważniejsze parametry

* **index** – nowy zestaw etykiet dla osi wierszy
* **columns** – nowy zestaw kolumn (dla DataFrame)
* **fill_value** – wartość dla brakujących miejsc
* **method** – sposób wypełnienia braków:

  * `'ffill'` – użyj ostatniej znanej wartości
  * `'bfill'` – użyj następnej znanej wartości

### Przykład

```python
import pandas as pd

data = pd.Series([10, 20, 30], index=[2001, 2003, 2005])
new_index = pd.RangeIndex(2000, 2006)
reindexed = data.reindex(new_index, fill_value=0)
```

Wynik:

```
2000     0
2001    10
2002     0
2003    20
2004     0
2005    30
```

### Wariant z metodą wypełnienia

```python
data.reindex(range(2000, 2006), method='ffill')
```

Wypełni brakujące wartości ostatnią znaną daną (forward fill).

---

## Podsumowanie

| Metoda         | Zastosowanie                                         | Kluczowy efekt                                                   |
| -------------- | ---------------------------------------------------- | ---------------------------------------------------------------- |
| `RangeIndex()` | Tworzenie zakresu indeksów (np. lat, dni)            | Lekki, szybki indeks liczbowy                                    |
| `reindex()`    | Dopasowanie danych do nowego zestawu indeksów/kolumn | Dodaje brakujące wiersze/kolumny i wypełnia je wartością (np. 0) |

Przykład połączenia obu metod:

```python
movies_per_year_full = movies_per_year.reindex(
    pd.RangeIndex(movies_per_year.index.min(), movies_per_year.index.max() + 1),
    fill_value=0
)
```
