==Ukrywanie implementacji obiektu==

mangling - zniekształcenie
# Prywatne ATRYBUTY
tworzne za pomocą podkreślenia _ :
```
class Zwierz:
    def __init__(self, imie, wiek):
        self.name = imie
        self.age = wiek

        if self.name.endswith('a'):
            self._plec = 'Klacz'  <--- ENKAPSULACJA ATRYBUTU
        else:
            self._plec = 'Ogier'  <--- ENKAPSULACJA ATRYBUTU
```

Atrybutów prywatnych nie można wywoływać (to znaczy nie powinno się... ale da się). Za to można je wykorzystać do metody

```
    def potomstwo(self):
        return 'Może mieć źrebaki' if self._plec == "Klacz" else "Nie może mieć źrebaków"

kon1 = Zwierz('Namira', 7)

print(f'Koń o imieniu {kon1.name} {kon1.potomstwo()}')
```

# Prywatne METODY
```
class Zwierz:
    def __init__(self, imie, ):
        self.name = imie
        self._plec = self._okresl_plec()  <-- ENKAPSULACJA ATRYBUTU

    def _okresl_plec(self):   <-- ENKAPSULACJA METODY
        return 'Klacz' if self.name.endswith('a') else 'Ogier'

    def potomstwo(self):  <-- JAWNA METODA WYKORZYSTUJĄCA 
        return 'TAK, może mieć źrebaki' if self._plec == 'Klacz' else 'Nie, nie może mieć źrebaków'

kon1 = Zwierz('Namira')
kon2 = Zwierz('Moro')

print(f'Koń o imieniu {kon1.name} - {kon1.potomstwo()}')
print(f'Koń o imieniu {kon2.name} - {kon2.potomstwo()}')
```

# Name Mangling
gdy użyje się podwójnego podkreślenia przed atrybutem lub metodą, wówczas jest pod spodem tak zniekształcony, że nie da się wywołać:
```
class Zwierz:
    def __init__(self, imie, wiek):
        self.name = imie
        self._plec = self._okresl_plec()
        self.__age = wiek   <-- MANGLING (ukrycie)

    def pelnoletnosc(self):   <-- tylko wewnątrz można przekształcić!
        return 'PEŁNOLETNI' if self.__age >=18 else 'NIEPEŁNOLETNI'

kon1 = Zwierz('Namira',12)
kon2 = Zwierz('Moro', 18)

print(kon1.__age)   <-- GENERUJE BŁĄD !!!
print(f'KOŃ {kon1.name} jest {kon1.pelnoletnosc()}')
print(f'KOŃ {kon2.name} jest {kon2.pelnoletnosc()}')
```
Ale można to obejść w taki sposób:
nazwa zmiennej . \_ nazwa klasy _ _ ukryty atrybut lub metoda
`print(kon1._Zwierz__age)`

