# 1 Tworzenie
CREATE DATABASE nazwa_bazy;
USE nazwa_bazy;
CREATE TABLE nazwa_tabeli
	(nzawa_kolumny1 TYP, nazwa_kolumny2 TYP NOT NULL DEFAULT wartość);
		CHAR
		VARCHAR
		BLOB
		INT
		DATE
		DATETIME
DESC nazwa_tabeli; - opis tabeli
DROP TABLE nazwa_tabeli; - kasuje całą tabelę

INSERT INTO nazwa_tabeli (kolumna1, kolumna2) VALUES ('wartość1', 'wartość2');

# 2 SELECT
SELECT * FROM nazwa_tabeli
SELECT nazwy_kolumn FROM nazwa_tabeli 
	WHERE nazwa_kolumny 
		{operator porównania} porównywana_wartość
			= > < <= >=  <>
			IS NULL
			IS NOT NULL
			BETWEEN wartosc1 AND wartosc2
			NOT BETWEEN ...
			IN ('zbiór', 'szukanych', 'wartości')
			NOT IN ('zbiór', 'wartości', 'nie', 'szukanych')
		LIKE 'tekst ze wiloznacznikami typu:'
			% - zastępuje dowolny ciąg znaków
			_ - zastępuje tylko jeden znak
		NOT LIKE ...
	~~WHERE NOT nazwa_kolumny ...~~ `NOT` najlepiej umieszczaj **bezpośrednio przed operatorem logicznym**, a nie przed nazwą kolumny.

operatory warunkowe:
AND
OR
IS
# 3 DELETE i UPDATE
DELETE FROM nazwa_tabeli WHERE nazwy_kolumn {operatory};
UPDATE nazwa_tabeli SET kolumna1 = 'nowa_wartość', kolumna2 = 'inna_wartość';

# 4 TABELE ZNORMALIZOWANE - edycja tabel i zmiany

EDYCJA TABEL:
Edycja istniejących tabel:
SHOW DATABASES; 
SHOW TABLES;

SHOW CREATE DATABASE nazwa_bazy_danych; - pokazuje kod stworzenia bazy danych.
SHOW CREATE TABLE nazwa_tabeli; - pokazuje kod stworzenia tabeli.
SHOW COLUMNS FROM nazwa_tabeli; pokazuje tabelę kolumn jak (skrót: DESC lub DESCRIBE)
SHOW INDEX FROM nazwa_tabeli; - pokazuje indeksy tabeli jak PRIMARY KEY.
SHOW WARNINGS; - pokazuje ostrzenienia.

# 5 ALTER
DODAWANIE / ZMIANA / MODYFIKACJA / USUWANIE / (KOLEJNOŚĆ - tylko AFTER!)
ALTER TABLE nazwa_tabeli 
- RENAME TO nawa_nazwa_tabeli;
- ADD COLUMN id INT NOT NULL AUTO_INCREMENT FIRST, ADD PRIMARY KEY (id);
- CHANGE COLUMN nazwa_kolumny nowa_nazwa TYPZMIENNEJ;
- MODIFY COLUMN nazwa_kolumny NOWYTYPZMIENNEJ AFTER za_czym_ma_stać;
- DROP COLUMN nazwa_kolumny;

FUNKCJE ŁAŃCUCHOWE CHAR i VARCHAR:
- SUBSTRING_INDEX (nazwa_kolumny, 'znak_rozdzielający', index_znaku) 2-pobierze wszystko sprzed 2 przecinka
- RIGHT(kolumna, ilosc_znakow_z_prawej_strony)
- SUBSTRING(lancuch, od_ktorego_znaku, ile_znakow)
- UPPER
- LOWER
- REVERSE
- LTIRM - usuwa białe znaki z lewej
- RTRIM - usuwa biełe znaki z prawej
- LENGTH

# 6 CASE
```SQL
UPDATE nazwa_tabeli
SET nowa_kolumna =
CASE
	WHEM kolumna1 = wartość1
		THEN nowa_wartość1
	WHEN kolumna2 = wartość2
		THEN nowa_wartość2
	ELSE nowa_wartość3
END;
```

# 7 TABELE ŁĄCZONE
1. jeden-do-jeden
2. jeden-do-wielu
3. wiele-do-wielu -> tabela łącząca

- ZALEŻNOŚĆ FUNKCJONALNA kolumn w tabeli RELACYJNEJ:
	`nazwa_tabeli.kolumna_wpływowa ->; nazwa_tabeli.kolumna_zależna


```sql
CREATE TABLE zainteresowania (
id_zainteresowania INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
zainteresowanie VARCHAR(50) NOT NULL,
id_kontaktu INT NOT NULL,
CONSTRAINT kontakty_id_kontaktu_fk
FOREIGN KEY (id_kontaktu)
REFERENCES kontakty (id)
);
```

# 8 ZŁĄCZENIA

- CROSS JOIN - złączenie kartezjańskie (każde z każdym)
- INNER JOIN ON warunek_porównania - złączenie uwarunkowane operatorem porównania
	- = złączenie równościowe
	- <> złączenie różnościowe
NATURAL JOIN - złączenie naturalne (rozpoznaje pasujące do siebie nazwy kolumn)

 