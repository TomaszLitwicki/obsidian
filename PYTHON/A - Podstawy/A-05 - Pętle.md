### WHILE
Wykonywana dopóki spełniony jest określony warunek
```
while WARUNEK:
	INSTRUKCJA
```
### FOR
Przechodzi przez wszystkie elementy sekwencji
	Sekwencja to uporządkowana kolekcja obiektów
```
for NazwaKażdegoElementuSekwencji in TypSekwencji (ZakresL,ZakresP)
	sekwencje
```

Typy sekwencji:
	- range(liczba powtórzeń)
	- range(start, stop)
	- enumerate(sekwencja)
	- lista itp...

_ - Podkreślenie w pętli `for _ in range (0,5):` oznacza, że tej zmiennej nie wykorzystujemy w sekwencji

` for i in lista[:]` - tworzy płytką kopię listy do iteracji 

### BREAK i CONTINUE 
continue - przerywa aktualne wywołanie pętli i przechodzi do kolejnego wywołania.
breake - przerywa działanie pętli


