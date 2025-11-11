1. TDD - Test Driven Developer
2. Unit Testing
3. Testy Integracyjne

## TDD 
Koncepcja w której przed napisaniem funkcjonalności piszemy test.
1. Nie możesz napisać kodu produkcyjnego, jeżeli nie napisałeś wcześniej nieprzechodzącego testu.
2. Nie możesz napisać większego UNIT TEST'u niż jest to potrzebne, żeby ten test nie przeszedł. Kod, który się nie kompiluje, również oznacza nieprzechodzący test.
3. Nie możesz napisać więcej kodu produkcyjnego niż jest to potrzebne, żeby przeszedł test, który obecnie nie przechodzi.

TDD to proces cykliczny: RED->GREEN->REFACTOR
TEST -> FAIL -> KOD -> TEST OK -> REFAKTORYZACJA KODU -> TEST...
Asercja to instrukcja w kodzie sprawdzająca, że dany warunek jest prawdziwy.
Każdy test powinien posiadać tylko jedną asercję, czyli tylko 1 powód dla którego może nie przejść.
## Unit Testing
Koncepcja testowania małych elementów w izolacji od całego systemu
## Testy Integracyjne
Testowanie większej części systemu przekrojowo

