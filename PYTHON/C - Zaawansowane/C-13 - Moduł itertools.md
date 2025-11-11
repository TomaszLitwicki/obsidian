`from itertools import islice`

# Funkcja #islice 
`itertools.islice(iterable, start, stop, step)

iterable – dowolny obiekt iterowalny: lista, krotka, string, plik, generator…
start – indeks początkowy (włącznie). Indeksujemy od zera, jak w listach.
stop – indeks końcowy (wyłączny). Jeśli damy None, to ciągnie do końca.
step – (opcjonalny) krok. Domyślnie 1. Można np. wziąć co drugi element.

```
lista = ['a', 'b', 'c', 'd', 'e']
print(list(islice(lista, 2)))          # ['a', 'b']  (czyli od 0 do 2)
print(list(islice(lista, 2, 4)))       # ['c', 'd']  (czyli indeksy 2,3)
print(list(islice(lista, 1, None, 2))) # ['b', 'd']  (od 1, co 2 krok, do końca)


def kwadraty():
    n = 0
    while True:
        yield n*n
        n += 1

print(list(islice(kwadraty(), 5)))       # [0, 1, 4, 9, 16]
print(next(islice(kwadraty(), 10, None)))# 100 (dokładnie 10-ty kwadrat)
```

Formy wywołania:
islice(x, N) → traktuje N jako stop (od 0 do N).
islice(x, A, B) → A = start, B = stop.
islice(x, A, B, C) → A = start, B = stop, C = step.

Adekwatnie do [list slicing]()

# Funkcja next()
#next()` to kluczowy kawałek układanki przy generatorach i #islice.

`next(iterator, default)

iterator – dowolny obiekt, po którym można iterować (np. generator, iter(lista)).
default – (opcjonalny) – wartość zwracana, jeśli iterator się skończy.

Jeśli go nie podasz, a iterator się wyczerpie → dostaniesz wyjątek StopIteration.

Działanie:
Każde wywołanie next() uruchamia iterator do momentu, aż odda kolejną wartość.
Iterator „pamięta stan”, więc kolejne next() zwracają następne elementy, aż do końca.