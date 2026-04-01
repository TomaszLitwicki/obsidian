# Dlaczego izolujemy kontenery? (Frontend, Backend, DB)

> [!abstract] Zasada "Jeden kontener = Jeden proces"
> 
> To fundament filozofii Dockera. Zamiast budować jeden wielki "kontener-monolit", dzielimy aplikację na mniejsze, wyspecjalizowane jednostki.

## 🚀 4 Powody, dla których to robimy

### 1. Niezależne Skalowanie (Scalability)

W razie dużego ruchu możesz uruchomić **5 instancji Backendu**, ale zachować tylko **1 instancję Bazy Danych**.

- Gdyby wszystko było w jednym obrazie, musiałbyś kopiować całą bazę danych (marnując RAM/CPU) tylko po to, by obsłużyć więcej zapytań HTTP.

### 2. Czystość Zależności (Dependency Hell)

- **Frontend** potrzebuje Node.js.
- **Backend** potrzebuje Pythona 3.12.
- **Baza** potrzebuje specyficznych bibliotek C++.
    Trzymanie ich osobno gwarantuje, że aktualizacja biblioteki w Backendzie nie zepsuje działania Bazy Danych.

### 3. Bezpieczeństwo (Isolation)

Jeśli haker przejmie kontrolę nad Frontendem (np. przez atak XSS), jest zamknięty w kontenerze frontowym. Nie ma bezpośredniego dostępu do plików bazy danych – musi pokonać warstwę sieciową Dockera, co znacznie utrudnia atak.

### 4. Łatwe zastępowanie (Modularity)

Chcesz zmienić bazę z MySQL na PostgreSQL? Podmieniasz tylko jeden kontener. Reszta aplikacji (Backend/Frontend) nawet "nie wie", że pod spodem zmienił się cały system – widzą tylko ten sam port sieciowy.

---
## 🏗️ Kiedy zacząć tak robić?

> [!important] Odpowiedź: Od samego początku.
> 
> Nawet jeśli Twój projekt to prosta lista "To-Do" robiona w weekend.

**Dlaczego?**
- **Nawyki:** Uczysz się standardów rynkowych (w pracy nikt nie pozwoli Ci na "wszystko w jednym").
- **Przenośność:** Wpisujesz `docker-compose up` i cały stos technologiczny startuje na każdym komputerze tak samo.
---

## 🛠️ Jak to połączyć? (Przykład Docker Compose)

```YAML
version: '3.8'
services:
  frontend:
    build: ./frontend
    ports: ["3000:3000"]

  backend:
    build: ./backend
    ports: ["8000:8000"]
    depends_on:
      - database # Backend czeka, aż baza wstanie

  database:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: secret_password
```
