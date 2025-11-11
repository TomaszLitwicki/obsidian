# Wstęp
\' - single quote
\" - double qote

Nie ma różnicy w zastosowaniu, oba tworzą STRING (*łańcuch znaków, tekst*) ale jest dobra praktyka KONWENCJA jak poniżej:

\' - *używamy częściej do INDENTYFIKATORÓW*
\" - używamy częściej do dłuższych tekstów...

""" Tworzą dłuższy tekst razem z TABami i ENTERami """

Zmienne są tworzone z podkreśleniem:
first_name = 'Tomasz'

escape charakter \ (backslash) - neguje działanie następnego znaku. I tutaj i w Python'ie.

# INTERPOLACJA STRINGA f 
	dodaje zmienne do ciągu tekstu STRING
		`channel_name = 'Jak nauczyć się programowania'`
		`text = f"Bardzo lubię kanał {channel_name}"`
		`print(text)

# Metody
Metody po kropce . *(Funkcja wywoływana na obiekcie to METODA)*

| .upper()                        | zmienia wszystkie litery na duże                  |
| ------------------------------- | ------------------------------------------------- |
| .capitalize()                   | zmienia pierwszą literę na dużą                   |
| .title()                        | zmienia każdą pierwszą lierę na dużą              |
| .strip()                        | kasuje białe znaki z początku i końca             |
| .rstrip('znak') .lstrip('znak') | kasuje znaki z prawej lub lewej strony            |
| .lower()                        | wszystkie znaki małe                              |
| .startswith("L")                | czy STRING zaczyna się od litery "L"              |
| .endswith("L")                  | czy STRING kończy się na literę "L"               |
| replace(old, new)               | zmiana znaku, ciągu znaków, wyrazów               |
| split("znak")                   | dzielenie STRINGA na osobne wyrazy względem ZNAKU |
| .join()                         |                                                   |
# SLICING
[[C-14 - SLICING]]
	`print(moj_string[1:3])` - *wyświetla znaki od 2 do 3 (indeksowanie od 0, ostatniego 4 nie wyświetla*
	`print(moj_string[0:-3])` - *wyświetla znaki od 1 do 3 od końca 
	`len(moj_string)` - zwraca ilość znaków*

# Typ zmiennej
`print(type(a))` - Sprawdzanie typu zmiennej 
`a = input("Napisz coś!")` - zawsze jest zmienną typu STRING
`int(a)` - zmienia typ zmiennej na liczbę całkowitą

# Formatowanie liczby 
Formatowanie f"{a / b:.2f}"

f"..." → f-string (czyli można w środku {} wstawiać Pythonowy kod).
{a / b} → to, co chcemy wstawić (wynik dzielenia).
: → mówi: „teraz podaję szczegóły formatowania”.
.2f → szczegół formatowania:
.2 → dwa miejsca po przecinku,
f → format fixed-point (liczba zmiennoprzecinkowa).

| Format  | Przykład kodu        | Wynik          | Opis                                                 |
| ------- | -------------------- | -------------- | ---------------------------------------------------- |
| `.2f`   | `f"{123.456:.2f}"`   | `123.46`       | Liczba zmiennoprzecinkowa z 2 miejscami po przecinku |
| `.0f`   | `f"{123.456:.0f}"`   | `123`          | Zaokrąglanie do liczby całkowitej                    |
| `10.2f` | `f"{123.456:10.2f}"` | `"    123.46"` | Szerokość pola = 10 znaków, wyrównanie do prawej     |
| `,`     | `f"{1234567:,}"`     | `1,234,567`    | Separator tysięcy (w stylu anglosaskim)              |
| `_`     | `f"{1234567:_}"`     | `1_234_567`    | Separator tysięcy w stylu Pythona (podkreślenie)     |
| `%`     | `f"{0.256:.2%}"`     | `25.60%`       | Format procentowy                                    |
| `e`     | `f"{12345:.2e}"`     | `1.23e+04`     | Notacja naukowa (wykładnicza)                        |
| `g`     | `f"{12345:.3g}"`     | `1.23e+04`     | „Najlepsza” forma (zależnie od wielkości liczby)     |
| `>10`   | `f"{42:>10}"`        | `"        42"` | Wyrównanie do prawej w polu 10 znaków                |
| `<10`   | `f"{42:<10}"`        | `"42        "` | Wyrównanie do lewej w polu 10 znaków                 |
| `^10`   | `f"{42:^10}"`        | `"    42    "` | Wyśrodkowanie w polu 10 znaków                       |

# Łączenie

`.join()` to metoda wbudowana w typ `str` (czyli string). Ona bierze **iterowalne elementy tekstowe** (np. listę napisów) i **łączy je w jeden string**, wstawiając pomiędzy nie ten tekst, na którym wywołujesz `.join()`.

Można myśleć o niej tak:  
„mam garść kawałków tekstu i chcę je skleić klejem `X`”.  
Tym klejem jest właśnie string przed `.join()`.

### Przykłady

**Łączenie listy słów spacją:**

`slowa = ["Ala", "ma", "kota"]
`zdanie = " ".join(slowa)
`print(zdanie)`

Wynik:

`Ala ma kota`

**Łączenie bez żadnego separatora:**

`litery = ["a", "b", "c"] slowo = "".join(litery) print(slowo)`

Wynik:

`abc`

**Łączenie przecinkiem:**

`cyfry = ["1", "2", "3"] lista = ",".join(cyfry) print(lista)`

Wynik:

`1,2,3`

**Łączenie z własnym separatorem:**

`elementy = ["pies", "kot", "koń"] tekst = " | ".join(elementy) print(tekst)`

Wynik:

`pies | kot | koń`

### Ważne uwagi

- `.join()` działa tylko na **napisach** – czyli elementy listy muszą być stringami. Jeśli masz liczby, musisz je najpierw zrzutować na stringi:
    `liczby = [1, 2, 3] print(", ".join(map(str, liczby)))`
    → `1, 2, 3`
- W przeciwieństwie do konkatenacji `+`, `.join()` jest **wydajniejszy**, gdy sklejasz wiele elementów (np. tysiące w pętli).
