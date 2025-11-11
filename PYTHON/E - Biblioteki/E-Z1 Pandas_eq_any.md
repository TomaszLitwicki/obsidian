# Metody `eq()` i `any()` w Pandas

## 1. `eq()`

Metoda `eq()` oznacza *equal (równe)*. Działa jak operator `==`, ale
jest bezpieczniejsza i wygodna przy pracy na całych kolumnach lub
DataFrame'ach.

### Składnia

``` python
DataFrame.eq(wartość)
Series.eq(wartość)
```

### Przykład

``` python
import pandas as pd

df = pd.DataFrame({
    'Star1': ['A', 'B', 'C'],
    'Star2': ['D', 'B', 'E']
})

print(df.eq('B'))
```

**Wynik:**

       Star1  Star2
    0  False  False
    1   True   True
    2  False  False

Każda komórka jest porównana z wartością `'B'`, a wynik to DataFrame z
wartościami logicznymi (`True` / `False`).

Można też porównywać dwie kolumny:

``` python
df['Star1'].eq(df['Star2'])
```

------------------------------------------------------------------------

## 2. `any()`

Metoda logiczna `any()` sprawdza, **czy w danym zakresie występuje choć
jedno `True`**.

### Składnia

``` python
DataFrame.any(axis=0 lub 1)
Series.any()
```

-   `axis=0` -- sprawdza w dół kolumny (domyślnie)
-   `axis=1` -- sprawdza w poziomie po wierszach

### Przykład

``` python
mask = df.eq('B')
print(mask.any(axis=1))
```

**Wynik:**

    0    False
    1     True
    2    False
    dtype: bool

Interpretacja:\
-- Drugi wiersz (`index = 1`) ma co najmniej jedno `True`.\
-- Pozostałe nie mają żadnego `True`.

Można to wykorzystać do filtrowania:

``` python
filtered = df[mask.any(axis=1)]
print(filtered)
```

------------------------------------------------------------------------

## 3. Połączenie `eq()` i `any()` w praktyce

W Twoim przykładzie:

``` python
cols = ['Star1', 'Star2', 'Star3', 'Star4']
actor = 'Robert De Niro'

mask = best_films[cols].eq(actor).any(axis=1)
```

To oznacza:

1.  `best_films[cols].eq(actor)` -- porównaj każdą komórkę w kolumnach
    `Star1`--`Star4` z nazwiskiem aktora.\
2.  `.any(axis=1)` -- jeśli **w którejkolwiek** kolumnie tego wiersza
    aktor się pojawi, zwróć `True`.\
3.  Wynik (`mask`) to seria `True/False`, która służy do filtrowania:

``` python
films = best_films.loc[mask, 'Series_Title']
```

Dzięki temu otrzymujesz wszystkie filmy, w których występuje dany aktor,
niezależnie od kolumny, w której jego nazwisko się pojawiło.


