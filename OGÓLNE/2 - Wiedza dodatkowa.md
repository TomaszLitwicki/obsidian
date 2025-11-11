## Sprawdzanie podanej zmiennej

```
age = input("How old are you? ")

try:
    age = int(age)
    print("You entered a valid age:", age)
except ValueError:
    print("That's not a valid number!")
    exit()
```

```
isinstance(age, int)  # Zwraca True lub False
```

## Zmienne

- `<class 'str'>` — ciąg znaków
- `<class 'int'>` — liczba całkowita
- `<class 'float'>` — liczba zmiennoprzecinkowa
- `<class 'bool'>` — wartość logiczna (True / False)
- `<class 'list'>` — lista
- `<class 'dict'>` — słownik 
## Kończenie stringa

 Python automatycznie przechodzi do nowej lini '\n'. Aby zostać w tej samej można na końcu printa dodać `print("jakiś tekst", end=" ")`

## REGEX Oznaczenia znaków w kodzie

\n - enter
\s - znak biały (spacja, TAB, ENTER)
\d - cyfra
\w - litera, cyfra
r - row string, surowy łańcuch znaków (nie interpretuje \\) 
. - dowolny znak

\ - znak ucieczki, wyświetla następny znak, nie wykonuje przypisanej do niego funkcji.
| - pipe

Rozbijmy `r'[.!?]+'`:

- `r'...'` → to **raw string** w Pythonie. Dzięki temu backslashe (`\n`, `\t`, `\w`) nie są interpretowane przez Pythona, tylko trafiają prosto do regexa.
- `[.!?]` → **klasa znaków**. Oznacza „dopasuj jeden dowolny znak spośród: kropka (`.`), wykrzyknik (`!`) albo pytajnik (`?`)”.
- `+` → **operator ilościowy**. Mówi: „dopasuj **co najmniej jeden** taki znak, a jeśli występują kolejne, też je zbierz w jednym dopasowaniu”.

Czyli:
- bez plusa: `[.!?]` → znajdzie **pojedynczy** znak kończący zdanie.
- z plusem: `[.!?]+` → znajdzie **ciąg takich znaków**, np. `...`, `!!`, `?!` — i potraktuje je jako jeden podział.

## regular expressions - wyrażenia regularne
`re.sub(...)` — zamienia wzorzec na coś innego (tu: na pusty ciąg `''`)
`re.sub(pattern, replacement, string, count=0)

|Parametr|Opis|
|---|---|
|`pattern`|Wzorzec (wyrażenie regularne), który ma zostać znaleziony|
|`replacement`|Tekst, który ma zastąpić znaleziony wzorzec|
|`string`|Tekst wejściowy, w którym dokonujemy zamiany|
|`count` _(opcjonalnie)_|Ile razy ma zostać wykonana zamiana (domyślnie 0 = wszystkie)|
`r'[^\w\s]'` — oznacza: wszystko, co **nie jest** (`^`) literą/cyfrą (`\w`) ani spacją (`\s`)

## Formatowanie kodu

Menu -> Code -> Reformat Code
Ctrl + Alt + L

## Breake czy Return?

break → zatrzymuje tylko pętlę i wraca do dalszego kodu w funkcji.
return → zatrzymuje całą funkcję i wraca (z wynikiem lub None).
pass → nic nie robi (wypełniacz instrukcji, aby nie generować błędu składni)

## kończenie kodu

```koniec_programu
import sys
sys.exit()
```

## moduły sys i os

### sys

Nazwa od system (ale w sensie: Python interpreter system).
Dotyczy tego, jak działa sam interpreter Pythona.
Przykłady:
sys.exit() – kończy program w kontrolowany sposób
sys.argv – dostęp do argumentów z wiersza poleceń
sys.path – lista katalogów, gdzie Python szuka modułów
sys.version – wersja interpretera
Czyli sys = narzędzia do sterowania Pythonem samym w sobie.

### os

Nazwa od operating system.
Służy do komunikacji z systemem operacyjnym (Windows, Linux, macOS).
Przykłady:
os.getcwd() – aktualny katalog roboczy
os.listdir() – lista plików w katalogu
os.remove("plik.txt") – usuwa plik
os.mkdir("nowy_folder") – tworzy katalog
os.environ – zmienne środowiskowe
Czyli os = narzędzia do rozmowy z komputerem/plikami/katalogami.
### w skrócie:

sys → rzeczy „wewnętrzne” Pythona (jak się uruchamia, gdzie szuka bibliotek, jak zakończyć).
os → rzeczy „zewnętrzne” (foldery, pliki, procesy, zmienne systemowe).

## Sprawdzenie typu zmiennej
### **`isinstance(x, TYP)`**  
Funkcja wbudowana w Pythona, która zwraca:
- `True`, jeśli obiekt `x` jest podanego typu,
- `False`, jeśli nie jest.
TYP - może być krotką (int, float)
### liczbowe typy zmiennej
| Metoda             | Co sprawdza?                                                     | Przykłady `True`                                  | Przykłady `False`                       | Typowe użycie                                                                                |
| ------------------ | ---------------------------------------------------------------- | ------------------------------------------------- | --------------------------------------- | -------------------------------------------------------------------------------------------- |
| **`.isdigit()`**   | Czy wszystkie znaki są cyframi (w sensie ogólnym Unicode)?       | `"123"`, `"²"` (do potęgi), `"四"` (chińska cyfra) | `"-5"`, `"3.14"`, `"abc"`, `""`         | Najczęściej używana do walidacji, czy string to **jakakolwiek liczba całkowita bez znaku**   |
| **`.isdecimal()`** | Czy wszystkie znaki są cyframi dziesiętnymi (0–9)?               | `"123"`                                           | `"²"`, `"四"`, `"-5"`, `"3.14"`, `"abc"` | Stricte sprawdzanie klasycznych cyfr arabskich, np. w numerach telefonów czy kodach PIN      |
| **`.isnumeric()`** | Jeszcze szersze – czy wszystkie znaki mają znaczenie numeryczne? | `"123"`, `"²"`, `"四"`, `"Ⅻ"` (rzymskie dwanaście) | `"-5"`, `"3.14"`, `"abc"`, `""`         | Bardziej uniwersalne walidacje, np. w aplikacjach obsługujących różne kultury i zapisy liczb |


## fajne znaczki specjalne @

| znaczek    | Unicode | Funkcja (ASCII) | html    |
| ---------- | ------- | --------------- | ------- |
| (kwadrat)² | \u00B2  | chr(178)        | \&sup2; |
|            |         |                 |         |

# Pydantic
pydantic to biblioteka do walidacji danych (data validation) i tworzenia modeli danych (data models) w Pythonie, która bazuje na typowaniu (type hints).
Innymi słowy:

Dzięki pydantic możesz tworzyć klasy, które sprawdzają, czy dane mają odpowiedni typ, strukturę i wartości — zanim ich użyjesz.

# Sortowanie
czym się różni .sort() od sorted() ?

.sort() zmienia listę, sorted() tworzy nową.
Obie robią „to samo”, ale różnią się sposobem działania i zastosowaniem.

## .sort() — to metoda listy
działa tylko na listach,
modyfikuje listę w miejscu (czyli zmienia oryginał),
nie zwraca nowej listy – zwraca None.

```Przyklad
horses = [5, 3, 8]
horses.sort()
print(horses)  # [3, 5, 8]
```
Tutaj horses zostało zmienione.

## sorted() — to funkcja wbudowana
działa na dowolnych **iterowalnych** obiektach (np. listach, krotkach, słownikach, zbiorach),
zwraca nową posortowaną listę, nie ruszając oryginału.

```Przyklad
horses = [5, 3, 8]
new_list = sorted(horses)
print(new_list)  # [3, 5, 8]
print(horses)    # [5, 3, 8] — bez zmian
```

Obie mają te same argumenty:
key= (definiuje, po czym sortować) i reverse= (czy odwrócić kolejność).
