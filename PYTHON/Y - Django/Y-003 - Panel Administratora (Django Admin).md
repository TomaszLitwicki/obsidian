**Tagi:** #django #backend #python #admin-panel #cms

## 1. Czym jest Django Admin?

Jedną z największych zalet Django jest automatycznie generowany, w pełni funkcjonalny **panel administracyjny (CMS)**.

> [!info] **Filozofia Django Admin**
> 
> Panel administratora nie jest przeznaczony dla końcowych użytkowników Twojej aplikacji. Jest to narzędzie stworzone dla **autorów treści, personelu (staff) oraz administratorów strony**, służące do wygodnego zarządzania danymi w bazie bez konieczności pisania zapytań SQL lub korzystania z shella.

## 2. Krok 1: Tworzenie Superużytkownika (Superuser)

Zanim zalogujesz się do panelu, musisz utworzyć konto o najwyższych uprawnieniach. Służy do tego specjalna komenda CLI:

```
python manage.py createsuperuser
```

Podczas konfiguracji zostaniesz poproszony o podanie:

1. **Nazwy użytkownika** (Username)
    
2. **Adresu e-mail** (Email address)
    
3. **Hasła** (Password) – _hasło podczas wpisywania w terminalu ze względów bezpieczeństwa będzie niewidoczne_.
    

> [!warning] **Ważne przy tworzeniu haseł**
> 
> Jeśli podasz zbyt proste hasło, Django ostrzeże Cię o tym, ale pozwoli Ci je zatwierdzić, jeśli pracujesz w środowisku deweloperskim (wpisując `y`). Na produkcji hasło musi bezwzględnie spełniać wymogi bezpieczeństwa!

## 3. Krok 2: Uruchomienie Serwera i Logowanie

Po pomyślnym utworzeniu konta uruchom serwer deweloperski:

```
python manage.py runserver
```

1. Wejdź w przeglądarce pod adres: `http://127.0.0.1:8000/admin/`
    
2. Zobaczysz domyślny formularz logowania Django.
    
3. Wpisz dane swojego superużytkownika.
    

Po zalogowaniu zobaczysz domyślny panel z dwiema sekcjami: **Groups** (Grupy) oraz **Users** (Użytkownicy). Są to domyślne modele dostarczone przez aplikację systemową `django.contrib.auth`.

## 4. Krok 3: Rejestracja Własnych Modeli (`admin.py`)

Domyślnie Django Admin nie wie o istnieniu Twoich modeli z aplikacji `polls` (takich jak `Question` czy `Choice`). Musisz je jawnie **zarejestrować** w pliku `admin.py` wewnątrz katalogu swojej aplikacji.

```
graph LR
    A[models.py: Klasa Question] -->|Rejestracja| B[admin.py: admin.site.register]
    B -->|Generowanie UI| C[Panel Admina: polls/questions]
```

### Kod rejestracji modelu `Question`:

Otwórz plik `polls/admin.py` i zmodyfikuj go następująco:

```
# polls/admin.py
from django.contrib import admin
# Importujemy nasz model Question
from .models import Question

# Rejestrujemy model w witrynie administracyjnej
admin.site.register(Question)
```

Po zapisaniu tego pliku i odświeżeniu strony `http://127.0.0.1:8000/admin/` w przeglądarce, zobaczysz nową sekcję **POLLS** z modelem **Questions**.

## 5. Jak Django Admin ułatwia Ci życie?

Gdy zarejestrujesz model i wejdziesz w edycję/dodawanie pytań, zobaczysz, jak wiele pracy Django wykonało za Ciebie za kulisami:

- **Automatyczne formularze:** Django sprawdza typy pól w `models.py`. Dla pola `CharField` wygenerowało zwykłe pole tekstowe (`<input type="text">`), a dla pola `DateTimeField` ładny interfejs wyboru daty i godziny wraz ze skrótami "Dzisiaj" i "Teraz".
    
- **Walidacja typów danych:** Jeśli spróbujesz wpisać tekst w pole daty, formularz nie przejdzie walidacji i wyświetli błąd.
    
- **Rola metody `__str__()`:** Lista pytań w panelu wyświetla się jako treść pytania (np. _"Co tam?"_), a nie jako bezużyteczny opis typu `Question object (1)`. To zasługa metody specjalnej `__str__`, którą dodaliśmy do modelu w Części 2.
    
- **Logi historii zmian:** Django automatycznie zapisuje historię modyfikacji każdego obiektu (kto, kiedy i co zmienił).
    

## 6. Szybka Ściągawka

|   |   |   |
|---|---|---|
|**Zadanie**|**Plik / Komenda**|**Co wpisać / zrobić?**|
|Tworzenie konta admina|`Terminal CLI`|`python manage.py createsuperuser`|
|Adres panelu|`Przeglądarka`|`http://127.0.0.1:8000/admin/`|
|Rejestracja modelu|`polls/admin.py`|`admin.site.register(NazwaModelu)`|
|Import modeli w adminie|`polls/admin.py`|`from .models import NazwaModelu`|