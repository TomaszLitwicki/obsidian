Serializowanie to zamiana obiektu (np. słownika, listy, klasy Pythona) na ciąg znaków lub bajtów, który można zapisać do pliku, wysłać przez sieć albo trzymać w bazie danych.
Po co? Bo obiekt w Pythonie istnieje tylko w pamięci RAM. Jeśli chcesz go utrwalić lub przesłać, musisz zmienić go w formę, która może „podróżować” lub być zapisana.

Przykłady formatów serializacji:

* JSON – najczęstszy w API i aplikacjach webowych
* Pickle – natywny dla Pythona, zapisuje pełny obiekt (nie tylko dane)
* XML – kiedyś popularny, dziś mniej

Prosty przykład w Pythonie – serializacja do JSON:

```python
import json

person = {
    "name": "Tomasz",
    "age": 35,
    "skills": ["Python", "Flask"]
}

json_data = json.dumps(person)
print(json_data)
```

Wynik:

```
{"name": "Tomasz", "age": 35, "skills": ["Python", "Flask"]}
```

To już jest tekst, który możesz zapisać do pliku albo wysłać przez API.

Deserializacja – proces odwrotny, czyli zamiana JSON z powrotem na obiekt:

```python
obj = json.loads(json_data)
print(obj["name"])
```

W kontekście backendu:

* klient (np. przeglądarka) wysyła JSON
* backend (Flask) deserializuje go do pythona
* backend odsyła JSON – znowu serializacja

Krótko: serializacja to pakowanie obiektu do formatu możliwego do przechowywania lub transportu. Deserializacja to rozpakowanie.

To ściśle przydaje się w API i bazach. W Pythonie użyjesz tego non stop.

## JSON PICKLE
`pip install jsonpickle`

przy standardowym tworzeniu klas:
```Python
class User:
    def __init__(self, user_id, name, born):
        self.user_id = user_id
        self.name = name
        self.born = born
```

```Python
from flask import Flask, request, Response
from user import User
import jsonpickle

@app.route('/users/')
def users():
    gender = request.args.get('gender')

    if gender == 'male':
        return jsonify([u for u in all_users if not u.name.endswith('a')])
    elif gender == 'female':
        return jsonify([u for u in all_users if u.name.endswith('a')])

    all_users_json = jsonpickle.encode(all_users, unpicklable=False)
    return Response(response=all_users_json, status=200, mimetype='application/json')
```

---
## JSONIFY vs JSONPICKLE
JSONIFY i JSONPICKLE robią podobną rzecz (serializują obiekty), ale działają na zupełnie innym poziomie i mają inne zastosowanie.

---
### `jsonify` (Flask)

To funkcja Flaska, która:

* zamienia **proste dane** (słowniki, listy, liczby, stringi) na JSON
* ustawia nagłówek `Content-Type: application/json`
* oddaje poprawną odpowiedź HTTP

Używasz tego tak:
```python
from flask import jsonify

@app.route('/user')
def user():
    return jsonify({"name": "Tomasz", "age": 35})
```

Zalety:
* idealne dla API
* szybkie
* bezpieczne
* zgodne z przeglądarkami i klientami HTTP
* nie zasypuje klienta dziwnymi polami

Wady:
* **nie serializuje skomplikowanych obiektów** (np. własnych klas, obiektów datetime, modeli ORM)
* musisz sam zamienić złożone obiekty na prostsze struktury (np. `.to_dict()`)

---
### `jsonpickle`

To osobna biblioteka, która potrafi serializować prawie każdy obiekt Pythona:
* klasy
* obiekty z metodami
* datetimes
* customowe struktury
* zagnieżdżone, trudne struktury danych

Przykład:
```python
import jsonpickle

class Horse:
    def __init__(self, name, age):
        self.name = name
        self.age = age

h = Horse("Namira", 8)
serialized = jsonpickle.encode(h)
print(serialized)
```

Wypluje coś takiego:
```json
{"py/object": "__main__.Horse", "name": "Namira", "age": 8}
```

Zalety:
* serializuje prawie wszystko, nawet złożone, własne klasy
* może później odtworzyć obiekt (`jsonpickle.decode`)

Wady:
* wynik **nie jest klasycznym JSON-em**, tylko JSON z sygnaturami Pythona (`py/object`), więc:
	  * frontend (JavaScript) zwykle nie chce czegoś takiego
	  * API staje się „Python-only”
* większy, mniej czytelny
* potencjalnie **niebezpieczny przy deserializacji** (możliwe wykonanie niechcianego kodu jeśli wczytujesz dane od obcych)

---
### Kiedy których używać?

`jsonify`:
* zawsze w API
* gdy odpowiedź ma być czytelna i kompatybilna z innymi językami
* gdy frontend lub mobilna aplikacja oczekuje JSON-a

`jsonpickle`:
* do zapisywania stanu obiektów między sesjami
* lokalne logi, cache danych
* komunikacja między Python ↔ Python
* ale **nie do publicznego API**

---
### Krótka, konkretna różnica:

* `jsonify` = czysty JSON, idealny do API, tylko proste typy
* `jsonpickle` = serializacja dowolnych obiektów Pythona, ale format "Pythonowy", cięższy, nie dla frontendu

---
Jeśli robisz aplikację webową → **używaj `jsonify`**
Jeśli chcesz zapisać stan obiektu Pythona na dysk → **jsonpickle** jest ok.

Takie podejście zabezpiecza API, jest szybkie i czytelne dla klientów.

---
## JSONIFY a DATACLASS

`jsonify` **nie potrzebuje** zawsze `dataclass` , tylko do obiektów własnej klasy.
`jsonify` przyjmuje wszystko, co można bezpośrednio zamienić na standardowy JSON:

* słowniki
* listy
* stringi
* liczby
* wartości `True/False`
* `None`

Przykłady działają bez żadnych dataclass:

```python
from flask import jsonify

@app.route('/test')
def test():
    return jsonify({"status": "ok", "value": 42})
```

Albo:
```python
return jsonify([1, 2, 3, 4])
```

---
### Skąd to zamieszanie z dataclass?

Jeśli masz **własny obiekt klasy**, np.:
```python
class Horse:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

I spróbujesz zrobić:
```python
h = Horse("Namira", 8)
return jsonify(h)
```

To nie zadziała. Flask nie wie, jak zamienić obiekt klasy na JSON.

Wtedy potrzebujesz **zamienić obiekt na słownik**:

* ręcznie
* przez metodę `.to_dict()`
* albo użyć `@dataclass`, bo dataclass daje gotowe `__dict__`

Przykład z dataclass:
```python
from dataclasses import dataclass
from flask import jsonify

@dataclass
class Horse:
    name: str
    age: int

@app.route('/horse')
def horse():
    h = Horse("Namira", 8)
    return jsonify(h.__dict__)
```

Ale da się też bez dataclass:
```python
class Horse:
    def __init__(self, name, age):
        self.name = name
        self.age = age

@app.route('/horse')
def horse():
    h = Horse("Namira", 8)
    return jsonify({"name": h.name, "age": h.age})
```

---
### Podsumowanie:

* `jsonify` **nie wymaga** `dataclass`
* `jsonify` wymaga **prostych struktur danych**
* obiekty klas muszą być „przekonwertowane” na słowniki lub listy

Dataclass tylko ułatwia życie, ale nie jest obowiązkowa.
