## 0. SDLC (Software Development Life Cycle)
**Opis:** Cykl życia oprogramowania to ustrukturyzowany proces tworzenia aplikacji, który dzieli pracę na logiczne etapy. To "wielki obraz" (Big Picture), który pozwala zrozumieć, kiedy i dlaczego używamy konkretnych technologii.
- [ ] **Status:** Do zrozumienia (kontekst)
- **Kluczowe etapy:**
    - `Planowanie i Analiza`: Co budujemy i po co?
    - `Projektowanie (Design)`: Tu decydujesz o **Mikroserwisach**.
    - `Implementacja`: Twoja praca w PyCharmie.
    - `Testowanie i Wdrożenie`: Tu wchodzi **CI/CD** oraz **Docker**.
    - `Utrzymanie i Operacje`: Tu królują **Kubernetes** i **OpenShift**.
- **Zadanie:** Zastanów się, w której fazie SDLC znajduje się obecnie Twoja strona PLP na PythonAnywhere.
---
## 00. Zasady SOLID (Clean Code)
**Opis:** Pięć podstawowych zasad projektowania obiektowego, które sprawiają, że kod jest łatwiejszy do zrozumienia, rozszerzania i utrzymania.
- [ ] **Status:** Do opanowania (teoria + praktyka)
- **Kluczowe zasady:**
    1. **S (Single Responsibility)**: Jedna klasa/funkcja powinna mieć tylko jedną odpowiedzialność (robić jedną rzecz dobrze).
    2. **O (Open/Closed)**: Kod powinien być otwarty na rozszerzenia, ale zamknięty na modyfikacje (dodajesz nowe funkcje bez psućia starych).
    3. **L (Liskov Substitution)**: Klasy pochodne muszą móc zastępować swoje klasy bazowe bez psućia aplikacji.
    4. **I (Interface Segregation)**: Lepiej mieć wiele małych, dedykowanych interfejsów niż jeden "do wszystkiego".
    5. **D (Dependency Inversion)**: Polegaj na abstrakcjach, a nie na konkretnych implementacjach (kluczowe przy testowaniu i Dockerze!).
> [!tip] Dlaczego SOLID jest ważny w kontekście mikroserwisów? Jeśli Twój kod nie spełnia zasady **S (Single Responsibility)**, to próba podzielenia go na osobne kontenery (mikroserwisy) będzie koszmarem. Dobry kod to łatwa konteneryzacja.

---
## 1. Docker (Fundament)
**Opis:** Konteneryzacja pozwalająca na izolację aplikacji.
- [ ] **Status:**  Do nauki
- **Kluczowe pojęcia:**
    - `Dockerfile`: Instrukcja budowania obrazu.
    - `Image`: Zamrożona paczka z aplikacją.
    - `Container`: Uruchomiona instancja obrazu.
    - `Docker Compose`: Orkiestracja lokalna wielu kontenerów.
- **Zadanie:** Stwórz Dockerfile dla swojej aplikacji i połącz ją z bazą danych w Docker Compose.
---
## 2. Mikroserwisy (Architektura)
**Opis:** Podejście projektowe dzielące system na małe moduły.
- [ ] **Status:**  Do nauki
- **Kluczowe pojęcia:**
    - Komunikacja synchroniczna (REST) i asynchroniczna (RabbitMQ/Kafka).
    - Database per Service (każdy serwis ma własną bazę).
    - API Gateway (punkt wejścia do systemu).
- **Zadanie:** Zaprojektuj diagram przepływu danych dla systemu e-commerce (koszyk, płatności, magazyn).
---
## 3. CI/CD (Automatyzacja)
**Opis:** Automatyzacja testów i wdrożeń.
- [ ] **Status:** Do nauki
- **Kluczowe pojęcia:**
    - `Pipeline`: Ciąg kroków od kodu do serwera.
    - `Continuous Integration`: Częste łączenie kodu i testowanie.
    - `Continuous Deployment`: Automatyczny release na produkcję.
- **Narzędzia:** GitHub Actions, GitLab CI, Jenkins.
---
## 4. Kubernetes (K8s - Orkiestracja)
**Opis:** System zarządzania kontenerami na dużą skalę.
- [ ] **Status:** Do nauki
- **Kluczowe pojęcia:**
    - `Pod`: Najmniejsza jednostka w K8s.
    - `Deployment`: Definicja pożądanej liczby kopii aplikacji.
    - `Service`: Stały adres IP dla grupy Podów.
    - `Helm`: Menedżer pakietów dla Kubernetesa.
- **Narzędzia:** Minikube, Kind, kubectl.
---
## 5. OpenShift (Enterprise K8s)
**Opis:** Platforma od Red Hat rozszerzająca możliwości Kubernetesa.
- [ ]  **Status:** Do nauki
- **Kluczowe pojęcia:**
    - `Project`: Izolowana przestrzeń (odpowiednik Namespace).
    - `Route`: Wystawienie usługi na zewnątrz (łatwiejszy Ingress).
    - `Source-to-Image (S2I)`: Budowanie obrazu prosto z kodu.

---

## Tabela porównawcza: K8s vs OpenShift

| Cecha | Kubernetes (Vanilla) | OpenShift |
| :--- | :--- | :--- |
| **Producent** | Open Source (CNCF) | Red Hat |
| **Instalacja** | Trudniejsza, duża swoboda | Zintegrowany instalator |
| **Bezpieczeństwo** | Trzeba skonfigurować samemu | Domyślnie bardzo rygorystyczne |
| **Interfejs** | Głównie CLI (kubectl) | Zaawansowana konsola Web |
