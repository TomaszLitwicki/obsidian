 Definiowanie nowej KLASY na bazie istniejącej KLASY i odziedziczyć wszystkie ATRYBUTY i METODY i dodać nowe ZACHOWANIA.

KLASA BAZOWA -> KLASA POTOMNA 

INSTANCJE KLASY POTOMNEJ są INSTANCJAMI KLASY BAZOWEJ!!!
`isinstance(instancja_klasy_potomnej, instancja_klasy_bazowej)` -> True

```
class Konie(Czlonek_Stada): ---> KLASA POTOMNA(KLASA BAZOWA)
    def __init__(self, name, born, rasa):
        Czlonek_Stada.__init__(self, name=name, born=born) **
        self.rasa = rasa
        
kon1 = Konie('Namira', 2018, 'Koń mały')
```

Wywołanie->super() KONSTRUKTORA KLASY BAZOWEJ->SUPER KLASY:
```
class Konie(Czlonek_Stada):
    def __init__(self, name, born, rasa):
        super().__init__(name, born) <- przekazanie argumentów pozycyjnych **
        self.rasa = rasa
```

Wywołanie metody klasy bazowej:
```
class Psy(Czlonek_Stada):
    def __init__(self, name, born, color):
        super().__init__(name, born)
        self.color = color

    def opis_psa(self):
        super().print_description()   <-- Wywołasnie metody klasy bazowej
        print(f'Kolor psa to {self.color}')
```

W KLASIE POTOMNEJ można dodawać i ATRYBUTY i METODY!
```
...
    def opis_rasy(self):
        print(f'Koń {self.name}, rasa: {self.rasa}')
        self.print_description()
```

Można też zmieniać METODĘ w KLASIE BAZOWEJ nazywając ją tak samo:
```
    def print_description(self):
        print(f'KOŃ {self.name} ma {2025 - self.born} lat')
        print(f'{"Jest pełnoletni" if self.is_adult() else "Jest niepełnoletni"} ')
        print(f'Mieszka na {self.company} i jest rasy: {self.rasa}.')
```

Dziedziczenie po wielu klasach
```
class Konie:
    miejsce = 'Ranczo Rajczyn'

    def __init__(self, imie, grzbiet):
        self.name = imie
        self.size = grzbiet

    def opis (self):
        print(f'Imię konia: {self.name} - Rozmiar grzbietu to {self.size}')

class Siodlo:
    def __init__(self, nazwa, rozmiary):
        self.marka = nazwa
        self.dopasuj = rozmiary
    def opis (self):
        print(f'Siodło {self.marka} pasuje na rozmiary: {self.dopasuj}')

class Dopasowanie(Konie, Siodlo):
    def __init__(self, kon, siodlo):
        self.konie = kon
        self.siodlo = siodlo

    def czy_pasuje(self):
        if self.konie.size in self.siodlo.dopasuj:
            return f"Siodło {self.siodlo.marka} PASUJE na konia {self.konie.name}"
        else:
            return f"Siodło {self.siodlo.marka} NIE pasuje na konia {self.konie.name}"


kon1 = Konie('Namira', 55)
siodlo1 = Siodlo('Continental',[51,52,53,54,55,56,57])

zestaw = Dopasowanie(kon1, siodlo1)

kon1.opis()
siodlo1.opis()
print(zestaw.czy_pasuje())

```