`@staticmethod` <- dekorator metody statycznej

METODA STATYCZNA to taka, która istnieje w KLASIE ale nie jest powiązana z żadną INSTANCJĄ KLASY. Nie potrzeba tworzyć INSTANCJI KLASY, aby korzystać z METODY STATYCZNEJ. Czyli METODA STATYCZNA przyjmuje ARGUMENTY, ale później one nie są nigdzie dalej wykorzystywane, przechowywane, czy zwracane w postaci ATRYBUTÓW METODY. 

METODY STATYCZNE są używane do tworzenia KLAS NARZĘDZIOWYCH (użytkowych) - utility class.

```
class Ciagi:
    @staticmethod       #<- DEKORATOR METODY STATYCZNEJ
    def silnia (n):
        if n == 0:
            return 1
        else:
            return n * Ciagi.silnia(n - 1)

    @staticmethod       #<- DEKORATOR METODY STATYCZNEJ
    def fibonacci (n):
        if n == 0:
            return 0
        elif n == 1:
            return 1
        else:
            return Ciagi.fibonacci(n - 2)+Ciagi.fibonacci(n - 1)

print(Ciagi.silnia(5))
print(Ciagi.fibonacci(19))
```

Komentarz:
- Dekorator @staticmethod:
Dzięki niemu metoda działa jak zwykła funkcja, ale pogrupowana w klasie.
To daje porządek: wszystkie funkcje związane z ciągami masz w jednej przestrzeni nazw Ciagi.

