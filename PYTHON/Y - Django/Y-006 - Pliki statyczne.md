**Tagi:** #django #backend #python #static-files #css #frontend

## 1. Czym są pliki statyczne?

Aplikacje webowe to nie tylko generowany przez serwer kod HTML, ale również dodatkowe zasoby niezbędne do prawidłowego wyświetlania strony: arkusze stylów (CSS), skrypty JavaScript i obrazy. W Django zasoby te określa się mianem **plików statycznych (static files)**.

W większych projektach z wieloma aplikacjami zarządzanie nimi bywa problematyczne. Django rozwiązuje to poprzez moduł `django.contrib.staticfiles`, który zbiera pliki ze wszystkich aplikacji do jednej lokalizacji na potrzeby wdrożenia (produkcji).

## 2. Organizacja i Namespacing

Django szuka plików statycznych w katalogu `static` wewnątrz każdej aplikacji (podobnie jak szuka szablonów w katalogu `templates`), korzystając z domyślnego findera `AppDirectoriesFinder`.

> [!warning] **Zasada Namespacingu**
> 
> Nie należy umieszczać plików statycznych bezpośrednio w katalogu `static` (np. `polls/static/style.css`). Jeśli inna aplikacja w projekcie również posiada plik `style.css`, Django wybierze pierwszy napotkany, co doprowadzi do błędu.
> 
> Należy stosować **namespacing**, czyli tworzyć dodatkowy podkatalog z nazwą aplikacji:
> 
> `polls/static/polls/style.css`

Dzięki temu ścieżka do pliku dla frameworka to jednoznaczne `polls/style.css`.

## 3. Dodawanie Arkusza Stylów (CSS)

Aby podpiąć nowo utworzony plik CSS (`polls/static/polls/style.css`) do szablonu HTML, należy użyć specjalnych tagów.

```
<!-- polls/templates/polls/index.html -->

<!-- 1. Załadowanie tagów statycznych na początku pliku -->
{% load static %}

<!-- 2. Użycie tagu {% static %} do wygenerowania poprawnego adresu URL -->
<link rel="stylesheet" href="{% static 'polls/style.css' %}">
```

Tag `{% static %}` generuje bezwzględny adres URL do pliku statycznego, co uniezależnia kod od konfiguracji ścieżek na serwerze.

## 4. Dodawanie Obrazów

Zasady dla obrazów są identyczne. Należy umieścić je w odpowiednim katalogu, np. `polls/static/polls/images/background.png`.

> [!important] **Odwoływanie się z wewnątrz plików CSS**
> 
> Tagi szablonów Django (takie jak `{% static %}`) **nie działają** wewnątrz czystych plików CSS, ponieważ pliki statyczne nie są przetwarzane przez silnik renderowania Django.
> 
> Aby odwołać się do obrazu z poziomu pliku CSS, należy zawsze używać **ścieżek względnych** (relative paths):

```
/* polls/static/polls/style.css */

body {
    /* Ścieżka względna - poprawna praktyka */
    background: white url("images/background.png") no-repeat;
}
```

Stosowanie ścieżek względnych w plikach CSS pozwala na bezproblemową zmianę ustawienia `STATIC_URL` w konfiguracji projektu bez konieczności masowej edycji wszystkich arkuszy stylów.