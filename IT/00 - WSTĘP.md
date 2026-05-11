## 0. SDLC (Software Development Life Cycle)
**Opis:** Cykl życia oprogramowania to ustrukturyzowany proces tworzenia aplikacji, który dzieli pracę na logiczne etapy. To "wielki obraz" (Big Picture), który pozwala zrozumieć, kiedy i dlaczego używamy konkretnych technologii.
- [ ] **Status:** Do zrozumienia (kontekst)
- **Kluczowe etapy:**
    - `Planowanie i Analiza`: Co budujemy i po co?
    - `Projektowanie (Design)`: Tu decydujesz o **Mikroserwisach**.
    - `Implementacja`: Twoja praca w PyCharmie.
    - `Testowanie i Wdrożenie`: Tu wchodzi **CI/CD** oraz **Docker**.
    - `Utrzymanie i Operacje`: Tu królują **Kubernetes** i **OpenShift**.
---
## 1. Zasady SOLID (Clean Code)
**Opis:** Pięć podstawowych zasad projektowania obiektowego, które sprawiają, że kod jest łatwiejszy do zrozumienia, rozszerzania i utrzymania.
- [ ] **Status:** Do opanowania (teoria + praktyka)
- **Kluczowe zasady:**
    1. **S (Single Responsibility)**: Jedna klasa/funkcja powinna mieć tylko jedną odpowiedzialność (robić jedną rzecz dobrze).
    2. **O (Open/Closed)**: Kod powinien być otwarty na rozszerzenia, ale zamknięty na modyfikacje (dodajesz nowe funkcje bez psucia starych).
    3. **L (Liskov Substitution)**: Klasy pochodne muszą móc zastępować swoje klasy bazowe bez psucia aplikacji. Klasa potomna musi dziedziczyć wszystkie metody klasy bazowej
    4. **I (Interface Segregation)**: Lepiej mieć wiele małych, dedykowanych interfejsów(klasy abstrakcyjne w Python) niż jeden "do wszystkiego". 
    5. **D (Dependency Inversion)**: Polegaj na abstrakcjach, a nie na konkretnych implementacjach (kluczowe przy testowaniu i Docker'ze!). Wysokopoziomowe moduły nie powinny zależeć od modułów niskopoziomowych (i wzajmnie), a zależności między nimi powinny wynikać z abstrakcji.

> [!tip] Dlaczego SOLID jest ważny w kontekście mikroserwisów? Jeśli Twój kod nie spełnia zasady **S (Single Responsibility)**, to próba podzielenia go na osobne kontenery (mikroserwisy) będzie koszmarem. Dobry kod to łatwa konteneryzacja.

---
## 2. Docker (Fundament)
**Opis:** Konteneryzacja pozwalająca na izolację aplikacji.
- [ ] **Status:**  Do nauki
- **Kluczowe pojęcia:**
    - `Dockerfile`: Instrukcja budowania obrazu.
    - `Image`: Zamrożona paczka z aplikacją.
    - `Container`: Uruchomiona instancja obrazu.
    - `Docker Compose`: Orkiestracja lokalna wielu kontenerów.
---
## 3. Mikroserwisy (Architektura)
**Opis:** Podejście projektowe dzielące system na małe moduły.
- [ ] **Status:**  Do nauki
- **Kluczowe pojęcia:**
    - Komunikacja synchroniczna (REST) i asynchroniczna (RabbitMQ/Kafka).
    - Database per Service (każdy serwis ma własną bazę).
    - API Gateway (punkt wejścia do systemu).
---
## 4. CI/CD (Automatyzacja)
**Opis:** Automatyzacja testów i wdrożeń.
- [ ] **Status:** Do nauki
- **Kluczowe pojęcia:**
    - `Pipeline`: Ciąg kroków od kodu do serwera.
    - `Continuous Integration`: Częste łączenie kodu i testowanie.
    - `Continuous Deployment`: Automatyczny release na produkcję.
- **Narzędzia:** GitHub Actions, GitLab CI, Jenkins.
---
## 5. Kubernetes (K8s - Orkiestracja)
**Opis:** System zarządzania kontenerami na dużą skalę.
- [ ] **Status:** Do nauki
- **Kluczowe pojęcia:**
    - `Pod`: Najmniejsza jednostka w K8s.
    - `Deployment`: Definicja pożądanej liczby kopii aplikacji.
    - `Service`: Stały adres IP dla grupy Podów.
    - `Helm`: Menedżer pakietów dla Kubernetesa.
- **Narzędzia:** Minikube, Kind, kubectl.
---
## 6. OpenShift (Enterprise K8s)
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
