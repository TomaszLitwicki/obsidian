Metody Specjalne (special/dunder/magic methods)
to wszystkie metody zaczynające się i kończące się podwójnym podkreśleniem
`__metodamagiczna__`
Czyli wszystko, co dzieje się pod spodem w Python'ie!

na przykład: 
- `print(NAPIS)` to `NAPIS.__str__` ;)
- `len(lista)` to `lista.__len__()`
- `+` to `(L1)__add__(L2)`
- `==` to `obj1.__eq__(obj2)`

# Metody do wyświetlenia
## Metoda STR
`__str__`
Zadaniem METODY STR jest wyświetlenie informacji w sposób przyjazny dla użytkownika.

Gdy nie ma danej metody specjalnej w danej klasie:
```
class User:
    def __init__(self, name):
       self.name = name

user1 = User('Tomasz')
print(user1)
```
*<__main__.User object at 0x0000022CFFA76900>* <-- DOMYŚLN IMPLEMENTACJA METODY

Można dostarczyć WŁASNĄ IMPLEMENTACJĘ!!!
```
class User:
    def __init__(self, name):
       self.name = name

    def __str__(self):      #<--WŁASNA IMPLEMENTACJA METODY STR
        return f'Użytkownik ma na imię {self.name}'

user1 = User('Tomasz')
print(user1)
```
*Użytkownik ma na imię Tomasz*

## Metoda repr
`__repr__`
Zadaniem METODY REPR jest wyświetlenie informacji w sposób przyjazny dla programisty.
```
class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):      #<--WYDRUK DLA UŻYTKOWNIKA
        return f'Użytkownik o imieniu {self.name} ma {self.age}.'

    def __repr__(self):      #<--WYDRUK DLA PROGRAMISTY
        return f'name={self.name}; age={self.age}'

user1 = User('Tomasz', 38)
print(user1)                #<-- WYWOŁANIE STR
print(repr(user1))          #<-- WYWOŁANIE REPR
```

# Metody do porównania
Podczas porównania dwóch różnych INSTANCJI o identycznych ATRYBUTACH:
```
user1 = User('Tomasz', 38)
user2 = User('Tomasz', 38)
print(user1 == user2)
```
Wynik to *False* ponieważ dla Python'a to dwa różne OBIEKTY
Jest sprawdzanie położenia na dysku, a one zajmują inne miejsca. W przeciwieństwie do tych nieszczęsnych list, co kiedyś się zdziwiłem że podstawienie pod nową zmienną innej listy nie tworzy nowej, tylko nowe odniesienie do istniejącej już listy.
## Metoda EQ
Porównuje wartość atrybutów obu OBIEKTÓw

## Metoda HASH
Odpowiedzialna za zwrócenie hash'u obiektu, ważne, gdy obiekt będzie użytkowany w SETACH i SŁOWNIKACH

```
class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):      #<--WYDRUK DLA UŻYTKOWNIKA
        return f'Użytkownik o imieniu {self.name} ma {self.age}.'

    def __repr__(self):      #<--WYDRUK DLA PROGRAMISTY
        return f'name={self.name}; age={self.age}'

    def __eq__(self, other): #<-- IMPLEMENTACJA PORÓWNANIA
        if isinstance(other, User):
            return self.name == other.name and self.age == other.age
        return False

    def __hash__(self):     #<-- do list i słowników
        return hash((self.name, self.age))


user1 = User('Tomasz', 38)
user2 = User('Tomasz', 38)
print(user1 == user2)       #<-- True bo ATRYBUTY takie same


# print(user1)                #<-- WYWOŁANIE STR
# print(repr(user1))          #<-- WYWOŁANIE REPR
```

## Metody porównawcze
Służą do porównywania dwóch obiektów za pomocą operatorów
```
__lt__() – <
__le__() – <=
__eq__() – ==
__ne__() – !=
__gt__() – >
__ge__() – >=
```

```
class Horse:
    def __init__(self, name, speed):
        self.name = name
        self.speed = speed

    def __lt__(self, other):
        return self.speed < other.speed

    def __repr__(self):
        return f"{self.name} ({self.speed} km/h)"

horses = [
    Horse("Namira", 45),
    Horse("Wiosenka", 50),
    Horse("Moro", 42)
]

sorted_horses = sorted(horses)  # korzysta z __lt__()
print(sorted_horses)
```
`[Moro (42 km/h), Namira (45 km/h), Wiosenka (50 km/h)]
