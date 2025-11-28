
# Koncept wyrażeń lambda w Pythonie

Wyrażenie **lambda** w Pythonie to sposób tworzenia funkcji anonimowej, czyli funkcji bez nazwy. Wykorzystuje się je tam, gdzie potrzebna jest krótka, jednorazowa funkcja, bez konieczności pisania pełnej definicji za pomocą `def`.

---

## Składnia

```python
lambda argumenty: wyrażenie
```

Przykład:

```python
lambda x: x * 2
```

Działa tak samo jak:

```python
def multiply(x):
    return x * 2
```

---

## Typowe zastosowania

Wyrażenia lambda najczęściej wykorzystuje się:

- przy sortowaniu (`sorted`, `.sort`)
- w funkcjach wyższego rzędu (`map`, `filter`, `reduce`)
- w callbackach (np. przy tworzeniu GUI w Tkinter)
- gdy potrzebna jest krótka funkcja użyta tylko raz w kodzie

Przykład sortowania listy:

```python
words = ["python", "ai", "kod"]
sorted_words = sorted(words, key=lambda w: len(w))
```

---

## Lambda w callbackach na przykładzie Tkinter

W Tkinter parametr `command=` oczekuje referencji do funkcji, a nie wyniku jej działania. Jeśli funkcja przyjmuje argumenty, lambda pozwala przekazać je bez natychmiastowego wywoływania.

Przykład poprawny:

```python
file_menu.add_command(label='Dodaj album', command=lambda: add_album(root))
```

Przykład błędny (funkcja wykonałaby się przy starcie aplikacji):

```python
file_menu.add_command(label='Dodaj album', command=add_album(root))
```

---

## Ograniczenia

Wyrażenia lambda mają celowo wprowadzone ograniczenia:

- mogą zawierać tylko jedno wyrażenie (bez pętli, przypisań, `if` jako instrukcji blokowej)
- nie nadają się do bardziej złożonej logiki

Jeśli lambda zaczyna być nieczytelna lub zbyt rozbudowana, należy użyć normalnej funkcji `def`.

---

## Alternatywa: `functools.partial`

W sytuacji, gdy lambda służy głównie do przekazania argumentów funkcji, można zastosować `partial`:

```python
from functools import partial

file_menu.add_command(label='Dodaj album', command=partial(add_album, root))
```

---

## Podsumowanie

Wyrażenia lambda są przydatnym sposobem na zapisanie krótkich funkcji w miejscach, gdzie pełna definicja funkcji byłaby niepotrzebnym rozbudowaniem kodu. Najczęściej używa się ich w sortowaniu, przetwarzaniu danych i callbackach. Lambda powinna być stosowana tam, gdzie jej użycie zwiększa czytelność, a nie ją utrudnia.