# PEP8 - Style Guide for Python Code
[https://peps.python.org/pep-0008/](oficjalna instrukcja PEP8)

Konwencja wprowadzania nazw 
	camelCase - nazywanie zmiennych w Java
	snake_case - nazywanie zmiennych w Python
	PascalCase - przy wprowadzaniu class w Pythonie (CapWords)
	PI=3.14 MAX_SPEED = 300 - wprowadzanie stałej (niezmienna)
	nazwy modułów nie zaczynają się od liczb
	max ilość znaków w linii to 79
	przypisywanie wartości do zmiennej
		domyślna `defoult=1
		zmienna `var = 1
	
Na końcu plika zawsze jedna linijka pusta

# Elementy pisania kodu
- Instrukcje - statements - for, while, if (wykonanie akcji, stan programu, nie zwracają wartości)
- wyrażenia - expressions - a+b, a>0 (zwracają wartość)
- 
# Klasy i moduły:
**import moduł from klasa**

instancja klasy (self)
klasa - szablon
metoda to funkcja przypisana do konkretnej klasy

# Różnica ATRYBUT/PARAMETR vs METODA:
## Atrybut
- nie wymaga nawiasów ()
- odczytuje cechy obiektu
- przechowuje jakąś wartość lub obiekt
- przykład: `df.shape

zwraca krotkę (tuple) (liczba_wierszy, liczba_kolumn), np. (1000, 12)
## Metoda
 - to funkcja należąca do obiektu
 - do wykonywania akcji
 - wywołujesz ją z nawiasami ()
 - przykład:
```Python
df.head()
df.describe()
df.sort_values('Released_Year')
```

## W Python'ie rozróżnia się to po nawiasach i kontekście.

1. Atrybut / parametr / własność obiektu
	- używasz kropki bez nawiasów
	- przykłady: `request.args`, `user.name`, `config.debug`
	- to jest wartość, zmienna, obiekt lub stały atrybut

2. Metoda (czyli funkcja należąca do obiektu)
	- po nazwie są okrągłe nawiasy ()
	- przykłady: `request.args.get()`, `user.save()`, `list.append(3)`
	- wywołanie metody wykonuje jakąś operację

Czyli:
```
request.args        # obiekt (ImmutableMultiDict)
request.args.get    # referencja do funkcji
request.args.get()  # faktyczne wywołanie metody
```

Możesz to zawsze sprawdzić:
```
type(request.args)        # pokaże typ obiektu
type(request.args.get)    # funkcja/metoda
```

Prosta logika:
- ma nawiasy → metoda się wykonuje
- nie ma nawiasów → tylko dostęp do pola / obiektu

Przykład analogiczny:
```
lista = [1,2,3]

lista.append      # metoda (jeszcze nie wywołana)
lista.append(4)   # metoda wywołana
lista[0]          # dostęp do elementu, nie metoda
```

Więc w twoim kodzie:
- `request` – obiekt żądania
- `request.args` – atrybut (słownik parametrów z URL)
- `request.args.get('gender')` – metoda `get()` wywołana na słowniku

Nie ma magii. Nawias = metoda. Brak nawiasu = własność.

# Pliki

* [CSV](https://pl.wikipedia.org/wiki/CSV_(format_pliku)) - comma-separated values (wartości oddzielone przecinkiem)
* [JSON](https://pl.wikipedia.org/wiki/JSON) - lekki format wymiany danych komputerowych. JSON jest formatem tekstowym, bazującym na podzbiorze języka JavaScript. (https://jsonlint.com/)
# Nazwy
#PARSOWANIE „parse” pochodzi z łacińskiego pars orationis, czyli „część mowy”.
Czyli dosłownie: „rozpoznawać części mowy w zdaniu”.
Tłumaczenie surowego tekstu (ciągu znaków) na strukturę, którą komputer może logicznie zrozumieć. **Rozumienie struktury tekstu przez jego rozłożenie na części i analizę zależności między nimi.**
#WALIDACJA to sprawdzenie poprawności danych – czyli upewnienie się, że dane są takie, jakich się spodziewasz, zanim zaczniesz ich używać.
#ZSERCJA to instrukcja w kodzie sprawdzająca, że dany warunek jest prawdziwy.

