`try:
	instrukcja do wykonania
`except:
	oczekiwany wyjątek (błąd)
`finally:
	instrukcja, która wykona się zawsze
 
`except Exception:` - błąd ogólny
	`pass` - powoduje ukrycie wyjątku (BARDZO ZŁA PRAKTYKA)

`except FileNotFoundError:` - nie znaleziono pliku ^bladotwarciapliku

Rzucanie wyjątku:
`raise Exception()` - rzuca ogólny wyjątek
`raise ValueError('Tekst do wyświetlenia)` - rzuca konkretny wyjątek z opisem.

```
class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @property
    def age(self):
         return self._age

    @age.setter
    def age(self,age):
        if age >= 0:
            self._age = age
        else:
            raise Exception('Wiek nie może być ujemny!')

user = User('Tomasz', -38)
print(f'Użytkownik {user.name} ma {user.age} lat.')

```