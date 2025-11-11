`from dataclasses import dataclass`

Dekorator który ułatwia tworzenie ==klas przechowujących dane==. Nie potrzeba już konstruktora, bo o to zadba już sam dekorator.

W pliku user.py

Czyli z klasycznego tworzenia klasy:
```Python
class User:
    def __init__(self, name, born):
        self.name = name
        self.born = born
```

Przechodzimy do:
```Python
from dataclasses import dataclass

@dataclass()
class User:
    name: str
    born: int
    
```

Sam dekorator dataclass dba o to aby instancję klasy w odpowiedni sposób porównywać. Nie potrzeba metody EQ [[D-09 - Metody Magiczne]] 
Przy zwracaniu odpowiedzi dobrze jest korzystać modułu jsonify:
```Python
from flask import Flask, jsonify

@app.route('/users')
def users():
    return jsonify(all_users)
```
