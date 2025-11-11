## definiowanie

`def nazwa(parametry_fukcji):
	`wszystko w tej fukcji

`parametry_funkcji (parametry_wymagane, parametry_opcjonalne)

DEFiniowanie fukcji ustala parametry. Przy wywoływaniu funkcji, do funkcji przekazujemy argumenty. Funkcja ma takie i takie argumenty.

Parametrom opcjonalnym można przypisywać wartość n.p. None i od tego uwarunkować wykonanie instrukcji w funkcji.

## wywołanie

Wywołanie funkcji za pomocą argumentów pozycyjnych (positional arguments)
```
def info(name, age):  
    print("The dog " + name + " is " + str(age) + " y.o.")  
  
  
info('Tori', 6)  
info('Figo', 4)  
info('Wrangler',6)
```

Wywoływanie funkcji po argumentach kluczowych (keyword arguments)
```
from _datetime import datetime  
  
def info(name, age):  
    print(name + " is " + str(age) + " y.o.")  
  
year = datetime.now().year  
  
info(name='Tomasz', age=(year-1987))  
info(name='Kasia', age=(year-1982))
```

UWAGA!
Po użyciu argumentów kluczowych, nie można używać argumentów pozycyjnych. Natomiast w drugą stronę to działa.

## KEYWORD-ONLY-ARGUMENTS

Wywołanie funkcji tylko po nazwie od znaku *
```
def create_user(name, *, age, active):
    print(name, age, active)

create_user("Tom", age=35, active=True)
```


