## 1. Wbudowany system autentykacji (django.contrib.auth)

Django posiada wbudowany, wysoce bezpieczny system zarządzania użytkownikami, logowania i autentykacji. Aby z niego skorzystać, nie trzeba pisać własnej logiki sprawdzania haseł, wystarczy podpiąć odpowiednie widoki systemowe do głównego pliku konfiguracyjnego adresów URL.

### Krok 1: Podpięcie ścieżek URL

W głównym pliku projektu należy zaimportować wbudowane ścieżki (tzw. `auth.urls`).

```Python
# projekt/urls.py
from django.urls import include, path

urlpatterns = [
    # Wbudowane widoki autentykacji (login, logout, password_change itp.)
    path('accounts/', include('django.contrib.auth.urls')),
    # ... inne ścieżki ...
]
```

Dzięki temu jednemu wpisowi aplikacja zyskuje automatyczną obsługę endpointów takich jak `/accounts/login/` czy `/accounts/logout/`.

### Krok 2: Konfiguracja przekierowań

Framework musi wiedzieć, dokąd przekierować użytkownika po pomyślnym zalogowaniu lub wylogowaniu. Odpowiednie stałe należy zdefiniować na końcu głównego pliku konfiguracyjnego.


```Python
# projekt/settings.py

# Gdzie przekierować po udanym logowaniu (np. strona główna lub panel)
LOGIN_REDIRECT_URL = '/'

# Gdzie przekierować po wylogowaniu
LOGOUT_REDIRECT_URL = '/'
```

## 2. Logowanie i Wylogowywanie (Szablony)

Widoki wbudowane w Django zawierają kompletną logikę, ale domyślnie nie dostarczają gotowych plików HTML. Należy je utworzyć we wskazanej przez framework strukturze katalogów.

### Szablon Logowania

Domyślnie widok logowania poszukuje szablonu w ścieżce `registration/login.html`.

```HTML
<h2>Zaloguj się</h2>

{% if form.errors %}
  <p class="error">Nazwa użytkownika lub hasło są niepoprawne.</p>
{% endif %}

<form method="post" action="{% url 'login' %}">
    {% csrf_token %}
    
    {{ form.as_p }}
    
    <button type="submit">Zaloguj</button>
</form>
```

Zastosowanie tagu `{% csrf_token %}` jest obligatoryjne dla każdego formularza modyfikującego stan i przesyłanego metodą POST.

### Rozpoznawanie stanu użytkownika w szablonach

Obiekt `user` jest automatycznie wstrzykiwany do kontekstu każdego szablonu. Można wykorzystać go do warunkowego wyświetlania treści. Wylogowanie realizuje się zazwyczaj za pomocą formularza przesyłanego metodą POST, co stanowi standard bezpieczeństwa we współczesnych aplikacjach.

```HTML
<div>
    {% if user.is_authenticated %}
        <p>Zalogowany jako: {{ user.username }}</p>
        <form method="post" action="{% url 'logout' %}">
            {% csrf_token %}
            <button type="submit">Wyloguj</button>
        </form>
    {% else %}
        <p>Witaj, gościu!</p>
        <a href="{% url 'login' %}">Zaloguj się</a>
    {% endif %}
</div>
```

## 3. Rejestracja Użytkowników

W przeciwieństwie do logowania, Django nie dostarcza wbudowanego widoku (endpointu) rejestracji, ponieważ proces ten często zależy od specyficznych wymagań biznesowych. Framework dostarcza jednak gotowy formularz `UserCreationForm`, który automatycznie waliduje dane i bezpiecznie hashuje hasła przed zapisem do bazy.

Do wdrożenia rejestracji zaleca się wykorzystanie widoków generycznych z modułu `django.views.generic`.
### Implementacja widoku rejestracji

```Python
# aplikacja/views.py
from django.urls import reverse_lazy
from django.views import generic
from django.contrib.auth.forms import UserCreationForm

class SignUpView(generic.CreateView):
    # Wskazanie wbudowanego formularza
    form_class = UserCreationForm
    # reverse_lazy opóźnia wywołanie urla, dopóki plik urls.py nie zostanie w pełni załadowany
    success_url = reverse_lazy('login')
    # Wskazanie własnego szablonu
    template_name = 'registration/signup.html'
```

Należy następnie podpiąć ten widok w pliku `urls.py` i stworzyć analogiczny plik HTML z formularzem (`templates/registration/signup.html`).

## 4. Zabezpieczanie Widoków (Restrykcje dostępu)

Często zachodzi potrzeba ograniczenia dostępu do wybranych podstron wyłącznie dla zalogowanych użytkowników. Podejście różni się w zależności od rodzaju zastosowanego widoku.

> [!warning] **Przekierowanie po pomyślnym logowaniu (Parametr `next`)**
> 
> Jeśli niezalogowany użytkownik spróbuje wejść na chronioną stronę, Django zablokuje dostęp i przekieruje go na stronę logowania, doklejając parametr `?next=/chroniona-strona/`. Po poprawnym zalogowaniu system odczyta ten parametr i odeśle użytkownika tam, gdzie pierwotnie próbował wejść.

### A. Widoki Funkcyjne (FBV)

Dla klasycznych funkcji wykorzystuje się dekorator `@login_required`.
```Python
# aplikacja/views.py
from django.contrib.auth.decorators import login_required
from django.shortcuts import render

@login_required
def chroniony_widok(request):
    return render(request, 'tajny_szablon.html')
```

### B. Widoki Generyczne (CBV)

W klasach (np. `ListView`, `DetailView`) stosuje się tzw. domieszkę (mixin) o nazwie `LoginRequiredMixin`.

> [!important] **Kolejność dziedziczenia**
> 
> Zgodnie ze sposobem rozwiązywania metod w Pythonie (MRO), `LoginRequiredMixin` **musi** znajdować się jako pierwszy argument na liście klas bazowych (po lewej stronie).


```Python
# aplikacja/views.py
from django.contrib.auth.mixins import LoginRequiredMixin
from django.views import generic

class ChronionyWidok(LoginRequiredMixin, generic.TemplateView):
    template_name = 'tajny_szablon.html'
```

## 5. Architektura Przepływu Autentykacji

Poniższy diagram obrazuje uproszczony cykl decyzyjny przy żądaniu dostępu do zabezpieczonego widoku:

```
graph TD
    A[Żądanie HTTP] --> B{Czy widok jest chroniony?}
    B -->|Nie| C[Wyrenderowanie widoku]
    B -->|Tak| D{Czy user.is_authenticated?}
    D -->|Tak| C
    D -->|Nie| E[Przekierowanie HTTP 302 na /accounts/login/]
    E --> F[Zalogowanie przez użytkownika]
    F -->|Sukces| C
```