==**polecenie -opcje argumenty**==

KATALOG ROBOCZY -  to ten, w którym obecnie się znajdujemy w terminalu!!

---
## 📂 Tabela: Nawigacja w systemie plików

| Polecenie / Pojęcie     | Opis działania                                                     | Przykład użycia         |
| ----------------------- | ------------------------------------------------------------------ | ----------------------- |
| `pwd`                   | Pokazuje pełną ścieżkę do bieżącego katalogu roboczego             | `pwd`                   |
| `ls`                    | Wyświetla zawartość katalogu                                       | `ls`                    |
| `cd`                    | Zmienia katalog roboczy                                            | `cd /home/tomasz`       |
| `cd ..`                 | Przechodzi do katalogu nadrzędnego                                 | `cd ..`                 |
| `cd ~`                  | Przechodzi do katalogu domowego użytkownika                        | `cd ~`                  |
| `cd -`                  | Przechodzi do poprzedniego katalogu                                | `cd -`                  |
| `.`                     | Oznacza katalog bieżący                                            | `ls .`                  |
| `..`                    | Oznacza katalog nadrzędny                                          | `ls ..`                 |
| **Ścieżka bezwzględna** | Zaczyna się od `/`, prowadzi od katalogu głównego                  | `cd /etc/network`       |
| **Ścieżka względna**    | Odnosi się do bieżącego katalogu, nie zaczyna się od `/`           | `cd dokumenty/projekty` |
| `ls ~ / user`           | Wyświetla zawartość dwóch katalogów                                |                         |
| `ls -R`                 | Rekurencyjnie pokazuje zawartość katalogów podrzędnych             | `ls -R`                 |
| `tree`                  | Graficzne przedstawienie struktury katalogów (jeśli zainstalowane) | `tree`                  |
| `file`                  | Określa typ pliku                                                  | `file raport.txt`       |
| `stat`                  | Szczegółowe informacje o pliku lub katalogu                        | `stat zdjęcie.jpg`      |
| `less`                  | Wyświetla zawrtość pliku                                           | `less test.txt`         |
📌 **Wyjaśnienie ścieżek:**

- **Bezwzględna ścieżka**: zawsze zaczyna się od `/` i prowadzi od katalogu głównego. Niezależna od bieżącego katalogu.
- **Względna ścieżka**: zależy od bieżącego katalogu (`pwd`). Używa `.` i `..` do nawigacji lokalnej.
---
### 🧮 Tabela opcji polecenia `ls`

| Opcja                       | Opis działania                                                              | Przykład użycia                |
| --------------------------- | --------------------------------------------------------------------------- | ------------------------------ |
| `-l`                        | Wyświetla szczegółowe informacje o plikach (długi format)                   | `ls -l`                        |
| `-a`                        | Pokazuje wszystkie pliki, w tym ukryte (zaczynające się od kropki)          | `ls -a`                        |
| `-h`                        | Pokazuje rozmiary plików w formacie czytelnym dla człowieka (np. KB, MB)    | `ls -lh`                       |
| `-R`                        | Rekurencyjnie pokazuje zawartość katalogów podrzędnych                      | `ls -R`                        |
| `-t`                        | Sortuje pliki według daty modyfikacji (od najnowszych)                      | `ls -lt`                       |
| `-S`                        | Sortuje pliki według rozmiaru (od największych)                             | `ls -lS`                       |
| `-r`                        | Odwraca kolejność sortowania                                                | `ls -lr`                       |
| `-d`                        | Pokazuje tylko katalogi, bez ich zawartości                                 | `ls -d */`                     |
| `--color=auto`              | Koloruje wynik w zależności od typu pliku (np. katalogi, pliki wykonywalne) | `ls --color=auto`              |
| `-i`                        | Pokazuje numer i-węzła każdego pliku                                        | `ls -i`                        |
| `-1`                        | Wyświetla każdy plik w osobnej linii                                        | `ls -1`                        |
| `-X`                        | Sortuje pliki według rozszerzenia                                           | `ls -lX`                       |
| `--group-directories-first` | Grupuje katalogi przed plikami w wynikach                                   | `ls --group-directories-first` |
| `--hide=PATTERN`            | Ukrywa pliki pasujące do wzorca                                             | `ls --hide=*.bak`              |
| `--time-style=FORMAT`       | Pozwala określić format daty (np. `long-iso`, `full-iso`)                   | `ls -l --time-style=long-iso`  |
### 📁 Typy plików w `ls -l` (pierwszy znak)

| Pierwszy znak | Typ pliku              | Opis                                                     |
| ------------- | ---------------------- | -------------------------------------------------------- |
| `-`           | Plik zwykły            | Standardowy plik (np. tekstowy, binarny, skrypt)         |
| `d`           | Katalog                | Folder zawierający inne pliki lub katalogi               |
| `l`           | Dowiązanie symboliczne | Skrót do innego pliku lub katalogu                       |
| `c`           | Urządzenie znakowe     | Specjalny plik urządzenia (np. terminal, port szeregowy) |
| `b`           | Urządzenie blokowe     | Specjalny plik urządzenia (np. dysk twardy, pendrive)    |
| `s`           | Gniazdo (socket)       | Plik komunikacyjny między procesami                      |
| `p`           | Potok nazwany (FIFO)   | Kolejka komunikacyjna między procesami                   |
Każdy z tych typów może być rozpoznany właśnie po pierwszym znaku w kolumnie uprawnień, np. `drwxr-xr-x` oznacza katalog, a `-rw-r--r--` zwykły plik.

---

## 📁 Tabela: Podstawowe polecenia manipulacji plikami i katalogami

|Polecenie|Krótki opis działania|
|---|---|
|`touch`|Tworzy pusty plik|
|`mkdir`|Tworzy nowy katalog|
|`cp`|Kopiuje plik lub katalog|
|`mv`|Przenosi lub zmienia nazwę|
|`rm`|Usuwa plik lub katalog|
|`rmdir`|Usuwa pusty katalog|
|`file`|Pokazuje typ pliku|
|`stat`|Szczegóły o pliku lub katalogu|
|`ln`|Tworzy dowiązanie twarde|
|`ln -s`|Tworzy do

---

### Tabela 4.1. Wieloznaczniki

| `Wieloznacznik` | Znaczenie                                         |
| --------------- | ------------------------------------------------- |
| `*`             | Pasuje do dowolnych znaków                        |
| `?`             | Pasuje do dowolnego pojedynczego znaku            |
| `[znaki]`       | Pasuje do dowolnego znaku z zestawu \[znaki\]     |
| `[!znaki]`      | Pasuje do dowolnego znaku spoza zestawu \[znaki\] |
| `[[:klasa:]]`   | Pasuje do dowolnego znaku z klasy                 |
|                 |                                                   |
### Tabela 4.2. Często używane klasy znaków

|`Klasa znaków`|Znaczenie|Przykład dopasowania|
|---|---|---|
|`[:alnum:]`|Dowolny znak alfanumeryczny|a1.txt, Z9.doc|
|`[:alpha:]`|Dowolny znak alfabetu|a.txt, Z.doc|
|`[:digit:]`|Dowolna cyfra|1.txt, 9.log|
|`[:lower:]`|Mała litera|a.txt, z.doc|
|`[:upper:]`|Wielka litera|A.txt, Z.doc|

### Tabela 4.3. Przykłady wieloznaczników

| `Wzorzec`                 | Pasuje do                                                             | Przykład pliku              |
| ------------------------- | --------------------------------------------------------------------- | --------------------------- |
| `*`                       | Wszystkich plików                                                     | test.txt, backup.zip        |
| `g*`                      | Plików zaczynających się od „g”                                       | grafika.jpg, game.exe       |
| `b*.txt`                  | Plików zaczynających się od „b” i kończących na .txt                  | backup.txt, baza.txt        |
| `Data??.*`                | Plików zaczynających się od „Data” i mających dokładnie 3 znaki dalej | Data01.txt, DataAB.doc      |
| `[abc]*`                  | Plików zaczynających się od a, b lub c                                | a.txt, baza.doc, config.ini |
| `BACKUP.[0-9][0-9][0-9]`  | Plików kończących się trzema cyframi po „BACKUP.”                     | BACKUP.001, BACKUP.123      |
| `[![:upper:]]*`           | Plików, których nazwa nie zaczyna się wielką literą                   | dokument.txt, backup.zip    |
| `[[:digit:]][[:digit:]]*` | Plików zaczynających się od dwóch cyfr                                | 12data.txt, 99backup.log    |
| `*[[:lower:]123]`         | Plików kończących się małą literą lub cyfrą 1, 2, 3                   | plik1, backup2, testa       |

---
### 🗂️ Tabela opcji poleceń `cp`, `mv` i `rm`

| **Polecenie** | **Opcja**           | **Znaczenie**                                                                                     |
| ------------- | ------------------- | ------------------------------------------------------------------------------------------------- |
| `cp`          | `-a, --archive`     | Kopiuje pliki i katalogi wraz z wszystkimi atrybutami, łącznie z przynależnością i uprawnieniami. |
| `cp`          | `-i, --interactive` | Przed nadpisaniem istniejącego pliku użytkownik jest proszony o potwierdzenie.                    |
| `cp`          | `-r, --recursive`   | Rekurencyjnie kopiuje katalogi i podkatalogi.                                                     |
| `cp`          | `-u, --update`      | Kopiuje tylko pliki nowsze niż wersje źródłowe lub nieistniejące w katalogu docelowym.            |
| `cp`          | `-v, --verbose`     | Wyświetla komunikaty informacyjne podczas kopiowania.                                             |
| `mv`          | `-i, --interactive` | Przed nadpisaniem pliku użytkownik jest proszony o potwierdzenie.                                 |
| `mv`          | `-u, --update`      | Przenosi tylko pliki nowsze lub nieistniejące w katalogu docelowym.                               |
| `mv`          | `-v, --verbose`     | Wyświetla komunikaty informacyjne podczas przenoszenia.                                           |
| `rm`          | `-i, --interactive` | Przed usunięciem pliku użytkownik jest proszony o potwierdzenie.                                  |
| `rm`          | `-r, --recursive`   | Rekurencyjnie usuwa katalogi i ich zawartość.                                                     |
| `rm`          | `-f, --force`       | Ignoruje błędy i nie pyta o potwierdzenie; nadpisuje `--interactive`.                             |
| `rm`          | `-v, --verbose`     | Wyświetla komunikaty informacyjne podczas usuwania.                                               |
 UWAGA NA RM!
 W systemach uniksowych takich jak Linux nie istnieje polecenie pozwalające  cofnąć usuwanie. Gdy usuniemy element za pomocą polecenia rm, już go nie  odzyskamy. Linux zakłada, że jesteśmy inteligentni i wiemy, co robimy. Należy szczególnie uważać na wieloznaczniki. Potraktujmy to jako klasyczny przykład. Załóżmy, że chcemy usunąć z jakiegoś katalogu tylko pliki HTML. W tym celu piszemy:
 `rm *.html
 co jest poprawne, lecz jeśli przypadkiem wpiszemy znak spacji między znakiem \* i .html jak poniżej:
 `rm * .html
polecenie rm usunie wszystkie pliki w katalogu, a następnie wyświetli komunikat, że nie istnieje żaden plik o nazwie .html. Oto przydatna wskazówka: niezależnie od tego, czy w poleceniu rm korzystamy z wieloznaczników (oprócz dokładnego sprawdzenia wpisanego tekstu!), czy nie, powinniśmy najpierw przetestować wzorzec z wieloznacznikami poleceniem ls. W ten sposób uzyskamy informację o plikach, które zostaną usunięte. Następnie naciskamy klawisz strzałki w górę, aby przywołać wcześniejsze polecenie, i zastępujemy ls poleceniem rm.

---
