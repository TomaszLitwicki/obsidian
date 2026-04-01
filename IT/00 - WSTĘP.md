## 1. Docker (Fundament)
**Opis:** Konteneryzacja pozwalająca na izolację aplikacji.
- [ ] **Status:**  Do nauki
- **Kluczowe pojęcia:**
    - `Dockerfile`: Instrukcja budowania obrazu.
    - `Image`: Zamrożona paczka z aplikacją.
    - `Container`: Uruchomiona instancja obrazu.
    - `Docker Compose`: Orkiestracja lokalna wielu kontenerów.
- **Zadanie:** Stwórz Dockerfile dla swojej aplikacji i połącz ją z bazą danych w Docker Compose.

## 2. Mikroserwisy (Architektura)
**Opis:** Podejście projektowe dzielące system na małe moduły.
- [ ] **Status:**  Do nauki
- **Kluczowe pojęcia:**
    - Komunikacja synchroniczna (REST) i asynchroniczna (RabbitMQ/Kafka).
    - Database per Service (każdy serwis ma własną bazę).
    - API Gateway (punkt wejścia do systemu).
- **Zadanie:** Zaprojektuj diagram przepływu danych dla systemu e-commerce (koszyk, płatności, magazyn).

## 3. CI/CD (Automatyzacja)
**Opis:** Automatyzacja testów i wdrożeń.
- [ ] **Status:** Do nauki
- **Kluczowe pojęcia:**
    - `Pipeline`: Ciąg kroków od kodu do serwera.
    - `Continuous Integration`: Częste łączenie kodu i testowanie.
    - `Continuous Deployment`: Automatyczny release na produkcję.
- **Narzędzia:** GitHub Actions, GitLab CI, Jenkins.

## 4. Kubernetes (K8s - Orkiestracja)
**Opis:** System zarządzania kontenerami na dużą skalę.
- [ ] **Status:** Do nauki
- **Kluczowe pojęcia:**
    - `Pod`: Najmniejsza jednostka w K8s.
    - `Deployment`: Definicja pożądanej liczby kopii aplikacji.
    - `Service`: Stały adres IP dla grupy Podów.
    - `Helm`: Menedżer pakietów dla Kubernetesa.
- **Narzędzia:** Minikube, Kind, kubectl.

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
