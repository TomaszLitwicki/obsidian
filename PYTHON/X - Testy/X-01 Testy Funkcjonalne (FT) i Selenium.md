Testy funkcjonalne (często nazywane _End-to-End_ lub _Acceptance Tests_) symulują zachowanie prawdziwego użytkownika w przeglądarce. W kursie wykorzystujemy do tego bibliotekę **Selenium** owiniętą w standardowy moduł Pythona `unittest`.

## Standardowa struktura pliku `functional_tests.py`

```python
from selenium import webdriver
from selenium.webdriver.common.by import By # (Nowszy standard dla Selenium 4+)
import unittest

class NewVisitorTest(unittest.TestCase):

    # Uruchamia się PRZED każdym testem (np. otwarcie przeglądarki)
    def setUp(self):
        self.browser = webdriver.Firefox()

    # Uruchamia się PO każdym teście (np. zamknięcie przeglądarki, nawet jeśli test nie przeszedł)
    def tearDown(self):
        self.browser.quit()

    # Każda metoda zaczynająca się od "test_" jest traktowana jako test
    def test_can_start_a_list_and_retrieve_it_later(self):
        # Symulacja wpisania adresu URL
        self.browser.get('http://localhost:8000')

        # Asert - sprawdzenie tytułu strony
        self.assertIn('To-Do', self.browser.title)
        
        # Wymuszenie błędu (przydatne jako przypomnienie "tu skończyłem pisać test")
        self.fail('Finish the test!')

if __name__ == '__main__':
    # Uruchomienie testów z poziomu CLI (warnings='ignore' ukrywa niepotrzebne ostrzeżenia)
    unittest.main(warnings='ignore')
```

## Polecenia i asercje (Selenium + unittest)

### Nawigacja przeglądarką
- `self.browser = webdriver.Firefox()` - inicjuje sterownik przeglądarki (wymaga zainstalowanego geckodriver).
- `self.browser.get('http://adres.pl')` - przechodzi pod wskazany adres URL.
- `self.browser.quit()` - zamyka okno przeglądarki i kończy sesję sterownika.

### Pobieranie elementów i wartości
- `self.browser.title` - zwraca tytuł aktualnej strony (zawartość tagu `<title>`).
- _Znajdowanie elementów (według współczesnego standardu)_:
    - `header_text = self.browser.find_element(By.TAG_NAME, 'h1').text` - znajduje pierwszy tag `<h1>` i pobiera z niego tekst.

### Podstawowe asercje testowe (`unittest`)
- `self.assertIn(a, b)` - sprawdza czy element `a` znajduje się w `b`. Np. `'To-Do' in browser.title`.
- `self.fail('Wiadomość')` - natychmiast przerywa test z błędem. Idealne do oznaczania "TODO" w procesie TDD.