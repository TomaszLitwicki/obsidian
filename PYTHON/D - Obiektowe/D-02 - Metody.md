Pierwsza metoda klasy to `__init__` która służy jako KONSTRUKTOR.
METODY to FUNKCJE powiązane z KLASĄ

```
class Czlonek_Stada:
    company = 'Ranczo Rajczyn'

    def __init__(self, name, born): ---> KONSTRUKTOR
        self.name = name
        self.born = born

    def is_adult(self): ---> PIERWSZA METODA
        return 2025 - self.born >= 18

    def print_description(self): ---> DRUGA METODA
        print(f'Ranczer {self.name} ma {2025 - self.born} lat')
        print(f'{"Jest pełnoletni" if self.is_adult() else "Jest niepełnoletni"} ')
        print(f'Mieszka na {self.company}')


cowboy = Czlonek_Stada('Tomasz', 1987)
pies1 = Czlonek_Stada('Tori', 2019)
pies2 = Czlonek_Stada('Figo', 2021)

print(cowboy)
print(cowboy.name)
print(cowboy.born)
print(cowboy.company)
print(cowboy.is_adult())
cowboy.print_description()
print()
print('I jego psy: ')
print(pies1.name, ' - ', pies1.born)
print(pies2.name, ' - ', pies2.born)
```
