# Kolejność
Kolejność argumentów w funkcji:
==POZYCYJNE , \*ARGS, NAZWANE, NAZWANE z wart.domyślną, \*\*KWARGS==
# \*args
\* - dowolna liczba argumentów pozycyjnych
**args** to konwencja, zazwyczaj oznaczana słowem *args* (ale może być innym)

` def funkcja(*args):`
`funkcja('argument1', 'argument2', 'argument_kolejny...')
args - jest zwracany jako TUPLES (krotka)
przed *\*args* (argumenty pozycyjne o zmiennej liczbie) może być dowolna liczba innych argumentów pozycyjnych
Ale przed wywołaniem funkcji z dowolną liczbą argumentów z #lista lub #krotka należy ja wpierw rozpakować z użyciem \* !!!
`suma_elementow_listy = funkcja_zliczajaca(*lista_elementow)

# \** kwargs
\** - dowolna liczba argumentów kluczowych (nazwanych)
**kwargs**- zmienna liczba argumentów nazwanych
kwargs zwracay jest jako DICT (słownik)
``` kwargd
def info_uzytkownik(**kwargs):
    user_info = {}
    for key, value in kwargs.items():
        user_info[key] = value

    return user_info

uzytkownik = info_uzytkownik(imie='Tomasz', wiek=38)
print(uzytkownik)
```
*{'imie': 'Tomasz', 'wiek': 38}*

# args i kwargs razem
args należy umieścić po wszystkich argumentach pozycyjnych!
 ``` args_i_kwargs
 def info_uzytkownik(user_info, *args, is_active=True, **kwargs):  
    for key, value in kwargs.items():  
        user_info[key] = value  
  
    user_info['is_active'] = is_active  
    user_info['zwierzeta'] = args  
  
    return user_info  
  
user_info = {}  
uzytkownik = info_uzytkownik(user_info, 'konie', 'psy', 'myszy', is_active=False, imie='Tomasz', wiek=38)  
print(uzytkownik)
```

# #Rozpakowywanie 
```
def show_args(*args):
    print(args)
```

| Wywołanie               | Co trafi do `args` | Typ w środku              |
| ----------------------- | ------------------ | ------------------------- |
| `show_args(1, 2, 3)`    | `(1, 2, 3)`        | krotka z trzema liczbami  |
| `show_args([1, 2, 3])`  | `([1, 2, 3],)`     | krotka z **jedną listą**  |
| `show_args((1, 2, 3))`  | `((1, 2, 3),)`     | krotka z **jedną krotką** |
| `show_args(*[1, 2, 3])` | `(1, 2, 3)`        | krotka z trzema liczbami  |
| `show_args(*(1, 2, 3))` | `(1, 2, 3)`        | krotka z trzema liczbami  |
| `show_args("ABC")`      | `('ABC',)`         | krotka z jednym stringiem |
| `show_args(*"ABC")`     | `('A', 'B', 'C')`  | krotka z trzema znakami   |


