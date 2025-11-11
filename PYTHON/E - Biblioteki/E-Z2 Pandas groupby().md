`groupby()` w **pandas** służy do grupowania danych — czyli dzielenia DataFrame’a na mniejsze części według wartości w jednej lub kilku kolumnach, a następnie wykonywania operacji (np. sumowania, liczenia, średniej) na każdej z tych grup osobno.

To bardzo przydatne narzędzie w analizie danych, zwłaszcza przy dużych zbiorach, gdzie chcesz np. zobaczyć średnią ocenę filmów dla każdego reżysera, łączną sprzedaż w każdym roku itp.

---

### Składnia:

```python
df.groupby('kolumna')
df.groupby(['kolumna1', 'kolumna2'])
```

Zazwyczaj po grupowaniu wykonujesz jakąś **agregację**, np. `.sum()`, `.mean()`, `.count()` itp.

---

### Przykład praktyczny:

Załóżmy, że masz taki DataFrame:

```python
import pandas as pd

data = {
    'Director': ['Nolan', 'Nolan', 'Spielberg', 'Tarantino', 'Nolan', 'Spielberg'],
    'Movie': ['Inception', 'Interstellar', 'Jaws', 'Pulp Fiction', 'Dunkirk', 'E.T.'],
    'IMDB_Rating': [8.8, 8.6, 8.0, 8.9, 7.9, 7.8],
    'Year': [2010, 2014, 1975, 1994, 2017, 1982]
}

df = pd.DataFrame(data)
```

---

### 1. Średnia ocen filmów każdego reżysera:

```python
avg_rating = df.groupby('Director')['IMDB_Rating'].mean()
print(avg_rating)
```

**Wynik:**

```
Director
Nolan        8.433333
Spielberg    7.900000
Tarantino    8.900000
Name: IMDB_Rating, dtype: float64
```

---

### 2. Ile filmów zrobił każdy reżyser:

```python
count_movies = df.groupby('Director')['Movie'].count()
print(count_movies)
```

**Wynik:**

```
Director
Nolan        3
Spielberg    2
Tarantino    1
Name: Movie, dtype: int64
```

---

### 3. Kilka operacji naraz (agregacja wielokrotna):

```python
summary = df.groupby('Director').agg({
    'IMDB_Rating': ['mean', 'max', 'min'],
    'Movie': 'count'
})
print(summary)
```

**Wynik:**

```
            IMDB_Rating             Movie
                   mean  max  min  count
Director
Nolan              8.43  8.8  7.9      3
Spielberg          7.90  8.0  7.8      2
Tarantino          8.90  8.9  8.9      1
```

---

### 4. Dostęp do konkretnej grupy:

```python
grouped = df.groupby('Director')
print(grouped.get_group('Nolan'))
```

**Wynik:**

```
  Director        Movie  IMDB_Rating  Year
0    Nolan    Inception          8.8  2010
1    Nolan  Interstellar          8.6  2014
4    Nolan       Dunkirk          7.9  2017
```
