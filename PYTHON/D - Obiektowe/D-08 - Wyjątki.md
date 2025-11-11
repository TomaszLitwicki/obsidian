Dziedziczy się po KLASIE ==Exception== 
```
class Moje_Errory(Exception):
    pass

raise Moje_Errory
```

```
class InvalidAgeError(Exception):   #<-- dziedziczenie po klasie EXCEPTION
    pass                            #<-- zazwyczaj nic się tu nie dodaje, ale można.

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
            raise InvalidAgeError(f'Wiek nie może być ujemny! Podałeś:  {age}') #<-- !!!

user = User('Tomasz', -38)      #<-- prowodyr wywołania wyjątku
print(f'Użytkownik {user.name} ma {user.age} lat.')

```

Klasę własnego wyjątku też można sparametryzować!
```
class InvalidAgeError(Exception):   #<-- dziedziczenie po klasie EXCEPTION
    def __init__(self, my_message, my_error_code):
        super().__init__(my_message)#<-- Zmienna na wiadomość
        self.error = my_error_code  #<-- Nadanie numeru errora

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
            raise InvalidAgeError('Wiek nie może być ujemny!', 102) #<-- !!!

user = User('Tomasz', -38)      #<-- prowodyr wywołania wyjątku
print(f'Użytkownik {user.name} ma {user.age} lat.')

```

*raise InvalidAgeError('Wiek nie może być ujemny!', 102) #<-- !!!
  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
InvalidAgeError: Wiek nie może być ujemny!*

