## Kontekst

`map()` i `lambda` są często używane razem w stylu **functional programming** do przekształcania danych.

---

## map()

### Definicja

`map()` to **higher-order function** — funkcja przyjmująca inną funkcję jako argument i stosująca ją do każdego elementu iterowalnego obiektu.

### Składnia

```python
map(function, iterable)
```

**Zwraca:** iterator (`map object`) → działa w trybie **lazy evaluation**

### Jak działa (koncepcyjnie)

```python
for element in iterable:
    yield function(element)
```

### Przykład

```python
num = [1, 2, 3]
result = map(int, ["1", "2", "3"])
print(list(result))  # [1, 2, 3]
```

### Cechy

* styl: funkcyjny
* niskie zużycie pamięci
* użycie: transformacje danych

---

## lambda

### Definicja

`lambda` tworzy **anonymous function** — krótką funkcję bez nazwy, zawierającą jedno wyrażenie.

### Składnia

```python
lambda arguments: expression
```

**Właściwości:**

* brak nazwy
* brak `return` (wynik zwracany automatycznie)
* tylko jedno wyrażenie

### Odpowiednik klasycznej funkcji

```python
def square(x):
    return x ** 2
```

to samo co:

```python
lambda x: x ** 2
```

---

## map() + lambda

```python
num = [1, 2, 3, 4]
squared = list(map(lambda x: x ** 2, num))
```

**Mechanizm działania:**

* `lambda` definiuje operację
* `map()` stosuje ją do każdego elementu

---

## map vs list comprehension

| map + lambda          | list comprehension     |
| --------------------- | ---------------------- |
| styl funkcyjny        | styl pythoniczny       |
| bardziej abstrakcyjne | bardziej czytelne      |
| dobre w pipeline      | najczęstsze w praktyce |

Odpowiednik:

```python
[x ** 2 for x in num]
```

---

## Kiedy używać

**map():**

* jednolita transformacja
* przetwarzanie strumieni danych

**lambda:**

* funkcja krótka i jednorazowa
* jako argument do `map`, `filter`, `sorted`

---

## Kiedy unikać

* złożona logika
* funkcja używana wielokrotnie
* spadek czytelności

---

## Pojęcia (EN)

* higher-order function
* anonymous function
* functional transformation
* lazy evaluation

---

## Esencja

**`lambda` definiuje operację, a `map()` stosuje ją do każdego elementu danych.**
