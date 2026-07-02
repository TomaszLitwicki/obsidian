Testy jednostkowe sprawdzają zachowanie małych, wyizolowanych fragmentów kodu (np. logikę widoków w Django) z punktu widzenia programisty (od wewnątrz). Zapisujemy je zazwyczaj w pliku `tests.py` danej aplikacji Django (np. `lists/tests.py`).

## Standardowa struktura `tests.py`

Do testowania w Django używamy klasy `TestCase` dostarczanej przez samo Django, która dziedziczy po standardowym `unittest.TestCase`.

```python
from django.test import TestCase
from django.urls import resolve
from django.http import HttpRequest
from lists.views import home_page

class HomePageTest(TestCase):

    # 1. Testowanie rozwiązywania adresów URL (Routing)
    def test_root_url_resolves_to_home_page_view(self):
        found = resolve('/')
        self.assertEqual(found.func, home_page)

    # 2. Testowanie widoku i renderowania szablonu HTML
    def test_home_page_returns_correct_html(self):
        # Utworzenie obiektu żądania (mock request)
        request = HttpRequest()
        
        # Wywołanie funkcji widoku (wykonuje logikę i zwraca response)
        response = home_page(request)
        
        # Dekodowanie bajtów odpowiedzi do stringa HTML
        html = response.content.decode('utf8')
        
        # Asercje weryfikujące poprawność HTML
        self.assertTrue(html.startswith('<html>'))
        self.assertIn('<title>To-Do lists</title>', html)
        self.assertTrue(html.endswith('</html>'))
```

## Polecenia i asercje (Django Unit Tests)

### Testowanie URL-i

- `resolve('/sciezka/')` - wewnętrzna funkcja Django (z `django.urls`), która sprawdza, do jakiego widoku mapuje dana ścieżka zdefiniowana w `urls.py`. Odnosi się do niej właściwość `.func` (np. `found.func`).

### Testowanie widoków i obiektów HttpRequest

- `HttpRequest()` - klasa z `django.http`, która pozwala wygenerować atrapę zapytania HTTP. Jest to konieczne, ponieważ funkcje widoków w Django (np. `def home_page(request):`) zawsze wymagają jako pierwszego argumentu obiektu żądania.
- `response.content` - przechowuje to, co serwer odsyła do przeglądarki, jednak dane są w formacie **bajtowym** (byte string, np. `b'<html>'`).
- `response.content.decode('utf8')` - dekoduje bajty na zwykłego pythonowego stringa, co znacznie ułatwia asercje (sprawdzanie stringów zamiast surowych bajtów).

### Użycie klienta testowego Django (Django Test Client)
 `self.client` automatyzuje proces tworzenia zapytań HTTP i rozwiązuje problem "kruchych testów" (testowania na sztywno stringów HTML).

```python
    def test_uses_home_page_template(self):
        # self.client.get() symuluje wysłanie zapytania GET pod wskazany adres
        response = self.client.get('/')
        
        # assertTemplateUsed sprawdza, czy widok użył konkretnego szablonu
        self.assertTemplateUsed(response, "home.html")
```

- `self.client.get('/sciezka/')` - wewnętrzny klient testowy pod maską wykonuje pełny cykl Django (od `urls.py` po `views.py`) bez konieczności ręcznego tworzenia `HttpRequest()` czy rozwiązywania URL-i za pomocą `resolve()`.
    
- `self.assertTemplateUsed(response, 'nazwa_szablonu.html')` - bardzo przydatna asercja dostarczana przez klasę `TestCase`. Pozwala sprawdzić pożądaną intencję ("czy wyrenderowano stronę tym szablonem?"), zamiast badać dokładnie wygenerowany kod HTML (co psułoby test przy każdej najmniejszej zmianie w pliku `.html`).


---
### Asercje testowe

**1. Standardowe asercje bazowe z Pythona (`unittest`)**

|Metoda|Sprawdza|
|---|---|
|`self.assertTrue(x)`|czy wyrażenie jest True|
|`self.assertFalse(x)`|czy jest False|
|`self.assertIn(a, b)`|czy `a` znajduje się w `b`|
|`self.assertIsNone(x)`|czy `x is None`|
|`self.assertGreater(a, b)`|czy `a > b`|
|`self.assertEqual(a, b)`|czy a == b|

**2. Asercje specjalne od klienta testowego Django (`django.test.TestCase`)**

|Metoda (Asercja)|Sprawdza (w kontekście odpowiedzi HTTP)|
|---|---|
|`self.assertTemplateUsed(response, 'home.html')`|Czy serwer użył danego pliku szablonu do wyrenderowania końcowego dokumentu HTML. Eliminuje badanie kodu linijka po linijce.|
|`self.assertContains(response, 'tekst')`|Czy podany fragment tekstu występuje w zwróconym przez serwer dokumencie. Pełni rolę inteligentniejszego `assertIn` (sam dekoduje z bajtów i sprawdza status HTTP!).|
|`self.assertNotContains(response, 'tekst')`|Odwrotność powyższego - czy niechciany komunikat/element na pewno nie wyświetlił się na stronie po operacji.|
|`self.assertRedirects(response, '/url/')`|Czy po wykonaniu zapytania (najczęściej `POST`) klient został poprawnie przekierowany pod nowy, wyznaczony adres.|
