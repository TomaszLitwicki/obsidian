# MatPlotLib
`pip install matplotlib`
`import matplotlib.pyplot as plt`

```Python
"""Prezentacja danych"""
import matplotlib.pyplot as plt #konwencja
import pandas as pd

# Wczytanie danych
df = pd.read_csv('imdb_top_1000.csv')

# Filtrowanie i sortowanie danych
df['Released_Year'] = pd.to_numeric(df['Released_Year'], errors='coerce')
df.dropna(subset='Released_Year',inplace=True)
df['Released_Year'] = df['Released_Year'].astype(int)
df = df[df['Released_Year'] >= 1930]
movie_counts = df.groupby('Released_Year').size()

#Prezentacja danych
ax = movie_counts.plot(kind='bar', color='red', figsize=(10,5))
ax.set_xticks(range(0, len(movie_counts), 5))
ax.set_xticklabels(movie_counts.index[::5], rotation=45)
plt.title('Liczba filmów z każdego roku')
plt.xlabel('Rok')
plt.ylabel('Liczba filmów ')

plt.show()
```

Fajna wersja z uzupełnieniem brakujących lat i podstawieniem wartości 0 filmów w tym roku:
```Python
df['Released_Year'] = pd.to_numeric(df['Released_Year'], errors="coerce").dropna().astype(int)
movies_per_year = df.groupby('Released_Year').size().sort_index()

full_index = pd.RangeIndex(movies_per_year.index.min(), movies_per_year.index.max()+1)
movies_per_every_year = movies_per_year.reindex(full_index, fill_value=0)
```

Na koniec w ogóle odjazd szczególnie z tym MaxNLocator...
```Python
"""Prezentacja danych"""
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib.ticker import MaxNLocator

df['Runtime'] = df['Runtime'].str.replace(' min','').astype(int)
df['Runtime'] = pd.to_numeric(df['Runtime'])

full_index = pd.RangeIndex(movies_per_year.index.min(), movies_per_year.index.max()+1)
movies_per_every_year = movies_per_year.reindex(full_index, fill_value=0)

runtime = df['Runtime'].sort_values(ascending=True)

plt.hist(runtime, bins=10, color='blue', edgecolor='black')
plt.xlabel('Czas trwania filmu')
plt.ylabel('ilość filmów tej długości')
plt.gca().xaxis.set_major_locator(MaxNLocator(nbins=10))

plt.show()
```