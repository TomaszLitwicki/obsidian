W Python'ie wszystko jest obiektem, nawet funkcje
Funkcje są First Class Objects.  
FUNKCJE i METODY można traktować jak pełnoprawne obiekty. 
[Zobacz na Wikipedii](https://pl.wikipedia.org/wiki/Typ_pierwszoklasowy)

potęga domknięć (closure).

### Funkcja w funkcji
```
def wydruk():  
    print("Jestem funkcją WYDRUK")  
  
def dekorator(funkcja):  
    print("To się dzieje PRZED wydrukiem")  
    funkcja()  
    print("To się dzieje PO wydruku")  
  
dekorator(wydruk)
```
*To się dzieje PRZED wydrukiem
Jestem funkcją WYDRUK
To się dzieje PO wydruku*

Opakowywanie funkcji w quasi-dekorator, na przykładzie liczenia czasu wywołania funkcji:
``` dekorator
import time  
  
def wydruk():  
    print("Jestem funkcją WYDRUK")    
  
def timer(funkcja):  
    print(start_time := time.time())  
    funkcja()  
    print(end_time := time.time())  
    print((end_time - start_time)*1000)  
  
timer(wydruk)
```
*1757411626.2825527
Jestem funkcją WYDRUK
1757411626.2825835
0.030755996704101562*

### DEKORATOR
Dekorator przyjmuje funkcję, dekoruje tą funkcję czyli definiuje wszystkie operacje dookoła funkcji i w odpowiedzi zwraca funkcję.
W praktyce wewnątrz definicji dekoratora nowa funkcja (osłonowa - wrapper) wewnątrz której przekazane są instrukcje co ma się wydarzyć i dekorator zwraca (return) funkcję udekorowaną. 
Przykład: 
``` wrapper
import time  
  
def timer(funkcja):  
    def wrapper():  
        print(start_time := time.time())  
        funkcja()  
        print(end_time := time.time())  
        print((end_time - start_time)*1000)  
  
    return wrapper  
  
def dekorator(funkcja):  
    def wrapper():  
        print("To się dzieje PRZED wydrukiem")  
        funkcja()  
        print("To się dzieje PO wydruku")  
  
    return wrapper
    
def wydruk():  
    print("Jestem funkcją WYDRUK") 
```

Wywołanie dekoratorów (znów funkcja w funkcji)
``` pierwszy_dekorator
wydruk_funkcji_udekorowanej_timerem = timer(wydruk)  
wydruk_funkcji_udekorowanej_timerem()
```
*1757413237.756024
Jestem funkcją WYDRUK
1757413237.75605
0.026226043701171875*
``` drygi_dekorator
wydruk_funkcji_udekorowanej_dekoratorem = dekorator(wydruk)  
wydruk_funkcji_udekorowanej_dekoratorem()
```
*To się dzieje PRZED wydrukiem
Jestem funkcją WYDRUK
To się dzieje PO wydruku*

### **ŁĄCZENIE DEKORATORÓW**
``` oba_dekoraoty_v1
oba_dekoratory = timer(dekorator(wydruk))  
oba_dekoratory()
```
*1757413586.477343
To się dzieje PRZED wydrukiem
Jestem funkcją WYDRUK
To się dzieje PO wydruku
1757413586.4773784
0.03528594970703125*
``` oba_dekoratory_v2
oba_dekoratory = dekorator(timer(wydruk))  
oba_dekoratory()
```
*To się dzieje PRZED wydrukiem
1757413696.7216656
Jestem funkcją WYDRUK
1757413696.7216792
0.013589859008789062
To się dzieje PO wydruku*

### OPIS DEKOR@TORÓW
za pomocą @ 
``` opis
@timer  
@dekorator  
def wydruk():  
    print("Jestem funkcją WYDRUK")  
  
wydruk()
```

# inne przykłady

## 1.
```
def dekorator_plci (funkcja):
    def wrapper(imie):
        if imie.endswith('a'):
            print(f'{imie.upper()} - jesteś KOBIETĄ')
        else:
            print(f'{imie.upper()} - jesteś MĘŻCZYZNĄ')
        return funkcja(imie)
    return wrapper

@dekorator_plci
def powitanie(imie):
    print(f"Cześć {imie}! Miło Cię tu widzieć :)")

powitanie(imie:=input('Jak masz na imię? '))
```

## 2.
```
from time import time
from functools import wraps

def licze_czas(funkcja):
    @wraps(funkcja)
    def wrapper(*args):
        print(f'START - {(t0 := time())}')
        obliczenia = funkcja(*args)
        print('Tworzę listę...')
        print(f'STOP - {(t1 := time())}')
        print(f'CZAS = {(t1 - t0) * 1000:.4f} msek.')
        return obliczenia
    return wrapper

@licze_czas
def ciag (n):
    """Funkcja zwraca listę wartości n^n czyli OEIS A000312"""
    return [pow(i,i) for i in range(1,n+1)]


print(f'oto Twoja lista OEIS: {ciag(int(input("Podaj n: ")))}')
print(ciag.__name__)
print(ciag.__doc__)
print(ciag.__dict__)
```
