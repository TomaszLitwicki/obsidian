Parametry w REST API
## Path Variable
`/users/3` zmienna wskazująca na użytkownika (user_id = 3)
## Query Parameter
`/users?gender=male` - filtrowanie po parametrze
`/users?sortBy=age` - sortowanie otrzymanych danych
`/users?number=100&start=300` - paginacja w sytuacji dużej ilość danych zwracanie ilościami
`/users?newsletter=true` - wyszukiwanie w zwracanych danych
`/users?format=xml` - zwracany konkretny format danych
`/users?gender=famale&sortBy=age&number50&start=100` - łączenie parametrów za pomocą &

pobieranie wartości parametru:
```Python
@app.route('/users/')
def users():
    gender = request.args.get('gender')

    if gender == 'male':
        return jsonify([u for u in all_users if not u.name.endswith('a')])
    elif gender == 'female':
        return jsonify([u for u in all_users if u.name.endswith('a')])

    return jsonify(all_users)
```

`request.args` to nie metoda, tylko obiekt.
Kropka w Pythonie oznacza „weź atrybut z obiektu”, więc czytamy to:
- `request` → obiekt reprezentujący bieżące żądanie HTTP (dostarczony przez Flask).
- `args` → słownik parametrów query (czyli tego, co pojawia się po ? w URL).
- `.get('gender')` → metoda get wyciąga wartość parametru gender z args.

To działa tak samo jak:
```
params = request.args          # np. ImmutableMultiDict({'gender': 'male'})
gender = params.get('gender')  # wynik: 'male'
```

Czyli `request.args` to:
- struktura danych typu `ImmutableMultiDict`,
- zawiera wszystkie parametry z URL, np.:
	`/users?gender=male&age=30

…będzie dostępne jako:
```
request.args['gender'] → 'male'
request.args['age'] → '30'
```

Dlaczego `request.args.get('gender')`, a nie np. `request.get('gender')`?
- `request` to duży obiekt z wieloma rzeczami (nagłówki, ciało, metoda, URL, itd.).
- `.args` to tylko podzbiór — parametry query.
- `.get()` jest metodą słownika: pozwala pobrać wartość i nie rzuci wyjątku, jeśli klucza nie ma.