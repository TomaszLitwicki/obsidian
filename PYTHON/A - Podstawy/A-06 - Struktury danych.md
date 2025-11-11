# ITEROWALNE (iterable)
[[A-06-1 - Listy]]
[[A-06-2 - Sety]]
[[A-06-3 - Słowniki]]
[[A-06-4 - Krotki]]
[[A-03 - String i konwencje]]
[[B-02 - With]]

Obiekty iterowalne można zrobić #Rozpakowywanie 
# ITERATOR

[[C-03 - Generatory]]
[[C-13 - Moduł itertools]]

To obiekt, który pamięta „gdzie jest” i ma metodę `__next__()`.
Każde next() zwraca kolejną wartość, aż do końca, kiedy rzuca StopIteration.

Każdy iterator ma też `__iter__()`, które zwraca samego siebie – dzięki temu można go używać w pętli for.

`iter(obj)` - zamienia obiekt iterowalny (czyli taki, po którym można robić for) w iterator.
`iter(callable, sentinel)` - To taki sprytny tryb: tworzysz iterator, który woła podaną funkcję aż do momentu, gdy ta zwróci wartość równą „strażnikowi” (sentinel).



