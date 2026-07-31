# SQL – `REGEXP`

`REGEXP` (synonim: `RLIKE`) to operator MySQL/MariaDB zwracający `1` (prawda) lub `0` (fałsz) — sprawdza, czy wzorzec pasuje **gdziekolwiek** w tekście (nie trzeba go otaczać `%...%` jak przy `LIKE`). Domyślnie jest **niewrażliwy na wielkość liter** (zależnie od collation kolumny).

```sql
SELECT * FROM users WHERE username REGEXP '^[0-9]';      -- pasuje
SELECT * FROM users WHERE username NOT REGEXP '^[0-9]';  -- negacja
```

> W przeciwieństwie do Pythona, SQL `REGEXP` służy tylko do **filtrowania** (prawda/fałsz) — nie zwraca dopasowanego fragmentu ani grup. Do ekstrakcji tekstu służy `REGEXP_SUBSTR()` (MySQL 8+), np. `REGEXP_SUBSTR(username, '[0-9]+')`.

---

## Kotwice (początek/koniec tekstu)

`^` – początek tekstu  
`$` – koniec tekstu

Przykład:

```sql
REGEXP '^[0-9]+$'
```

— cały tekst to cyfry.

---

## Klasy znaków

`[abc]` – dowolny z: a, b lub c  
`[^abc]` – dowolny oprócz a, b, c  
`[0-9]` – zakres cyfr  
`[A-Za-z]` – litery bez polskich znaków

Standardowe klasy POSIX:

- `[:digit:]` – cyfry
- `[:alpha:]` – litery
- `[:alnum:]` – litery lub cyfry
- `[:space:]` – białe znaki (spacja, tab)
- `[:punct:]` – znaki interpunkcyjne

Klasy POSIX muszą być zagnieżdżone w dodatkowych `[]`:

```sql
REGEXP '^[[:alpha:]]'
```

---

## Ilości

`+` – jeden lub więcej  
`*` – zero lub więcej  
`?` – zero lub jeden  
`{n}` – dokładnie n  
`{n,}` – n lub więcej  
`{n,m}` – między n a m

Przykłady:

| Wzorzec | Znaczenie |
|---|---|
| `[0-9]+` | jedna lub więcej cyfr |
| `[A-Z]{3}` | dokładnie 3 wielkie litery |
| `[0-9]{2,4}` | 2–4 cyfry |

---

## Kropka (dowolny znak)

`.` – jeden dowolny znak

Przykład:

```sql
'.{3}'   -- dokładnie trzy dowolne znaki
```

---

## Alternatywa

`a|b` – a lub b

Przykład:

```sql
REGEXP '^(kot|pies)$'
```

---

## Przykłady praktyczne

| Cel | Wzorzec |
|---|---|
| Tekst zaczyna się od cyfry | `REGEXP '^[0-9]'` |
| Tekst kończy się literą | `REGEXP '[A-Za-z]$'` |
| Zawiera spację | `REGEXP ' '` |
| Zawiera jakąkolwiek cyfrę | `REGEXP '[0-9]'` |
| Tylko cyfry, nic więcej | `REGEXP '^[0-9]+$'` |

---

# Python — moduł `re`

Wszystkie kotwice, klasy znaków i ilościowe z sekcji powyżej działają identycznie w Pythonie — moduł `re` implementuje ten sam standard wyrażeń regularnych. Różnica jest tylko w API: zamiast `REGEXP` w SQL, w Pythonie wywołuje się funkcje modułu `re`.

Wzorce warto zapisywać jako **raw string** (`r"..."`), żeby `\` nie było interpretowane jako znak specjalny Pythona.

```python
import re
```

---

## re.match()

Sprawdza dopasowanie **tylko na początku** tekstu. Zwraca obiekt `Match` lub `None`.

```python
result = re.match(r'^[0-9]+', "123abc")
if result:
    print(result.group())  # "123"
```

---

## re.fullmatch()

Sprawdza, czy **cały** tekst pasuje do wzorca (odpowiednik `'^...$'`).

```python
result = re.fullmatch(r'[0-9]+', "12345")
print(bool(result))  # True
```

---

## re.search()

Szuka dopasowania **w dowolnym miejscu** tekstu, zwraca pierwsze znalezione.

```python
result = re.search(r'[0-9]+', "abc123def")
if result:
    print(result.group())  # "123"
```

---

## re.findall()

Zwraca **listę wszystkich** dopasowań (jako stringi, lub krotki, jeśli wzorzec ma grupy).

```python
result = re.findall(r'[0-9]+', "a1 b22 c333")
print(result)  # ['1', '22', '333']
```

---

## re.finditer()

Jak `findall()`, ale zwraca **iterator obiektów Match** (dostęp do pozycji, grup itd.).

```python
for match in re.finditer(r'[0-9]+', "a1 b22 c333"):
    print(match.group(), match.start(), match.end())
```

---

## re.sub()

Zamienia dopasowania na inny tekst.

```python
result = re.sub(r'[0-9]+', '#', "a1 b22 c333")
print(result)  # "a# b# c#"
```

Z funkcją zamiast stringa:

```python
result = re.sub(r'[0-9]+', lambda m: str(int(m.group()) * 2), "a1 b22")
print(result)  # "a2 b44"
```

Limit liczby zamian: `re.sub(wzorzec, zamiennik, tekst, count=1)`

---

## re.split()

Dzieli tekst według wzorca.

```python
result = re.split(r'[,;]\s*', "a, b; c,d")
print(result)  # ['a', 'b', 'c', 'd']
```

---

## re.compile()

Kompiluje wzorzec do obiektu, który można wielokrotnie używać (szybsze przy powtarzalnym stosowaniu tego samego wzorca).

```python
pattern = re.compile(r'[0-9]+')

pattern.match("123abc")
pattern.search("abc123")
pattern.findall("a1 b22")
pattern.sub('#', "a1 b22")
```

---

## Grupy

Nawiasy `()` wydzielają grupy — dostępne przez `.group(n)` lub `.groups()`.

```python
result = re.search(r'(\d{3})-(\d{4})', "tel: 123-4567")
print(result.group())   # "123-4567"
print(result.group(1))  # "123"
print(result.group(2))  # "4567"
print(result.groups())  # ("123", "4567")
```

Grupy nazwane — `(?P<nazwa>...)`, dostęp przez `.group('nazwa')`:

```python
result = re.search(r'(?P<kierunkowy>\d{3})-(?P<numer>\d{4})', "123-4567")
print(result.group('kierunkowy'))  # "123"
print(result.group('numer'))       # "4567"
print(result.groupdict())          # {'kierunkowy': '123', 'numer': '4567'}
```

---

## Flagi

Podawane jako dodatkowy argument `flags=`:

- `re.IGNORECASE` (`re.I`) – ignoruje wielkość liter
- `re.MULTILINE` (`re.M`) – `^` i `$` dopasowują początek/koniec **każdej linii**, nie całego tekstu
- `re.DOTALL` (`re.S`) – `.` dopasowuje też znak nowej linii
- `re.VERBOSE` (`re.X`) – pozwala formatować wzorzec z białymi znakami i komentarzami

```python
re.findall(r'kot', "Kot i KOT", flags=re.IGNORECASE)  # ['Kot', 'KOT']

re.findall(r'^\w+', "linia1\nlinia2", flags=re.MULTILINE)  # ['linia1', 'linia2']
```

---

## Skróty klas znaków (Python)

Dodatkowo do klas z sekcji SQL, Python udostępnia skróty:

- `\d` – cyfra (jak `[0-9]`)
- `\D` – nie-cyfra
- `\w` – znak słowa: litera, cyfra, `_`
- `\W` – nie-znak słowa
- `\s` – biały znak (spacja, tab, nowa linia)
- `\S` – nie-biały znak
- `\b` – granica słowa

```python
re.findall(r'\b\w+\b', "To jest test")  # ['To', 'jest', 'test']
```

---

## Obiekt Match
Obiekt `Match` (często nazywany w kodzie `match`) to specjalny obiekt tworzony przez bibliotekę `re` w Pythonie, gdy funkcje takie jak `re.search()` lub `re.match()` znajdą w tekście to, czego szukałeś.

Możesz o nim myśleć jak o raporcie z sukcesu. Nie jest to zwykły tekst, ponieważ zawiera mnóstwo informacji o tym, gdzie, co i jak zostało znalezione.

Oto co najciekawszego znajdziesz w obiekcie `Match` i co możesz z nim zrobić:

---

### 1. Pozycje w tekście (Metody `.start()`, `.end()`, `.span()`)

Obiekt dokładnie wie, w którym miejscu Twojego stringa (na jakich indeksach) znajduje się dopasowany tekst.

- `match.start()` – zwraca indeks, na którym _zaczyna się_ dopasowanie.
- `match.end()` – zwraca indeks, na którym _kończy się_ dopasowanie.
- `match.span()` – zwraca krotkę `(start, end)`, czyli pełny przedział pozycji.

Przykład:

```python
text = "Kod HTML strony..."
match = re.search(r'<title>(.*?)</title>', text)

print(match.span())   # Wypisze np. (342, 385)
print(text[342:385]) # Wytnie dokładnie cały dopasowany fragment
```

---

### 2. Zaawansowane grupy (Metody `.groups()` i `.groupdict()`)

Jeśli Twój regex ma więcej niż jeden nawias, zarządzanie numerami `.group(1)`, `.group(2)` robi się trudne. Obiekt Match ma na to świetne metody:

- `match.groups()` – zwraca krotkę (tuple) zawierającą wszystkie przechwycone grupy na raz (pomijając grupę zerową).
- Grupy nazwane i `match.groupdict()` – w regexie możesz nadawać grupom własne nazwy za pomocą składni `(?P<nazwa>...)`. Metoda `.groupdict()` wyciąga je jako czytelny słownik (dict).

Przykład z grupami nazwanymi:

```python
# Szukamy tytułu i wersji językowej w jednym regexie
wzorzec = r'<title lang="(?P<jezyk>\w+)">(?P<tytul>.*?)</title>'
html = '<title lang="pl">Moja Strona</title>'

match = re.search(wzorzec, html)

print(match.groupdict()) 
# Wynik: {'jezyk': 'pl', 'tytul': 'Moja Strona'}

print(match.group('tytul')) 
# Wynik: 'Moja Strona' (możesz użyć nazwy zamiast numeru!)
```

---

### 3. Informacje o samym wyszukiwaniu (Atrybuty `.re` i `.string`)

Obiekt Match pamięta kontekst, w jakim został stworzony:

- `match.string` – zwraca cały oryginalny tekst, który przekazałeś do przeszukania (w Twoim przypadku to byłby cały kod HTML strony). Atrybut ten jest tylko do odczytu.
- `match.re` – zwraca skompilowany obiekt wyrażenia regularnego, który został użyty do tego wyszukiwania.

---

### Podsumowanie w jednym przykładzie:

```python
import re

html = "Id: 123, Nazwa: HorseOwner"
match = re.search(r"Id: (\d+), Nazwa: (\w+)", html)

if match:
    print(match.group(0))   # "Id: 123, Nazwa: HorseOwner" (całość)
    print(match.group(1))   # "123" (pierwszy nawias)
    print(match.groups())   # ('123', 'HorseOwner') (wszystkie nawiasy)
    print(match.span())     # (0, 26) (pozycja w tekście)
    print(match.string)     # "Id: 123, Nazwa: HorseOwner" (cały przeszukiwany tekst)
```

Warto pamiętać: jeśli Regex nic nie znajdzie, `re.search()` zwraca `None`. Próba wywołania jakiejkolwiek metody (np. `None.span()`) wyrzuci słynny błąd `AttributeError`, dlatego zawsze przed użyciem obiektu `Match` warto sprawdzić `if match:`.
