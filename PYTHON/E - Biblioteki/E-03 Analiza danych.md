# Pandas
`pip install pandas`

`df - od Date Frame`
## Wyświetlanie danych
```Python
import pandas as pd

df = pd.read_csv('imdb_top_1000.csv')
print(df) # wyświetla 5 pierwszych i 5 ostatnich wierszy
print(df.head()) # wyświetla 5 pierwszych wierszy
print(df.tail()) # wyświetla 5 ostatnich wierszy
print(df.tail(n=10)) # wyświetla n=10 ostatnich wierszy

pd.set_option('display.max_rows', 20) #zmienia parametry wyświetlania
pd.set_option('display.min_rows', 20)
print(df) # wyświetla teraz 10 pierwszych i 10 ostatnich wierszy

pd.set_option('display.width', None) # wyświetla wszystkie kolumny
print(df['Director']) # wyświetla konkretną kolumnę po nazwie
print(df.Released_Year) # wyświetla konkretną kolumnę po nazwie
print(df.Released_Year[2]) # wyświetla konkretną kolumnę po nazwie i konkretny wiersz
```

```Python
print(df.shape)          # atrybut – daje krotkę (wiersze, kolumny)
print(df.columns)        # atrybut – lista nazw kolumn
print(df.dtypes)         # atrybut – typy danych kolumn

print(df.head())         # metoda – pokazuje pierwsze 5 wierszy
print(df.describe())     # metoda – robi statystyki opisowe
print(df.info())         # metoda – wypisuje informacje o DataFrame

```
## Filtrowanie danych
```Python
import pandas as pd

df = pd.read_csv('imdb_top_1000.csv')

df['Released_Year'] = pd.to_numeric(df['Released_Year'], errors='coerce')
filtered_df = df[df['Released_Year'] == 2000]
result_df = filtered_df[['Director', 'Series_Title']]
print(result_df)
```

```Python
# - Pomijanie wiersza gdy zmienna ma inny typ niż numeric:
	# - Zamiast przypisywania do nowej zmiennej:
df = df.dropna(subset=['Released_Year'])
	# - Można zrobić podstawienie w miejscu:
df.dropna(subset=['Released_Year'], inplace=True)

# - Albo można przypisać domyślną wartość:
df.fillna(0, inplace=True) #Ale to zamienia wszystkie wartości NaN
	# Więc lepiej podstawić tak:
df['Released_Year'] = df['Released_Year'].fillna(0)
df['Released_Year'] = df['Released_Year'].astype(int)
	# albo podstawić tak:
df['Released_Year'] = df['Released_Year'].fillna(9999).astype(int)

filtered_df = df[df['Released_Year'] >= 2019]
result_df = filtered_df[['Director', 'Series_Title', 'Released_Year']]
print(result_df)
```

```Python
film_count_per_year = df['Released_Year'].value_counts()
top_year = film_count_per_year.idxmax()     # rok o największej liczbie filmów
top_year_count = film_count_per_year.max()  # liczba filmów w tym roku
```
## Zliczanie danych
```Python
# Zliczanie wystąpień danej wartości
top_directors = df['Director'].value_counts()

print(top_directors.head(n=10))
print()
# Wyświetlanie ilości filmów konkretnego reżysera
print(director := 'Quentin Tarantino', top_directors.get(director, 'Brak DANYCH'))

# Lista filmów w roku, w którym powstało ich najwięcej
top_years = df['Released_Year'].value_counts()
print(top_years.head())

top_year = top_years.idxmax() # Ta
print(top_year)

films_in_bes_year = df[df['Released_Year'] == top_year]
print(films_in_bes_year[['Series_Title', 'Released_Year']])
```
## Sortowanie danych
```Python
# Sortowanie od najnowszego do najstarszego i alfabetycznie
sorted_year = df.sort_values(by=['Released_Year', 'Series_Title'], ascending=[False, True])
print(sorted_year[['Series_Title', 'Released_Year']].head(n=10))

# Sortowanie filmów z 2000 roku od najlepszego według IMDB
pd.set_option('display.width', None)
films_2000 = df[df['Released_Year'] == 2000].sort_values(by='IMDB_Rating', ascending=False)
print(films_2000[['Series_Title', 'Released_Year', 'IMDB_Rating', 'Meta_score', 'No_of_Votes']])

# Sortowanie filmów z 2000 roku od najlepszego według Meta_score
pd.set_option('display.width', None)
films_in_2000 = df[df['Released_Year'] == 2000].sort_values(by='Meta_score', ascending=False)
best_films_in_2000 = films_in_2000
print(best_films_in_2000[['Series_Title', 'Released_Year', 'Meta_score', 'IMDB_Rating', 'No_of_Votes']])
```

## Grupowanie danych
```Python
import pandas as pd

df = pd.read_csv('imdb_top_1000.csv')

df['Released_Year'] = pd.to_numeric(df['Released_Year'], errors="coerce")
df['Released_Year'] = df['Released_Year'].fillna(-1).astype(int)

# Filtrowanie roku z najwyższą średnią za filmy
mean_ratings_by_year = df.groupby('Released_Year')['IMDB_Rating'].mean()
print(mean_ratings_by_year.nlargest(1))

# Filtrowanie 5 lat z najwyższą średnią za filmy po 2000 roku
mean_ratings_after_2000 = df[df['Released_Year'] >= 2000].groupby('Released_Year')['IMDB_Rating'].mean()
print(mean_ratings_after_2000.nlargest(5))

# Filtrowanie 5 lat od 2000 z najwyższą średnią za filmy i tytuł najlepszego filmu w roku
mean_ratings_by_year = df[df['Released_Year'] >= 2000].groupby('Released_Year')['IMDB_Rating'].mean()
top_years = mean_ratings_by_year.nlargest(5)

	# Pomysł własny
for i in top_years.index:
    best_film_by_year = df[df['Released_Year'] == i].sort_values(by='IMDB_Rating', ascending=False).head(n=1)
    print(best_film_by_year[['Series_Title', 'Released_Year', 'IMDB_Rating']])

	# Pomysł z kursu
db_best_movies_each_year = pd.DataFrame() # Nowa baza danych

for year in top_years.index:
    best_movie_of_year = df[df['Released_Year'] == year].nlargest(1, 'IMDB_Rating')
    db_best_movies_each_year = pd.concat([db_best_movies_each_year, best_movie_of_year])

print(db_best_movies_each_year[['Series_Title', 'Released_Year', 'IMDB_Rating']])
```

```Python
# 10 aktorów występujący najczęściej w filmach z rankingiem powyżej 8.0
best_films = df[df['IMDB_Rating'] >= 8.0]
# .melt() - przekształca format kolumn z szerokiego na długi
all_actors = best_films[['Star1', 'Star2', 'Star3', 'Star4']].melt(value_name='Actor').drop('variable', axis=1)
most_actors = all_actors.value_counts().nlargest(10)
print(most_actors)

# Wyświetlanie wszystkich filmów w jakich zagrali
cols = ['Star1', 'Star2', 'Star3', 'Star4']
for actor, count in most_actors.items():
    actor_name = actor[0]
    rows = best_films[cols].eq(actor_name).any(axis=1)
    titles = best_films.loc[rows, 'Series_Title'].unique()
    print(f'{actor_name} ({count}) - {", ".join(sorted(titles))}')

# Ilość filmów w każdym roku
movie_counts = df.groupby('Released_Year').size()
print(movie_counts)
```

