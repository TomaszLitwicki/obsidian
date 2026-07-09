**Tagi:** #django #backend #python #testing #tdd #qa

## 1. Wprowadzenie do Testów Automatycznych

Testy automatyczne to procedury sprawdzające poprawność działania kodu. Tworzenie testów (np. w metodyce TDD - _Test-Driven Development_) jest kluczową praktyką w inżynierii oprogramowania.

**Dlaczego należy pisać testy?**
- **Oszczędność czasu:** Automatyzacja zapobiega konieczności ręcznego "przeklikiwania" aplikacji po każdej zmianie.
- **Identyfikacja regresji:** Testy natychmiast informują, czy nowo dodany kod nie uszkodził istniejących i działających wcześniej funkcjonalności.
- **Dokumentacja:** Zbiór testów stanowi formę technicznej specyfikacji, pokazującej jak system powinien zachowywać się w określonych przypadkach brzegowych.
## 2. Testowanie Modeli: Wykrywanie i naprawa błędów

W modelu `Question` zaimplementowano metodę `was_published_recently()`. Posiada ona błąd logiczny: zwraca `True` również dla pytań, których data publikacji (`pub_date`) znajduje się w przyszłości.

Należy utworzyć test weryfikujący to niepożądane zachowanie, a następnie zaktualizować kod modelu.

### Definiowanie testu (`tests.py`)
Zaleca się umieszczanie testów w dedykowanym pliku `tests.py` wewnątrz katalogu aplikacji. Framework automatycznie odnajdzie klasy dziedziczące po `django.test.TestCase`.

```
# polls/tests.py
import datetime
from django.test import TestCase
from django.utils import timezone
from .models import Question

class QuestionModelTests(TestCase):

    def test_was_published_recently_with_future_question(self):
        """
        Metoda was_published_recently() powinna zwracać False dla pytań, 
        których data publikacji znajduje się w przyszłości.
        """
        time = timezone.now() + datetime.timedelta(days=30)
        future_question = Question(pub_date=time)
        
        # Asercja (sprawdzenie założenia): Oczekujemy, że wynik będzie False
        self.assertIs(future_question.was_published_recently(), False)
```

### Poprawa logiki modelu (`models.py`)

Po uruchomieniu testu i jego niepowodzeniu (tzw. faza _Red_), należy zmodyfikować kod produkcyjny, aby test zakończył się sukcesem (faza _Green_).

```
# polls/models.py
def was_published_recently(self):
    now = timezone.now()
    # Pytanie zostało opublikowane niedawno TYLKO jeśli 
    # data publikacji jest starsza niż wczoraj, ale nie nowsza niż "teraz"
    return now - datetime.timedelta(days=1) <= self.pub_date <= now
```

## 3. Uruchamianie Testów (Interfejs CLI)

Do egzekucji testów służy wbudowane polecenie narzędzia `manage.py`.

```
python manage.py test polls
```

**Co wykonuje Django w tle podczas uruchamiania testów?**

1. Odszukuje moduł `polls` i plik `tests.py`.
2. Tworzy **izolowaną, tymczasową bazę danych** przeznaczoną wyłącznie do testów (dzięki czemu testy nie modyfikują danych produkcyjnych/deweloperskich).
3. Odszukuje metody, których nazwy rozpoczynają się od prefiksu `test_` wewnątrz klas dziedziczących po `TestCase`.
4. Wykonuje kod testu i raportuje wyniki na standardowe wyjście (konsola).
5. Niszczy testową bazę danych po zakończeniu procesu.

## 4. Testowanie Widoków (Klient Testowy)

Zaleca się również testowanie warstwy prezentacji (widoków). Służy do tego `Test Client` – narzędzie symulujące żądania przeglądarki internetowej. Dostęp do klienta wewnątrz testów uzyskuje się poprzez atrybut `self.client`.

Należy upewnić się, że widoki generyczne (`IndexView`, `DetailView`) nie wyświetlają pytań zaplanowanych na przyszłość. Wymaga to uprzedniej modyfikacji widoków (np. dodania filtra `pub_date__lte=timezone.now()` w metodzie `get_queryset`).

### Przykładowy test dla widoku głównego:

```
# polls/tests.py (fragment)
from django.urls import reverse

def create_question(question_text, days):
    """
    Funkcja pomocnicza generująca pytania z przesunięciem czasowym (w dniach).
    Wartości ujemne oznaczają przeszłość, dodatnie - przyszłość.
    """
    time = timezone.now() + datetime.timedelta(days=days)
    return Question.objects.create(question_text=question_text, pub_date=time)

class QuestionIndexViewTests(TestCase):
    
    def test_past_question(self):
        """
        Pytania z datą publikacji w przeszłości powinny być 
        wyświetlane na stronie głównej (index).
        """
        question = create_question(question_text="Pytanie z przeszłości.", days=-30)
        
        # Symulacja żądania GET do widoku index
        response = self.client.get(reverse('polls:index'))
        
        # Weryfikacja kodu odpowiedzi (200 OK)
        self.assertEqual(response.status_code, 200)
        
        # Weryfikacja, czy utworzone pytanie znajduje się w kontekście szablonu
        self.assertQuerySetEqual(
            response.context['latest_question_list'],
            [question],
        )

    def test_future_question(self):
        """
        Pytania z datą publikacji w przyszłości NIE powinny być wyświetlane.
        """
        create_question(question_text="Pytanie z przyszłości.", days=30)
        response = self.client.get(reverse('polls:index'))
        
        # Oczekujemy pustej listy pytań
        self.assertQuerySetEqual(response.context['latest_question_list'], [])
```

> [!tip] **Zasada niezależności testów**
> 
> Każda metoda testowa uruchamiana jest na czystej, pustej bazie danych. Oznacza to, że dane utworzone w teście `test_past_question` nie istnieją podczas wykonywania testu `test_future_question`. Gwarantuje to pełną izolację i wiarygodność wyników.

## 5. Przegląd Metod Asercji (Ściągawka)

Klasa `TestCase` w Django dziedziczy po standardowej bibliotece Pythona `unittest`, co daje dostęp do bogatego zestawu metod sprawdzających (asercji), a także rozszerza go o narzędzia specyficzne dla środowiska webowego.

### Asercje wykorzystane w tutorialu

|Metoda|Opis i zastosowanie|
|---|---|
|`assertEqual(a, b)`|Sprawdza, czy wartość `a` jest równa `b`. Najpopularniejsza metoda testująca, użyta m.in. do sprawdzenia kodu HTTP (`response.status_code == 200`).|
|`assertIs(a, b)`|Sprawdza identyczność obiektów (czy `a` jest tym samym obiektem w pamięci co `b`). Użyta w tutorialu do weryfikacji wartości boolowskiej (`False`).|
|`assertQuerySetEqual(qs, list)`|**Specyficzna dla Django.** Porównuje zawartość wynikowego QuerySetu (`qs`) z oczekiwaną listą elementów.|

### Inne kluczowe asercje (Python `unittest`)

|Metoda|Opis i zastosowanie|
|---|---|
|`assertTrue(x)`|Weryfikuje, czy wyrażenie `x` jest ewaluowane jako Prawda (`True`).|
|`assertFalse(x)`|Weryfikuje, czy wyrażenie `x` jest ewaluowane jako Fałsz (`False`).|
|`assertIn(a, b)`|Sprawdza, czy element `a` znajduje się w kolekcji `b` (np. w liście, krotce lub słowniku).|
|`assertRaises(exc, fun, *args)`|Weryfikuje, czy wywołanie funkcji `fun` ze wskazanymi argumentami skutkuje wyrzuceniem określonego wyjątku `exc` (np. `KeyError`).|

### Asercje rozszerzone (Django `TestCase`)

Szczególnie przydatne podczas testowania widoków i odpowiedzi HTTP za pomocą `Test Client`.

|Metoda|Opis i zastosowanie|
|---|---|
|`assertContains(response, text)`|Weryfikuje, czy odpowiedź posiada status `200 OK` i czy jej treść HTML zawiera wskazany fragment tekstu `text`. Idealna do sprawdzania, czy formularz lub błąd wyrenderował się poprawnie.|
|`assertNotContains(response, text)`|Jak wyżej, lecz upewnia się, że dany ciąg znaków **nie** znajduje się w zwróconym dokumencie HTML.|
|`assertRedirects(response, url)`|Sprawdza, czy żądanie zakończyło się przekierowaniem (kod HTTP z rodziny 3xx, zazwyczaj `302`) na wskazany adres `url` docelowy (np. po pomyślnym przesłaniu formularza).|
|`assertTemplateUsed(response, tpl)`|Weryfikuje, czy do wygenerowania odpowiedzi został wykorzystany prawidłowy plik szablonu (np. `'polls/detail.html'`).|