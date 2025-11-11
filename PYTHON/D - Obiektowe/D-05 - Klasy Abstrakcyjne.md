`from abc import ABC, abstractmethod`

- KLASA ABSTRAKCYJNA oprócz regularnych metod definiuje METODY ABSTRAKCYJNE 
- METODA ABSTRAKCYJNA to METODA która ma zdefiniowaną  SYGNATURĘ, ale nie ma zdefiniowanego CIAŁA
- Aby KLASA była ABSTRAKCYJNA musi posiadać przynajmniej jedną METODĘ ABSTRAKCYJNĄ
- Nie da się utworzyć INSTANCJI KLASY ABSTRAKCYJNEJ 
- KLASA ABSTRAKCYJNA ==wymaga== aby KLASY DZIEDZICZĄCE zaimplementowały METODĘ ABSTRAKCYJNĄ 

```
from abc import ABC, abstractmethod  <-- import modułów abstrakcyjnych

class Pracownicy_stajni(ABC):
    def __init__(self, name):
        self.name = name

    def print_name(self):
        print(f'Imie - {self.name}')
    @abstractmethod                  <-- dekorator abstrakcyjny
    def wynagrodzenie(self):         <-- METODA ABSTRAKCYJNA z sygnaturą
        pass                         <-- / brak zdefiniowanego CIAŁA \
		...                          <-- \ PLACEHOLDER (tymczasowo)  /

class Stajenny(Pracownicy_stajni):
    def __init__(self, name, pensja_bazowa):
        super().__init__(name)
        self.pensja_bazowa = pensja_bazowa

    def wynagrodzenie(self):        <-- implementaca METODY ABSTRAKCYJNEJ
        return self.pensja_bazowa



class Instruktor(Pracownicy_stajni):
    _bonus = 1000

    def __init__(self, name, pensja_bazowa):
        super().__init__(name)
        self.pensja_bazowa = pensja_bazowa

    def wynagrodzenie(self):        <-- implementaca METODY ABSTRAKCYJNEJ
        return self.pensja_bazowa + self._bonus

stajenny1 = Stajenny('Kaziu', 3500)
instruktor1 = Instruktor('Kasia', 3500)
print(f'Stajenny {stajenny1.name} zarabia {stajenny1.pensja_bazowa}')
instruktor1.print_name()
print(instruktor1.wynagrodzenie())
```

W Pythonie te `...` (trzy kropki) to obiekt **ELLIPSIS**. Ma on parę zastosowań, ale w tym kontekście to po prostu „PLACEHOLDER” – coś wstawionego tymczasowo, żeby kod się kompilował, ale nic nie robił. Podobnie do PASS

