**Tagi:** #django #backend #python #architecture #web-dev

## 1. Filozofia Architektury: Projekt vs Aplikacja

Django opiera się na modularnej architekturze, w której panuje ścisły rozdział ról. Podstawowym założeniem jest podział na **Projekt** oraz **Aplikacje**.

> [!info] **Projekt (Project)**
> 
> To cała Twoja witryna internetowa lub platforma. Stanowi globalną konfigurację, zbiera ustawienia (`settings.py`), definiuje główny routing (`urls.py`) i łączy wszystkie elementy w całość. Projekt sam w sobie "nie robi" niczego konkretnego – jest kontenerem.

> [!tip] **Aplikacja (App)**
> 
> To samodzielny, przenośny pakiet kodu Pythona, który realizuje jedno konkretne zadanie (np. ankiety `polls`, blog, system komentarzy, koszyk sklepowy). Dobrze zaprojektowaną aplikację można łatwo przenieść z jednego projektu Django do innego.

### Struktura katalogów po utworzeniu aplikacji `polls`:

```
djangotutorial/             <-- Katalog główny projektu (kontener)
├── manage.py               <-- Skrypt CLI do zarządzania projektem
├── djangotutorial/         <-- Właściwy katalog projektu (ustawienia)
│   ├── __init__.py
│   ├── settings.py         <-- Plik konfiguracyjny projektu
│   ├── urls.py             <-- Główny rozdzielnik adresów URL
│   └── wsgi.py / asgi.py
└── polls/                  <-- Katalog naszej aplikacji ankietowej
    ├── __init__.py
    ├── admin.py            <-- Konfiguracja panelu admina dla polls
    ├── apps.py             <-- Metadane konfiguracyjne aplikacji
    ├── migrations/         <-- Historia zmian w bazie danych
    ├── models.py           <-- Definicja bazy danych (modele)
    ├── tests.py            <-- Testy jednostkowe aplikacji
    ├── urls.py             <-- Lokalny routing aplikacji (musisz utworzyć ręcznie!)
    └── views.py            <-- Widoki (funkcje obsługujące żądania i zwracające odpowiedzi)
```

## 2. Ściągawka z komend CLI: Start Projektu

Przed uruchomieniem poniższych komend upewnij się, że masz aktywowane środowisko wirtualne (`.venv`).

| **Komenda**                                  | **Opis**                                                                                                                            |
| -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `django-admin startproject djangotutorial .` | Inicjalizuje nowy projekt o nazwie `djangotutorial` w bieżącym katalogu (kropka na końcu zapobiega tworzeniu zbędnego podkatalogu). |
| `python manage.py startapp polls`            | Tworzy szkielet nowej aplikacji o nazwie `polls`.                                                                                   |
| `python manage.py runserver`                 | Uruchamia lokalny serwer deweloperski pod adresem `http://127.0.0.1:8000/`.                                                         |

## 3. Przepływ Żądania (Request ➔ Response) i Pierwszy Widok

Kiedy użytkownik wpisuje adres w przeglądarce, Django przechodzi przez precyzyjną ścieżkę, aby dostarczyć odpowiedź.

```
graph TD
    A[Przeglądarka użytkownika] -->|1. Żądanie /polls/| B[Główny djangotutorial/urls.py]
    B -->|2. Przekazuje dopasowaną ścieżkę| C[Lokalny polls/urls.py]
    C -->|3. Wskazuje właściwy widok| D[polls/views.py index]
    D -->|4. Tworzy i zwraca| E[HttpResponse]
    E -->|5. Wyświetlenie tekstu| A
```

### Krok A: Tworzenie widoku (`polls/views.py`)

Widok to funkcja (lub klasa), która przyjmuje obiekt żądania sieciowego (`HttpRequest`) i musi zwrócić obiekt odpowiedzi (`HttpResponse`).

```python
# polls/views.py
from django.http import HttpResponse

def index(request):
    # Prosta odpowiedź tekstowa
    return HttpResponse("Witaj w ankietach! To działa.")
```

### Krok B: Tworzenie lokalnego routingu (`polls/urls.py`)

Tworzymy plik `urls.py` wewnątrz katalogu aplikacji, aby powiązać adres URL z funkcją z pliku `views.py`.

```python
# polls/urls.py
from django.urls import path
from . import views

urlpatterns = [
    # path(route, view, name)
    path('', views.index, name='index'), 
]
```
`path(route, view, kwargs=None, name=None)`
*  `route` (wzorzec adresu URL) – _wymagany_
	To ciąg znaków, który określa, jakiego adresu w przeglądarce Django ma szukać.
	- `''` (pusty string), oznacza to stronę główną danej aplikacji (np. `/polls/`).
	-  `'<int:question_id>/vote/'`, Django zrobi coś genialnego: dopasuje adresy typu `/polls/5/vote/` lub `/polls/12/vote/`. Część `<int:question_id>` to tzw. **konwerter ścieżki**. Mówi on Django: _„Przechwyć tę część adresu, upewnij się, że to liczba całkowita (`int`) i przekaż ją do widoku pod nazwą `question_id`”_.
*  `view` (funkcja widoku) – _wymagany_
	To wskazanie, którą funkcję z pliku `views.py` Django ma wykonać, gdy adres z `route` się zgadza.
	- Np. `views.detail` lub `views.vote`. Ważne: podajesz tu nazwę funkcji **bez nawiasów okrągłych `()`** na końcu. Nie wywołujesz jej sam – dajesz tylko Django namiar na nią, żeby framework wywołał ją w odpowiednim momencie, automatycznie dorzucając obiekt `request` i przechwycone zmienne (np. `question_id`).
-  `kwargs` (dodatkowe argumenty) – _opcjonalny_
	Umożliwia przekazanie dodatkowych danych w postaci słownika (`dict`) bezpośrednio do funkcji widoku. Rzadko używany na początku tutoriala, ale bardzo potężny, gdy chcesz przekazać jakieś stałe konfiguracyjne do widoku.
-  `name` (unikalna nazwa) – _opcjonalny, ale kluczowy_
	To nadanie „ksywki” dla tej konkretnej ścieżki (np. `name='detail'`). Dzięki temu w innych miejscach projektu (w kodzie Pythona lub w szablonach HTML) nie musisz wpisywać adresów na sztywno jako tekst (`/polls/5/`). Zamiast tego używasz bezpiecznej nazwy (funkcji `reverse('detail', args=[5])`). Jeśli w przyszłości postanowisz zmienić adres z `/polls/5/` na `/ankiety/pytanie-5/`, zmienisz to **tylko w jednym miejscu w `path()`**, a cały system linków na stronie nie rozsypie się, bo wciąż będzie szukał ścieżki o nazwie `'detail'`.
	
### Krok C: Podłączenie aplikacji do projektu (`djangotutorial/urls.py`)

Musimy poinformować główny projekt, że ma nasłuchiwać adresów z aplikacji `polls`. Używamy do tego funkcji `include()`.
Aplikacje trzeba zarejestrować w `settings.py` w `INSTALLED_APPS`.
```python
# djangotutorial/urls.py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    # Przekierowuje wszystkie zapytania zaczynające się od 'polls/' do naszej aplikacji
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

> [!warning] **Kiedy używać `include()`?**
> 
> Zawsze używaj `include()`, gdy podłączasz routing innych aplikacji (`polls`, `blog` itp.).
> 
> Jedynym wyjątkiem jest `admin.site.urls`, który jest wbudowany bezpośrednio w Django.

## 4. Co warto zapamiętać?

1. **Modularność:** Zawsze twórz osobną aplikację (`startapp`) dla odrębnych logicznie funkcji projektu.
2. **`urls.py` to drogowskazy:** Główny plik URL odcina początek adresu, a lokalny plik URL w aplikacji analizuje resztę i decyduje, który widok wywołać.
3. **Funkcja `path()` przyjmuje 4 argumenty** (w tym 2 wymagane):
    - `route` (wymagany): wzorzec adresu URL (np. `'polls/'`).
    - `view` (wymagany): funkcja widoku do wywołania (np. `views.index`).
    - `kwargs` (opcjonalny): słownik dodatkowych argumentów przekazywanych do widoku.
    - `name` (opcjonalny): unikalna nazwa wzorca, ułatwiająca generowanie linków w szablonach (np. `name='index'`).