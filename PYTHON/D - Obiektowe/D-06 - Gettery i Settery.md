 `GETTER (zwraca wartość) - @property`
 `SETTER (ustawia wartość) - @nazwa_atrybutu_knstruktora.setter`

Przy tworzeniu INSTANCJI KLASY chcemy wpływać na ATRYBUTY ( zmienić lub zweryfikować) jakimiś instrukcjami. Ale to nie działa już przy zmienianiu istniejących ATRYBUTÓW KLASY, tylko przy tworzeniu. Wówczas potrzebne są GETTERY i SETTERY, które działają zawsze!

```
class User:
    company = 'Ranczo Rajczyn'
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @property       #<-- dekorator tworzący GETTERA
    def name(self):
        return self._name

    @name.setter    #<-- dekorator tworzący SETTERA
    def name(self, name):
        self._name = name.upper()

    @property       #<-- dekorator tworzący GETTERA
    def age(self):
        return self._age

    @age.setter    #<-- dekorator tworzący SETTERA
    def age(self, age):
        if age >= 18:
            self._age = "PEŁNOLETNI"
        else:
            self._age = "niepełnoletni"

user = User('Tomasz', 37)   #<-- tworzenie INSTANCJI KLASY
print(f'Imię: {user.name}; Wiek: {user.age}; Miejsce pracy: {user.company}')
user.name = 'Kasia'                    #<-- zmiany INSTANCJI
user.age = 17                          # ale i tak przchodzi przez SETTERA
print(f'Imię: {user.name}; Wiek: {user.age}; Miejsce pracy: {user.company}')

```

## Dodatkowe wykorzystanie @property
### Jak działa
W Pythonie dekorator @property zamienia metodę w atrybut tylko do odczytu.
Czyli zamiast pisać:
`ranczo1.age()
możesz napisać:
`ranczo1.age
— bez nawiasów, jakby to była zwykła zmienna, choć w rzeczywistości pod spodem nadal działa funkcja.
### Dlaczego to się robi
Z punktu widzenia użytkownika klasy: wiek to cecha obiektu, nie czynność.
Nie myślimy „oblicz wiek()”, tylko „wiek wynosi…”.
Python pozwala nam więc ukryć wywołanie metody za atrybutem, zachowując pełną logikę obliczeń.
### Jak to wygląda w praktyce

Bez @property:
```
def age(self):
    return 2025 - self.born

print(ranczo1.age())   # <- trzeba dodać nawiasy
```

Z @property:
```
@property
def age(self):
    return 2025 - self.born

print(ranczo1.age)     # <- wygląda jak zwykły atrybut, ale to metoda
```
### Co to daje

Możesz ukryć logikę obliczeń i nadal mieć prosty interfejs (ranczo1.age).

Jeśli w przyszłości zmienisz sposób liczenia wieku, nie musisz zmieniać sposobu wywoływania w całym kodzie.

Możesz później dodać setter dla tego samego atrybutu, np.:
```
@age.setter
def age(self, value):
    self.born = 2025 - value
```

Dzięki temu można ustawić wiek, a Python automatycznie przeliczy rok urodzenia