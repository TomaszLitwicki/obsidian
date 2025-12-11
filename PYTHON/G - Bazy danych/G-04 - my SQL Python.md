`pip install mysql-connector-python`

# Pobieranie danych
## fetchall
Gdy nie istnieje -> zwraca pustą listę
```Python
import mysql.connector  
from mysql.connector import Error  
  
try:  
    with mysql.connector.connect(  
        host="localhost",  
        user="root",  
        password="pppp",  
        database="python_sql"  
    ) as connection:  
  
        with connection.cursor() as cursor:  
  
            cursor.execute("SELECT id, username, first_name, last_name, age FROM user")  
  
            result = cursor.fetchall()  
  
            for row in result:  
                print(f'ID: {row[0]}')  
                print(f'Nazwa użytkownika: {row[1]}')  
                print(f'Imię: {row[2]}')  
                print(f'Nazwisko: {row[3]}')  
                print(f'Wiek: {row[4]}')  
                print()  
except Error as e:  
    print(e)
```
## fetchmany()
Przeprocesowanie nie wszystkich użytkowników (all), tylko tylu, ilu jest podanych w nawiasie fetchmany(ilość)
`result = cursor.fetchmany(2)`
Gdy nie istnieje -> zwraca pustą listę

Procesowanie partiami:
```Python
while True:  
    result = cursor.fetchmany(2)  
  
    if not result:  
        break  
  
    for row in result:  
        print(f'ID: {row[0]}')  
        print(f'Nazwa użytkownika: {row[1]}')  
        print(f'Imię: {row[2]}')  
        print(f'Nazwisko: {row[3]}')  
        print(f'Wiek: {row[4]}')  
        print()
```

## fetchone()
Przeprocesowanie pojedynczych rekordów w formie krotki
`result = cursor.fetchone()`
Gdy nie istnieje -> zwraca obiekt None
```Python
[...]
cursor.execute("SELECT id, username, first_name, last_name, age FROM user WHERE id=2")  
  
result = cursor.fetchone()  
print (result)
```

# Dodawanie danych
ACID to cztery cechy dobrze zaprojektowanej transakcji w bazie danych. Transakcja to zestaw operacji, który ma zostać wykonany „albo w całości, albo wcale”.
- **A – Atomicity (atomowość)** Transakcja jest niepodzielna.
- **C – Consistency (spójność)**  Po każdej zakończonej transakcji baza danych musi pozostać w stanie zgodnym z zasadami
- **I – Isolation (izolacja)**  Transakcje nie przeszkadzają sobie nawzajem.
- **D – Durability (trwałość)**  Jeżeli transakcja została zatwierdzona (`COMMIT`), dane **muszą przetrwać**:
Dane biznesowe muszą być:
- **prawdziwe**
- **kompletne**
- **niezniszczalne**
- **zgodne z zasadami**
- **odporne na równoczesne operacje**
Bez ACID baza danych stałaby się chaosem przy pierwszym równoległym dostępie lub awarii.

Albo COMMIT albo ROLLBACK
```Python
with connection.cursor() as cursor:  
    insert_query = f"""  
    INSERT INTO user (username, first_name, age)
    VALUES ("Cowboy", "Tomasz", 38)
    """  
    cursor.execute(insert_query)  
    connection.commit()
```

# Aktualizowanie danych
```Python
with connection.cursor() as cursor:  
    update_query = "UPDATE user SET last_name = 'Litwicki' WHERE id = 32"  
    cursor.execute(update_query)  
    connection.commit()
```

# Usuwanie danych
```Python
with connection.cursor() as cursor:  
    delete_query = "DELETE FROM user WHERE id = 32"  
    cursor.execute(delete_query)  
    connection.commit()
```

## Uwagi dodatkowe

`%s` = **bezpieczny placeholder na dane**, obsługiwany przez bibliotekę, a nie Pythona.  
Dzięki temu dane trafiają do zapytania oddzielnie, co:
- chroni przed SQL injection,
- upraszcza kod,
- pilnuje typów.

`cursor.execute("INSERT INTO users (name, age) VALUES (%s, %s)", ("Tomasz", 38))`

