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
UPDATE nazwa_tabeli SET kolumna1 = 'nowa_wartość', kolumna2 = 'inna_wartość' WHERE {warunki identyfikujące rekord};

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

\G na końcu zmienia postać wyświetlania:
	mysql> SHOW CREATE TABLE moje_kontakty\G;
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

# 9 PODZAPYTANIA
Złączenia są bardziej wydajne aniżeli podzapytania

A. ZAPYTANIE ZEWNĘTRZNE
B. ZAPYTANIE WEWNĘTRZNE
1. Podzapytania nieskorelowane
2. Podzapytania skorelowane

* IN
* NOT IN
* EXIST
* NOT EXIST

# 10 ZŁĄCZENIA ZEWNĘTRZNE

# 11 OGRANICZENIA, WIDOKI, TRANSAKCJE
## CHECK 
**to ograniczenie sprawdzające (constraint), czyli formalna kontrola poprawności danych na poziomie tabeli.** MySQL pozwala wstawić wiersz tylko wtedy, gdy warunek w CHECK jest prawdziwy.
Za każdym INSERT-em i UPDATE-em MySQL sprawdza wyrażenie.

Tworzenie tabeli:
```SQL
CREATE TABLE pracownicy (
    id INT AUTO_INCREMENT PRIMARY KEY,
    wiek INT,
    plec CHAR(1),
    CHECK (wiek BETWEEN 18 AND 70),
    CHECK (plec IN ('K','M'))
);
```
Edytowanie tabeli:
```SQL
ALTER TABLE pracownicy
ADD CONSTRAINT chk_wiek CHECK (wiek BETWEEN 18 AND 70);
```
CONSTRAINT - nadaje nazwę ograniczeniu!

Można też używać bardziej złożonych warunków:
`CHECK (pensja_min <= pensja_max)`

To jest bardzo użyteczne, bo pilnuje logiki wewnątrz jednego wiersza.

Kilka ważnych faktów, które są często mylone.
1.  CHECK **nie zastępuje kluczy obcych.**  
	CHECK pilnuje wartości _wewnątrz tabeli_.  
	FOREIGN KEY pilnuje relacji _między tabelami_.

2. CHECK **nie widzi innych wierszy.**  
	Nie zrobisz nim czegoś w stylu „pensja nie może być większa niż średnia w tabeli”. To nie ten poziom narzędzia.

3. możesz dodać CHECK później albo usunąć:
	`ALTER TABLE pracownicy ADD CONSTRAINT chk_wiek CHECK (wiek BETWEEN 18 AND 70);`
	`ALTER TABLE pracownicy DROP CHECK chk_wiek;`

Krótka definicja robocza:  
CHECK = lokalny test logiczny, który musi być prawdziwy dla każdego wstawianego lub aktualizowanego wiersza.

- PRIMARY KEY pilnuje unikalności,  
- FOREIGN KEY pilnuje relacji,  
- CHECK pilnuje sensu danych.

## WIDOK
Tworzenie widoku, zachowuje się jak tabela:
```SQL
CREATE VIEW projektanci_stron AS
SELECT mk.id_kontaktu, mk.imie, mk.nazwisko, mk.telefon, mk.email, pp.stanowisko FROM moje_kontakty AS mk
JOIN praca_poszukiwana AS pp ON mk.id_kontaktu = pp.id_kontaktu
WHERE pp.stanowisko LIKE '%WWW%';
```

Wywołanie:
`SELECT * FROM projektanci_stron;`

Tworzenie widoku z klauzulą CHECK OPTION:
`CREATE VIEW sw_piatki AS SELECT * FROM skarbonka WHERE moneta='P' WITH CHECK OPTION;`
Klauzula ta informuje system zarządzania relacyjnymi bazami danych, że należy sprawdzić, czy wykonanie polecenia INSERT i UPDATE są zgodne z warunkami WHERE podanymi podczas tworzenia widoku! 

Usuwanie:
`DROP VIEW sw_piatki;`

Lista widoków:
SHOW TABLES;

## TRANSAKCJE
test ACID:
- ATOMICITY (niepodzielność)
- CONSISTENCY (spójność)
- ISOLATION (izolacja)
- DURABILITY (odporność)

```SQL
START TRANSACTION;
COMMIT;
ROLLBACK;
```

# 12 BEZPIECZEŃSTWO
## Tworzenie użytkownika:
`CREATE USER 'jan' IDENTIFIED BY 'SilneHaslo123!';`

Sposoby dostępu do bazy:
- `'jan'@'localhost'`
- `'jan'@'%'` <- domyślny jak na górze
- `'jan'@'192.168.1.10'`

## Nadawanie uprawnień:
`GRANT SELECT ON nazwa_tabeli TO nazwa_używtkownika;`
	SELECT, INSERT, DELETE, UPDATE, ALL 
`GRANT SELECT ON nazwa_tabeli TO nazwa_używtkownika WITH GRANT OPTION;`
	WITH GRANT OPTION - możliwość przydzielania tego uprawnienia innym użytkownikom...

## Usuwanie uprawnień:
`REVOKE SELECT ON nazwa_tabeli FROM nazwa_użytkownika;`

Usuwanie uprawnień do nadawania uprawnień:
`REVOKE GRANT OPTION FOR DELETE ON nazwa_tabeli FROM użytkownik;`
		pozostałe osoby zachowują swoje uprawnienia jeśli użytkownik im nadał... ale jeśli:
	`[...] FROM użytkownik CASCADE;` <- odbiera uprawnienia wszystkim, którym nadał uprawnienia użytkownik.
	`[...] FROM użytkownik RESTRICT;` <- BŁĄD jeśli użytkownik nadał już komuś uprawnienia. Nikt nie traci uprawnień.

## Role uprawnień:
`CREATE ROLE nowa_rola;`
	nadanie uprawnień roli:
	`GRANT SELECT, INSERT, UPDATE ON baza_danych.* TO nowa_rola;`
		przypisanie użytkownika:
		`GRANT nowa_rola TO 'tomek'@'localhost';`
		najlepiej zaznaczyć DEFAULT:
			`SET DEFAULT ROLE nowa_rola TO 'tomek'@'localhost';`

Jak sprawdzić, co ma rola:
`SHOW GRANTS FOR nowa_rola;`

Jak sprawdzić, jakie role ma użytkownik:
`SHOW GRANTS FOR 'tomek'@'localhost';`

# DODATKI
Jawna transakcja:

`START TRANSACTION;`
Od tego momentu jesteś w „wyizolowanej bańce”. Teraz możesz robić normalne zmiany:

`UPDATE klienci SET status = 'AKTYWNY' WHERE id = 10;  DELETE FROM zamowienia WHERE id = 555;`

Nic jeszcze nie jest zapisane na stałe.

Teraz masz dwa wyjścia:

`COMMIT;`

To mówi: zapisz wszystko, co zrobiłem od START TRANSACTION.

Albo:

`ROLLBACK;`
To mówi: cofnięcie wszystkiego, jakbyś nic nie zrobił.