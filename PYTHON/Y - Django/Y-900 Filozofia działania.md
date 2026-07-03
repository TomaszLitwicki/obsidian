# Django: Dot-lookup i automatyczne wywoływanie metod

**DTL**, czyli **Django Template Language** (Język Szablonów Django)
System szablonów Django celowo upraszcza składnię, aby oddzielić warstwę prezentacji (HTML) od logiki biznesowej (Python). Dwa najważniejsze mechanizmy tego systemu to **uniwersalna kropka** oraz **brak nawiasów przy wywoływaniu metod**.

## 1. Uniwersalna kropka (Dot-lookup)

W czystym Pythonie dostęp do różnych typów danych wymaga innej składni:

- Słownik: `pracownik['imie']`
- Obiekt: `samochod.marka`
- Lista: `zakupy[0]`

W szablonach Django wszystko to zastępuje **jeden zapis z kropką**: `{{ pojemnik.element }}`. Django pod spodem próbuje dopasować ten zapis na trzy sposoby, zachowując ścisłą kolejność:

### 1️⃣ Krok 1: Słownik (Dictionary lookup)

Django najpierw sprawdza, czy `pojemnik` jest słownikiem, a `element` jego kluczem.
- **W szablonie:** `{{ pracownik.imie }}`
- **Działanie Pythona:** `pracownik['imie']`

### 2️⃣ Krok 2: Atrybut obiektu (Attribute lookup)

Jeśli krok pierwszy się nie powiedzie, Django sprawdza, czy `pojemnik` to obiekt posiadający pole (atrybut) o nazwie `element`.
- **W szablonie:** `{{ samochod.marka }}`
- **Działanie Pythona:** `samochod.marka`

### 3️⃣ Krok 3: Indeks na liście (List-index lookup)

Jeśli powyższe zawiodą, a `pojemnik` jest listą lub krotką, Django sprawdza, czy `element` jest liczbą całkowitą i traktuje go jako indeks.
- **W szablonie:** `{{ zakupy.0 }}` _(pobiera pierwszy element z listy)_
- **Działanie Pythona:** `zakupy[0]`

> [!NOTE] Brak dopasowania
> 
> Jeśli żaden z powyższych kroków nie przyniesie rezultatu, Django nie zgłasza błędu (np. `KeyError` czy `IndexError`). Zamiast tego domyślnie wstawia w szablonie **pusty ciąg znaków** (`""`).

## 2. Automatyczne wywoływanie metod (Method-calling)
Jedną z kluczowych zasad architektury Django jest: **w szablonach nigdy nie używamy nawiasów wywołania `()`**.
### Jak to działa?
Jeśli podczas **Kroku 2 (szukania atrybutu)** Django napotka pole, które jest funkcją lub metodą (czyli obiektem typu _callable_), **automatycznie ją uruchomi w tle** i wyświetli jej wynik.
### Przykład 1: Obliczanie sumy
Wyobraźmy sobie obiekt zamówienia ze sklepu internetowego (`zamowienie`), który posiada metodę obliczającą sumę koszyka: `oblicz_sume()`.

- **W Pythonie:** `kwota = zamowienie.oblicz_sume()`
- **W szablonie Django:** `{{ zamowienie.oblicz_sume }}`
    Django sam widzi, że to funkcja, wywołuje ją za nas i wstawia obliczoną kwotę w miejsce znacznika.
### Przykład 2: Pobieranie powiązanych danych w pętli
Bardzo często metody zwracają listy obiektów, które chcemy od razu wyświetlić na stronie:

```Django
{% for ksiazka in autor.pobierz_wszystkie_ksiazki %}
    <li>{{ ksiazka.tytul }}</li>
{% endfor %}
```

- **Co robi Django?** Wywołuje w tle metodę `autor.pobierz_wszystkie_ksiazki()`, otrzymuje listę książek, po której pętla `{% for %}` może bez problemu iterować.
## 🧠 Dlaczego to tak zaprojektowano? (Filozofia Django)
1. **Separacja obowiązków (Separation of Concerns):** Szablony mają służyć wyłącznie do _prezentacji_ danych. Brak możliwości użycia nawiasów `()` uniemożliwia przekazywanie argumentów do funkcji z poziomu HTML. Jeśli funkcja wymaga argumentów, musisz przygotować te dane wcześniej w widoku (`views.py`).
2. **Bezpieczeństwo i czystość kodu:** Kod HTML pozostaje przejrzysty, wolny od skomplikowanej logiki programistycznej i bezpieczny przed przypadkowym wykonaniem niedozwolonego kodu.