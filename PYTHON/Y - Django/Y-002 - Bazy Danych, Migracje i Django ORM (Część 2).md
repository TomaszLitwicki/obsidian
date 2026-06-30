**Tagi:** #django #backend #python #database #orm #migrations

## 1. Konfiguracja Bazy Danych w Django

Po zainicjalizowaniu projektu w pliku `settings.py` znajduje się domyślna konfiguracja bazy danych.

- **Domyślna baza:** Django po instalacji konfiguruje **SQLite** (`db.sqlite3`). To plikowa baza danych – idealna do developmentu, ponieważ nie wymaga instalacji dodatkowego oprogramowania ani serwerów bazy.
    
- **Aplikacje systemowe (`INSTALLED_APPS`):** Django domyślnie rejestruje zestaw aplikacji (np. `django.contrib.admin`, `django.contrib.auth`, `django.contrib.contenttypes`), które do swojego działania wymagają własnych tabel w bazie danych.
    

## 2. Projektowanie Modeli (`models.py`)

Modele w Django to klasy Pythona reprezentujące tabele w bazie danych. Każdy atrybut klasy odpowiada kolumnie w tabeli.

### Twoje modele z aplikacji `polls`:

```
# polls/models.py
import datetime
from django.db import models
from django.utils import timezone

class Question(models.Model):
    # Kolumna tekstowa (CharField) o maksymalnej długości 200 znaków
    question_text = models.CharField(max_length=200)
    # Kolumna przechowująca datę i czas (DateTimeField) z czytelną etykietą
    pub_date = models.DateTimeField("date published")

    # Reprezentacja tekstowa obiektu (bardzo ważna dla konsoli i panelu Admina)
    def __str__(self):
        return self.question_text

    # Własna metoda pomocnicza
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)

class Choice(models.Model):
    # Relacja jeden-do-wielu (Klucz Obcy). Każda odpowiedź należy do jednego pytania.
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    # Kolumna dla liczb całkowitych z domyślną wartością 0
    votes = models.IntegerField(default=0)

    def __str__(self):
        return self.choice_text
```

### Kluczowe elementy modeli:

- `models.ForeignKey(Question, on_delete=models.CASCADE)`: Powiązanie tabeli `Choice` z tabelą `Question`. Kaskadowe usuwanie (`CASCADE`) sprawia, że usunięcie pytania automatycznie usuwa wszystkie przypisane do niego odpowiedzi.
    
- `__str__()`: Domyślnie Django wyświetla obiekty w konsoli jako `<Question: Question object (1)>`. Zdefiniowanie `__str__` sprawia, że Django użyje np. treści pytania: `<Question: Co tam?>`.
    

## 3. Trzykrokowy Cykl Życia Migracji

Migracje w Django to pliki, które tłumaczą zmiany w kodzie Pythona (`models.py`) na polecenia SQL modyfikujące bazę danych.

```
graph TD
    A[Krok 1: Edycja models.py] -->|python manage.py makemigrations| B[Krok 2: Generowanie pliku migracji]
    B -->|python manage.py migrate| C[Krok 3: Wykonanie zmian w bazie SQLite]
```

### Tabela przydatnych poleceń migracji:

|   |   |   |
|---|---|---|
|**Komenda**|**Co robi?**|**Kiedy używać?**|
|`python manage.py makemigrations`|Generuje plik migracji w folderze `migrations/` na podstawie zmian w `models.py`.|Zawsze po zmianie lub dodaniu modeli.|
|`python manage.py makemigrations --dry-run`|Pokazuje, jakie migracje zostałyby utworzone, bez fizycznego zapisu plików.|Aby sprawdzić, co Django chce zrobić.|
|`python manage.py makemigrations --check`|Sprawdza, czy są jakieś zmiany w modelach, które nie zostały zapisane do migracji.|Przydatne w systemach CI/CD lub przed commitem.|
|`python manage.py sqlmigrate polls 0001`|Pokazuje czysty kod SQL, jaki Django wygeneruje dla danej migracji.|Do debugowania i dla administratorów baz danych.|
|`python manage.py migrate`|Wykonuje wszystkie zaległe migracje w bazie danych.|Po pobraniu nowego kodu lub po `makemigrations`.|
|`python manage.py showmigrations`|Wyświetla status wszystkich migracji w projekcie.|Aby sprawdzić, co już zostało wgrane do bazy.|

## 4. Django ORM API (Konsola / Shell)

Wpisując w terminalu `python manage.py shell`, uruchamiasz powłokę Pythona ze skonfigurowanym środowiskiem Django.

### Podstawowy zestaw zapytań CRUD:

```
from polls.models import Question, Choice
from django.utils import timezone

# --- CREATE (Tworzenie) ---
# Sposób A:
q = Question(question_text="Co na obiad?", pub_date=timezone.now())
q.save() # Wymaga jawnego zapisu

# Sposób B:
Question.objects.create(question_text="Gdzie jedziemy?", pub_date=timezone.now()) # Zapisuje automatycznie

# --- READ (Odczyt) ---
Question.objects.all()                      # Zwraca QuerySet wszystkich rekordów
Question.objects.filter(id=1)               # Filtrowanie (zwraca QuerySet)
Question.objects.get(pk=1)                  # Pobranie dokładnie jednego obiektu (pk = Primary Key)

# --- UPDATE (Aktualizacja) ---
q = Question.objects.get(pk=1)
q.question_text = "Zmieniony tekst pytania"
q.save()

# --- DELETE (Usuwanie) ---
q = Question.objects.get(pk=1)
q.delete()
```

## 5. Relacje Wsteczne i `choice_set`

Ponieważ model `Choice` posiada klucz obcy do `Question`, instancja pytania ma automatyczny dostęp do swoich odpowiedzi poprzez tzw. wstecznego menedżera (reverse manager).

> [!info] **Zasada nazewnictwa wstecznego menedżera**
> 
> Domyślna nazwa tego menedżera to:
> 
>   
> 
> $$\text{nazwa\_powiązanego\_modelu} + \text{\_set}$$
> 
> Dla klasy `Choice` powiązanej z `Question` będzie to: `question_instance.choice_set`

### Wykorzystanie w konsoli:

```
q = Question.objects.get(pk=1)

# Wyciągnięcie wszystkich odpowiedzi przypisanych do tego pytania:
q.choice_set.all() # Zwraca QuerySet, np. <QuerySet []>

# Dodanie nowej odpowiedzi bezpośrednio przez obiekt pytania:
q.choice_set.create(choice_text="Pizza", votes=0)
# Django automatycznie uzupełni klucz obcy (id pytania) w nowej odpowiedzi.
```

## 6. Git, pliki i baza danych – Ważna lekcja

> [!warning] **Dlaczego po `git pull` baza bywa pusta?**
> 
> Plik bazy danych `db.sqlite3` jest dopisany do `.gitignore`. Git przenosi wyłącznie **przepisy** (kod modeli w `models.py` i pliki migracji `0001_initial.py`), ale nie przenosi **gotowych danych** (samego pliku bazy).
> 
> **Rozwiązanie:** Zawsze po zaciągnięciu nowych zmian z repozytorium wykonaj komendę `python manage.py migrate`, aby Twoja lokalna baza zaktualizowała się do najnowszego stanu kodu.