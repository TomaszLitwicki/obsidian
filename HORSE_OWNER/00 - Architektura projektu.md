# Etap 1: Setup i Architektura początkowa Django

Przejście z edukacyjnego MVP na pełnoprawną aplikację z użyciem TDD i zaawansowanej architektury wymagało odpowiedniego przygotowania fundamentów. Poniżej znajduje się podsumowanie wszystkich wdrożonych dotąd kroków konfiguracyjnych.

## 1. Płaska struktura projektu

Zamiast generować domyślne, zagnieżdżone w sobie foldery, projekt został zainicjowany komendą:

`django-admin startproject horse_owner .`

**Dlaczego?**

Użycie kropki (`.`) na końcu sprawiło, że projekt zainicjował się w _obecnym_ katalogu (w głównym folderze repozytorium). Dzięki temu uniknęliśmy podwójnego katalogu `horse_owner/horse_owner/`, co ułatwia późniejsze zarządzanie ścieżkami, Dockerem i Gitem.

## 2. Modułowa architektura ustawień (Settings)

Domyślny plik `settings.py` został usunięty i zastąpiony katalogiem `settings/` z plikiem inicjacyjnym `__init__.py`. Konfiguracja została rozbita na:

- `base.py` – rdzeń ustawień, zasady współdzielone (np. zainstalowane aplikacje).
- `local.py` – środowisko deweloperskie (dziedziczy z `base.py`, ma włączone `DEBUG = True` i podpiętą lokalną bazę).
- `test.py` – środowisko testowe (optymalizacja pod TDD).

**Dlaczego?**

Trzymanie wszystkiego w jednym pliku `settings.py` rodzi ogromne ryzyko (np. przypadkowe wysłanie haseł produkcyjnych na GitHuba lub testowanie kodu na danych produkcyjnych). Modułowość pozwala na elastyczne przełączanie się między środowiskami. Wymagało to również modyfikacji ścieżki w pliku `manage.py` (zmiana na `horse_owner.settings.local`).

## 3. Profesjonalna baza danych (Docker + PostgreSQL)

Domyślna baza SQLite3 w pliku została zastąpiona pełnoprawnym silnikiem PostgreSQL 15 uruchamianym przez Dockera.

- Utworzono plik `docker-compose.yml`, który definiuje kontener z bazą, użytkownika, hasło i mapowanie portów.
- Zainstalowano adapter `psycopg[binary]`, aby Python potrafił rozmawiać z Postgreskiem.
- W pliku `local.py` nadpisano słownik `DATABASES`, by wskazywał na Dockera.

**Dlaczego?**

Środowisko deweloperskie powinno być jak najbardziej zbliżone do produkcyjnego (tzw. _dev/prod parity_). Dzięki temu unikamy sytuacji, w której "u mnie działa", a na serwerze aplikacja "wybucha" przez różnice w dialekcie SQL między SQLite a PostgreSQL.

## 4. Ekstremalna optymalizacja pod TDD (Pytest)

Środowisko testowe zostało uzbrojone w `pytest`, `pytest-django` oraz `pytest-cov`. Utworzono plik `pytest.ini`, który jest punktem wejścia dla testów i wskazuje frameworkowi, by korzystał wyłącznie z konfiguracji `horse_owner.settings.test`.

W pliku `test.py` zastosowano dwa "hacki" przyspieszające TDD:

1. **In-memory DB:** Ustawiono `NAME: ':memory:'` dla bazy SQLite. Baza danych dla testów buduje się wyłącznie w ultraszybkiej pamięci RAM i znika po zakończeniu testów (nie dotykając w ogóle bazy z Dockera).
2. **Szybki Hashing:** Zmieniono `PASSWORD_HASHERS` na prosty algorytm `MD5PasswordHasher`.

**Dlaczego?**

W TDD testy uruchamia się setki razy w ciągu dnia. Domyślny system logowania Django (PBKDF2) celowo spowalnia haszowanie haseł (dla bezpieczeństwa przed atakami). W testach zależy nam wyłącznie na czasie – MD5 przyspiesza tworzenie użytkowników testowych nawet 100-krotnie. Baza w RAM zapobiega z kolei "wąskiemu gardłu", jakim jest zapis na fizycznym dysku.

> [!check] Status
> 
> Projekt jest w 100% gotowy do stworzenia logiki (Custom User Model) zgodnie z cyklem TDD (Red -> Green -> Refactor).