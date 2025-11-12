# 1. ENDPOINT - rzeczowniki a nie czasowniki
NIE:
	~~/getUsers
	/users/add
	/users/delete
	/users/tomasz/edit~~

TAK: (od tego są metody HTTP)
	GET    /users
	POST   /users
	DELETE /users
	PATCH  /users


# 2. GET nie zmienia stanu
Służy tylko do pobierania. Trzymamy się funkcji metod! Do zmian stanu służą POST, PATCH, DELETE
```
POST   /users/01/activate
POST   /users/01?activate=true
DELETE /posts/102
```

# 3. Rzeczowniki
W liczbie mnogiej a nie pojedynczej (odwrotnie niż w bazach danych!)
TAK: /users/102
NIE: ~~/user/1~~

Bo wybieramy użytkownika 102 spośród wszystkich użytkowników, a nie pole numer jeden jakiegoś użytkownika.

# 4. Status kodu
Kody zgodnie z ich przeznaczeniem!
Szczegóły błędu w Response Body
- 404 NOT FOUND {"message": "Użytkownik nie został znaleziony!"}
- 401 UNAUTHORIZED {"message": "Błędne dane logowania!"}
- 400 BAD REQUEST {"message": "Błędny format daty urodzenia!"}
- 403 FORBIDDEN {"message": "Nie posiadasz wystarczających uprawnień do wykonania tej operacji!"}

# 5. Autoryzacja a Autentykacja
## Autoryzacja
Po autentykacji (już zalogowani) sprawdzenie wystarczających uprawnień do wykonywania danej operacji.
- 403 FORBIDDEN {"message": "Nie posiadasz wystarczających uprawnień do wykonania tej operacji!"}
## Autentykacja
Logowanie się do systemu. Login, Hasło -> autentykujemy się.
- 401 UNAUTHORIZED {"message": "Błędne dane logowania!"}

# 6. Zagnieżdżenie danych
wyświetlanie wszystkich postów użytkownika o id=102
NIE: ~~/users/7/posts~~
TAK: /posts?user_id=7
Używać query parameter, nie tworzyć zbędnych endpointów!

Prawidłowe zagnieżdżanie danych tylko w kontekście konkretnego użytkownika:
TAK: /users/102/friends

# 7. Filtrowanie i Paginacja
używać QUERY PARAMETERS:

`/users?page=1&page_size=30`

NIE: ~~/users/activated~~
TAK: /users?activated=true

