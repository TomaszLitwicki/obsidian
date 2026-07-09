**Tagi:** #django #backend #python #forms #views #generic-views #dry

## 1. Tworzenie Formularza w Szablonie

W celu umożliwienia użytkownikom oddawania głosów, należy utworzyć formularz HTML w szablonie szczegółów pytania (`detail.html`). Formularz ten posłuży do przesłania identyfikatora wybranej odpowiedzi na serwer.

```
<!-- polls/templates/polls/detail.html -->
<form action="{% url 'polls:vote' question.id %}" method="post">
    <!-- Wymagane zabezpieczenie przed atakami CSRF -->
    {% csrf_token %}
    
    <fieldset>
        <legend><h1>{{ question.question_text }}</h1></legend>
        
        <!-- Iteracja po wszystkich odpowiedziach powiązanych z pytaniem (wsteczny menedżer) -->
        {% for choice in question.choice_set.all %}
            <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}">
            <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br>
        {% endfor %}
    </fieldset>
    
    <input type="submit" value="Zagłosuj">
</form>
```

### Kluczowe właściwości formularza:

- **`method="post"`:** Formularz modyfikuje stan danych na serwerze (dodaje rekord głosu do bazy), dlatego **wymagane** jest zastosowanie metody POST, a nie GET.
- **`{% csrf_token %}`:** (Szczegóły w notatce Y-901). Zabezpieczenie obligatoryjne dla każdego formularza przesyłanego metodą POST.
- **`name="choice"` i `value="{{ choice.id }}"`:** W momencie wysłania formularza, przeglądarka przekazuje dane w formacie `choice=3` (gdzie 3 stanowi ID wybranej odpowiedzi).
- **`forloop.counter`:** Specjalna zmienna systemowa dostępna w pętli `{% for %}` wewnątrz szablonów Django. Zwraca numer aktualnej iteracji (1, 2, 3...), co jest tu wykorzystywane do generowania unikalnych atrybutów ID dla znaczników `<input>` oraz `<label>`.

## 2. Odbieranie i Przetwarzanie Danych (Widok `vote`)

Gdy formularz przesyła dane żądaniem POST na adres powiązany z widokiem `vote`, niezbędne jest ich odebranie, weryfikacja, aktualizacja bazy danych oraz zwrócenie odpowiedniej odpowiedzi HTTP.

```
# polls/views.py
from django.http import HttpResponseRedirect
from django.shortcuts import get_object_or_404, render
from django.urls import reverse
from .models import Choice, Question

def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        # Próba pobrania ID wybranej odpowiedzi z danych formularza.
        # Obiekt request.POST funkcjonuje jak słownik i zwraca wartości typu string.
        selected_choice = question.choice_set.get(pk=request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        # Sytuacja, w której użytkownik nie zaznaczył opcji przed wysłaniem formularza
        return render(request, 'polls/detail.html', {
            'question': question,
            'error_message': "Nie wybrano żadnej odpowiedzi.",
        })
    else:
        # Inkrementacja liczby głosów i zapis operacji w bazie danych
        selected_choice.votes += 1
        selected_choice.save()
        
        # Zawsze należy zwracać HttpResponseRedirect po prawidłowym przetworzeniu żądania POST.
        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
```

## 3. Architektura: Wzorzec PRG (Post-Redirect-Get)

Widok `vote` po prawidłowym zapisaniu danych **nie wykorzystuje** funkcji `render()`. Zamiast tego zwraca obiekt `HttpResponseRedirect`. Stanowi to fundamentalną praktykę w programowaniu aplikacji internetowych.

> [!warning] **Znaczenie przekierowania po żądaniu POST**
> 
> Zwrócenie standardowej strony HTML (np. przy użyciu funkcji `render()`) po odebraniu danych POST sprawia, że w przypadku odświeżenia strony przez użytkownika, przeglądarka zaproponuje ponowne wysłanie formularza. Może to skutkować zduplikowaniem operacji w bazie danych (np. dodaniem kolejnego głosu).
> 
> Zastosowanie przekierowania (**Redirect**) wymusza na przeglądarce wykonanie nowego żądania GET pod nowo wskazany adres URL, co czyni proces odświeżania strony całkowicie bezpiecznym.

```
graph TD
    A[Formularz HTML] -->|Żądanie POST| B[Widok: Zapis do bazy]
    B -->|Zwraca: 302 Redirect| C[Przeglądarka]
    C -->|Automatyczne nowe żądanie GET| D[Widok: Strona Wyników]
```

### Funkcja `reverse()`

W powyższym kodzie zastosowano funkcję `reverse('polls:results', args=(question.id,))`. Jej działanie jest analogiczne do tagu `{% url %}` w szablonach HTML – zapobiega ona konieczności tzw. twardego kodowania (_hardcoding_) adresów URL. Na podstawie nazwy zdefiniowanego widoku, funkcja ta generuje i zwraca prawidłową ścieżkę jako ciąg znaków (np. `"/polls/3/results/"`).

## 4. Widoki Generyczne (Class-Based Views)

Standardowe widoki oparte na funkcjach, takie jak `index`, `detail` i `results`, zazwyczaj realizują powtarzalny schemat operacji:

1. Pobranie danych z bazy.
2. Załadowanie wskazanego szablonu.
3. Przekazanie danych do szablonu i jego wyrenderowanie.

Framework Django udostępnia mechanizm **Widoków Generycznych**, który automatyzuje te najczęstsze wzorce projektowe. Pozwala on na konwersję tradycyjnych funkcji na klasy, co znacząco redukuje ilość powtarzalnego kodu (zgodnie z regułą DRY - _Don't Repeat Yourself_).

### Modyfikacja w `urls.py`: Zmiana `question_id` na `pk`

W celu skorzystania z wbudowanych widoków generycznych, w konfiguracji routingu (`urls.py`) należy zmienić nazwę przechwytywanej zmiennej z `<int:question_id>` na `<int:pk>` (gdzie `pk` oznacza _Primary Key_, czyli klucz główny tabeli).

```
# polls/urls.py
from django.urls import path
from . import views

app_name = 'polls'
urlpatterns = [
    path('', views.IndexView.as_view(), name='index'),
    # Zmiana konwertera z question_id na pk
    path('<int:pk>/', views.DetailView.as_view(), name='detail'),
    path('<int:pk>/results/', views.ResultsView.as_view(), name='results'),
    path('<int:question_id>/vote/', views.vote, name='vote'), # Widok vote pozostaje funkcją
]
```

### Implementacja klas w `views.py`

Dotychczasowe widoki funkcyjne (`index`, `detail`, `results`) należy usunąć i zastąpić klasami dziedziczącymi z modułu `generic`:

```
# polls/views.py
from django.views import generic
from .models import Question

class IndexView(generic.ListView):
    # ListView odpowiada za prezentację listy obiektów danego modelu
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list' # Nadpisanie domyślnej nazwy zmiennej przekazywanej do szablonu

    def get_queryset(self):
        """Zwraca 5 ostatnich opublikowanych pytań."""
        return Question.objects.order_by('-pub_date')[:5]

class DetailView(generic.DetailView):
    # DetailView odpowiada za prezentację szczegółów pojedynczego obiektu
    model = Question
    template_name = 'polls/detail.html'

class ResultsView(generic.DetailView):
    # Prezentacja szczegółów tego samego modelu, ale przy użyciu innego szablonu
    model = Question
    template_name = 'polls/results.html'
```

> [!info] **Zasada działania `DetailView`**
> 
> W przypadku klasy `DetailView`, zdefiniowanie atrybutu `model = Question` jest wystarczające. Framework automatycznie odpytuje bazę danych o obiekt powiązany z kluczem głównym przechwyconym z adresu URL (`pk`), a następnie przekazuje go do szablonu pod zmienną o nazwie `question`. Eliminuje to całkowicie konieczność ręcznego wywoływania funkcji pobierających dane (np. `get_object_or_404`).