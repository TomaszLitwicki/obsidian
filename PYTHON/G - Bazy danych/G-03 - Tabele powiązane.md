Tabela "post" będzie powiązana z tabelą "user" za pomocą "user_id" - klucz obcy
FOREIGN KEYS:
	Reference Table->Ferign key details->Column->Referenced Column

## Podzapytania
```SQL
SELECT id, body, user_id FROM post WHERE user_id =1; {gdy znamy id}
SELECT id, body, user_id FROM post WHERE user_id = (SELECT id from user WHERE username = 'Cowboy');

SELECT id from user WHERE username = 'Cowboy'; {zwraca po prostu numer id}
```

## Złączenia tabel
JOIN:

```SQL
SELECT user.id, post.id, username, body FROM user
JOIN post ON user.id = post.user_id;

{z aliasami kolumn dla czytelności:}
SELECT user.id user_id, post.id post_id, username nazwa_użytkownika, body treść FROM user
JOIN post ON user.id = post.user_id;

{filtrowanie rekordów i pochodzenie kolumn:}
SELECT u.id, p.id, u.username, p.body FROM user u <-skrót do aliasu user spacja albo AS
JOIN post AS p ON u.id = p.user_id
WHERE u.username = 'Cowboy';
```

## GROUP BY i HAVING
służy do pogrupowania wyników.
```SQL
SELECT count(id) FROM post; {sprawdzanie ile jest postów}

{grupowanie otrzymanych rekordów}
SELECT user.username, COUNT(post.id) FROM post
JOIN user ON post.user_id = user.id
GROUP BY user.username;

{filtrowanie otrzymanych rekordów}
SELECT user.username, COUNT(post.id) number_of_posts FROM post
JOIN user ON post.user_id = user.id
GROUP BY user.username
HAVING number_of_posts > 2
```