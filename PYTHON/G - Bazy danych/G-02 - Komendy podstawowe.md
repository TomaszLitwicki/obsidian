 `USE nazwa_bazy_danych;`
`SELECT * FROM nazwa_tabeli;`
ctrl + Enter - wykonanie kodu.

## Kolejność
### wpisywania
SELECT → FROM → JOIN ON→ WHERE → AGREGACJE → GROUP BY → HAVING → ORDER BY → LIMIT

### działania SQL
FROM/JOIN → WHERE → GROUP BY → AGREGACJE → HAVING → SELECT → ORDER BY → LIMIT

1. FROM/JOIN → Zbierz dane
2. WHERE → Odfiltruj pojedyncze wiersze
3. GROUP BY → Zbij wiersze w grupę
4. AGREGACJE → Policz coś dla każdej grupy
5. HAVING → Odfiltruj całe grupy
6. SELECT → Wybierz co wyświetlić
7. ORDER BY → Jak to posortować
8. LIMIT → Ile wyświetlić
## Wyświetlanie tabeli
```SQL
SELECT * FROM user; {ale tego nie robimy z *}
SELECT username, first_name, last_name, age FROM user; {lepiej podawać nazwy kolumn} 
```
## Dodawanie rekordów do bazy danych
`INSERT INTO nazwa_tabeli(nawzy, kolumn, gdzie, będą, dodawane, dane) VALUES(wartości, danych, w, podanej, kolejności);`

```SQL
INSERT INTO USER (username, first_name, last_name, age) VALUES('Cowboy', 'Tomasz', 'Litwicki', 38);
```

## Wyświetlanie selektywne rekordów z bazy
\* w komendzie SELECT nie używamy gwiazdki!
\_ podkreślenie zastępuje pojedynczy znak
\% podkreślenie zastępuje dowolną ilość znaków

### WHERE
selekcjonowanie rekordów
```SQL
SELECT id, nazwy_kolumn FROM tabela WHERE id > 1;
SELECT id, nazwy_kolumn FROM tabela WHERE id = 1;

...WHERE username = 'Cowboy';
...WHERE id > 1 AND id < 3; {przedział otwarty}
...WHERE id BETWEEN 1 AND 3; {przedział zamnknięty}
...WHERE id = 1 OR age = 43;
...WHERE age IS NULL
...WHERE last_name LIKE 'Litwick_';
...WHERE last_name LIKE 'Litw%';

```

### LIMIT 
wyświetla ilość rekordów
```SQL
SELECT ... FROM user LIMIT 3; {3 rekordy}
SELECT ... FROM user LIMIT 4, 2; {od 5 rekordu, 2 rekordy}

```

### ORDER BY
sortowanie rekordów po nazwie kolumny
ASC - ascending - rosnąco
DESC - descending - malejąco
```SQL
SELECT ... FROM user ORDER BY age;
SELECT ... FROM user ORDER BY age DESC;
SELECT ... FROM user ORDER BY last_name, first_name; {najpierw nazwisko, potem imię}
```

### DISTINCT
Różny, odmienny - aby wyświetlane wartości się nie powtarzały
```SQL
SELECT DISTINCT last_name FROM user {nazwiska się już nie powtarzają}
```
### AGREGACJE: MIN, MAX, SUM, AVG, COUNT
Działają na wielu wierszach naraz.
```SQL
SELECT sum(age) FROM user 
SELECT AVG(age) FROM user {średnia wiaku}

SELECT * FROM user WHERE age >= 40 {wyświetlanie selektywne}
SELECT COUNT(*) FROM user WHERE age >= 40 {zlicza ilość rekordów spełniających warunek}
```

## Pojedyncze funkcje
Działają na pojedynczych rekordach, zmieniają pojedynczą komórkę
```SQL
LENGTH() → ile danych zajmuje tekst w pamięci
CHAR_LENGTH() → ile rzeczywiście jest znaków

```
## Aktualizowanie i usuwanie rekordów
UWAGA!!!
`UPDATE user SET username = 'Mama'` - TO ZAKTUALIZUJE WSZYSTKICH UŻYTKOWNIKÓW!
`UPDATE user SET username = 'Mama' WHERE id = 8;` - TRZEBA WSKAZAĆ KTÓREGO!!!
Ale jest tryb zabezpieczenia i albo podaje się po Primary Key - czyli ID albo wprowadza obejście LIMIT'em.

```SQL
UPDATE user SET username = 'Mały', age = 4 WHERE id = 4;
DELETE FROM user WHERE username = 'Maruda' LIMIT 1; {zabezpieczenie LIMIT 1}
```