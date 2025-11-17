`pip install flask`
## Podstawowe dekoratory:

| Dekorator                | Po co jest?                                                        |
| ------------------------ | ------------------------------------------------------------------ |
| `@app.route('/path')`    | Mapuje adres URL na funkcję (obsługuje domyślnie metodę GET)       |
| `@app.get('/path')`      | Skrót dla `route` z metodą GET                                     |
| `@app.post('/path')`     | Obsługa formularzy, dodawanie danych (metoda POST)                 |
| `@app.put('/path')`      | Aktualizacja danych (REST)                                         |
| `@app.delete('/path')`   | Usuwanie danych (REST)                                             |
| `@app.before_request`    | Kod uruchamiany przed każdym requestem (np. sprawdzanie logowania) |
| `@app.after_request`     | Kod po wysłaniu odpowiedzi (np. dodanie nagłówków)                 |
| `@app.errorhandler(kod)` | Własna strona błędu, np. 404                                       |

### route
obsługuje tylko metodę GET, więc uruchamia ją domyślnie.
```Python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_word():
    return 'Tu aplikacja Flaska... z trybem debudowania i zmianą portu '

if __name__ == "__main__":
    app.run(debug=True, port=5001)
```

## GET

```Python
@app.route('/users/')
def users():
    return jsonify(all_users)

@app.route('/users/<int:user_id>/')
def user_one(user_id):
    for user in all_users:
        if user.user_id == user_id:
            return jsonify(user)

    return jsonify({'error': 'Użytkownik o takim ID nie istnieje'}), 404
```
## POST
```Python
@app.route('/users', methods=['POST'])
def add_user():
    return 'Dodawanie użytkowników'
```

```Python
@app.route('/users/', methods=['POST'])
def add_user():
    data = request.json
    user = User(len(all_users)+1, data['name'], data['born'])
    all_users.append(user)

    return jsonify(user), 201
```
## PUT
```Python
@app.route('/users', methods=['PUT'])
def update_user():
    return "Aktualizacja całego użytkownika..."
```

```Python
@app.route('/users/<int:user_id>/', methods=['PUT'])
def update_user(user_id):
    data = request.json

    for user in all_users:
        if user.user_id == user_id:
            user.name = data['name']
            user.born = data['born']

            return jsonify(user)

    return jsonify({'error': 'Użytkownik nie został znaleziony'}), 404
```
## PATCH
```Python
@app.route('/users', methods=['PATCH'])
def partially_update_user():
    return "Aktualizacja częściowa użytkownika..."
```

```Python
@app.route('/users/<int:user_id>/', methods=['PATCH'])
def partially_update_user(user_id):
    data = request.json

    for user in all_users:
        if user.user_id == user_id:
            if 'name' in data:
                user.name = data['name']
            if 'born' in data:
                user.born = data['born']

            return jsonify(user)

    return jsonify({'error': 'Użytkownik nie został znaleziony'}), 404
```
## DELETE
```Python
@app.route('/users', methods=['DELETE'])
def delete_user():
    return "Usuwanie użytkownika!"
```

```Python
@app.route('/users/<int:user_id>/', methods=['DELETE'])
def delete_user(user_id):
    global all_users

    user_to_delete = None

    for user in all_users:
        if user.user_id == user_id:
            user_to_delete = user

    if user_to_delete:
        all_users = [u for u in all_users if u.user_id != user_id]
        return '', 204
    else:
        return jsonify({'error': 'Użytkownik nie został znaleziony'}), 404
```

## Refaktoryzacja kodu
```Python
from flask import Flask, jsonify, request
from user import User

user1 = User(1, 'Tori', 2019)
user2 = User(2, 'Figo', 2022)
user3 = User(3, 'Wrangler', 2019)

all_users = [user1, user2, user3]
app = Flask(__name__)

def user_not_found():
    return jsonify({'error': 'Użytkownik nie został znaleziony'}), 404

def find_user_by_id(user_id):
    for user in all_users:
        if user.user_id == user_id:
            return user
    return None
    
@app.route('/')
def hello_word():
    return 'Tu aplikacja Flaska... z trybem debudowania i zmianą portu '

@app.route('/users/')
def users():
    return jsonify(all_users)

@app.route('/users/<int:user_id>/')
def user_one(user_id):
    user = find_user_by_id(user_id)

    if user is None:
        return user_not_found()

    return jsonify(user)

@app.route('/users/', methods=['POST'])
def add_user():
    data = request.json
    user = User(len(all_users)+1, data['name'], data['born'])
    all_users.append(user)

    return jsonify(user), 201

@app.route('/users/<int:user_id>/', methods=['PUT'])
def update_user(user_id):
    data = request.json

    user = find_user_by_id(user_id)
    if user is None:
        return user_not_found()

    user.name = data['name']
    user.born = data['born']

    return jsonify(user)

@app.route('/users/<int:user_id>/', methods=['PATCH'])
def partially_update_user(user_id):
    data = request.json

    user = find_user_by_id(user_id)
    if user is None:
        return user_not_found()

    if 'name' in data:
        user.name = data['name']
    if 'born' in data:
        user.born = data['born']

    return jsonify(user)

@app.route('/users/<int:user_id>/', methods=['DELETE'])
def delete_user(user_id):
    global all_users

    user = find_user_by_id(user_id)
    if user is None:
        return user_not_found()

    all_users = [u for u in all_users if u.user_id != user_id]
    return '', 204

if __name__ == "__main__":
    app.run(debug=True, port=5001)
```