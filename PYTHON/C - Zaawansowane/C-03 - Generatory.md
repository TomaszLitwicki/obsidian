**OBIEKT** Funkcja
elementy nie są zapisane w pamięci tylko generowane na bieżąco i  zwracane .
**==yield==** - komenda odpowiedzialna w funkcji za aby wartości były zwracane na bieżąco (a nie return, które zapisuje wszystkie operacje w pamięci)

bo przykładowo zliczenie sumy miliarda elementów zamorduje pamięć:
LIST COMPREHENSION:
`print(sum([number for number in range(1_000_000_000)]))`
bo każda wartość jest zapisywana do listy!!!
# I metoda za pomocą funkcji
``` funkcja
def bilion_integers():
	for number in range(1_000_000_000):
		yield number

print(sum(bilion_integers()))
# 499999999500000000 :)
```

# II metoda za pomocą expresion
**Generator expresions** lub **generator comprehension**
zamiast list comprehension:
`print(sum([number for number in range(1_000_000_000)]))`
wystarczy usunąć nawiasy klamrowe \[ \]
`print(sum(number for number in range(1_000_000_000)))`

# Rozróżnienie metod
```
1. list_comprehension = [number for number in range (1_000)]
2. generator_comprehension = (number for number in range (1_000))
```
typy zmiennych
1 - to **lista** 1000 liczb przechowywanych w pamięci podręcznej
2 - to tyko **generator** z  możliwym dostępem to tych liczb jedna po drugiej

# Generowanie wyniku
aby wygenerować wynik należy użyć funkcji #islice z  [[C-13 - Moduł itertools]]
i przypisać go to listy lub funkcji next().

# Przykłady
## Fibonacci
```
def ciag_fibonacciego (q):
    a, b = 0, 1
    for _ in range(q+1):
        yield a
        a, b = b, a+b
m = int(input('Podaj element ciągu Fibonnaciego: '))
```
Wyciągnięcie konkretnego elementu z generatora można zrobić za pomocą modułu:
`from itertools import islice`
używając funkcji itertools.islice():
`wynik = next(islice(ciag_fibonacciego(m),m,None))`

