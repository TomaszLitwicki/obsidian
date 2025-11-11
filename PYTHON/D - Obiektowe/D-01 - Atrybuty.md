1. Klasy
2. Instancji 

```
class User:
    company = 'Ranczo Rajczyn' -> atrybut klasy
    
    def __init__(self, name, born):
        self.name = name -> 1 atrybut instancji
        self.born = born -> 2 atrybut instancji


cowboy = User('Tomasz', 1987)
pies1 = User('Tori', 2019)
pies2 = User('Figo', 2021)

print(cowboy)
print(cowboy.name)
print(cowboy.born)
print(cowboy.company)
```

Tworzenie instacji klasy:
`user = User('Tomasz', 37)`

Wyświetlanie:
`print(f'Imię: {user.name}; Wiek: {user.age}; Miejsce pracy: {user.company}')`
*Imię: Tomasz; Wiek: 37; Miejsce pracy: Ranczo Rajczyn*

Zmienianie atrybutów instancji:
`user.name = 'Kasia'`
`user.age = 42`
