1. TDD - Test Driven Developer
2. Unit Testing
3. Testy Integracyjne

## TDD (Red-Green-Refactor)
Koncepcja w której przed napisaniem funkcjonalności piszemy test.
1. Nie możesz napisać kodu produkcyjnego, jeżeli nie napisałeś wcześniej nieprzechodzącego testu.
2. Nie możesz napisać większego UNIT TEST'u niż jest to potrzebne, żeby ten test nie przeszedł. Kod, który się nie kompiluje, również oznacza nieprzechodzący test.
3. Nie możesz napisać więcej kodu produkcyjnego niż jest to potrzebne, żeby przeszedł test, który obecnie nie przechodzi.

TDD to proces cykliczny: RED->GREEN->REFACTOR
TEST -> FAIL -> KOD -> TEST OK -> REFAKTORYZACJA KODU -> TEST...

1. **Red (Czerwony)** – Napisz test dla nowej funkcjonalności i uruchom go. Test **musi** nie przejść (zakończyć się błędem), ponieważ funkcjonalność jeszcze nie istnieje.
2. **Green (Zielony)** – Napisz _najprostszą możliwą_ ilość kodu aplikacji, aby test przeszedł pomyślnie. Nie przejmuj się na tym etapie elegancją kodu.
3. **Refactor (Refaktoryzacja)** – Popraw jakość kodu (czytelność, struktura, usuwanie duplikacji) mając pewność, że przechodzące testy chronią Cię przed zepsuciem działania aplikacji.

Asercja to instrukcja w kodzie sprawdzająca, że dany warunek jest prawdziwy.
Każdy test powinien posiadać tylko jedną asercję, czyli tylko 1 powód dla którego może nie przejść.
## Unit Testing
Koncepcja testowania małych elementów w izolacji od całego systemu z perspektywy programisty. Jest sterowany przez testy funkcjonalne.
## Integration Test
Testowanie większej części systemu przekrojowo - Sprawdza **czy współpracujące ze sobą moduły poprawnie się komunikują**.
## Functional Test
Sprawdza **czy system robi to, czego oczekuje użytkownik**.




---

## BDD — Behavior-Driven Development

**BDD** to rozwinięcie TDD, skupione nie na _kodzie_, lecz na **zachowaniu systemu (system behavior)** z perspektywy użytkownika.

Twórca koncepcji: **Dan North**

 Rdzeń idei:
> Zamiast pytać: _czy funkcja działa?_  
> pytamy: **czy system zachowuje się tak, jak oczekuje użytkownik?**

## **DSL — Domain Specific Language**

**DSL** to **język specjalizowany (domain-specific)** stworzony do jednego celu.

Przeciwieństwo:  
**GPL (General Purpose Language)** → Python, Java

## Double-Loop TDD (Podwójna pętla)

W podejściu zaprezentowanym w tutorialu stosujemy dwie współpracujące ze sobą pętle testowe:

1. **Zewnętrzna pętla (Testy Funkcjonalne / FT)**: Testuje aplikację z punktu widzenia użytkownika końcowego. Kiedy FT nie przechodzi, wiemy, jakiej funkcjonalności wysokiego poziomu brakuje.
    
2. **Wewnętrzna pętla (Testy Jednostkowe / UT)**: Kieruje implementacją konkretnych elementów kodu (np. widoków, routingu URL w Django).
    
    - _Proces:_ Piszemy FT -> FT nie przechodzi -> Piszemy UT dla brakującego elementu -> UT nie przechodzi -> Piszemy kod -> UT przechodzi -> Wracamy do FT (aż FT przejdzie w całości).
        

## Ważne zasady z pierwszych rozdziałów

- **Nie testuj stałych (Don't test constants)**: Testy jednostkowe nie powinny polegać na dokładnym sprawdzaniu długich ciągów znaków (np. całego kodu HTML). Są wtedy "kruche" i psują się przy drobnej zmianie w widoku. Lepiej sprawdzić, czy użyto odpowiedniego szablonu (template).
    
- **Zasada małych kroków**: Zawsze staraj się, aby zmiany między błędami w testach (lub między testem a poprawnym kodem) były jak najmniejsze.
    
- **Commitowanie w fazie Green**: Jeśli to możliwe, rób commity w systemie kontroli wersji (Git), gdy testy świecą na zielono.