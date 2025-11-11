`map()` to wbudowana funkcja w Pythonie, która **bierze funkcję** oraz **iterowalny obiekt** (np. listę) i zwraca nowy obiekt, w którym każdy element został przetworzony przez tę funkcję.

Można myśleć o tym tak: masz listę marchewek, chcesz każdą obrać — i zamiast obierać je ręcznie w pętli, mówisz „hej, Python, zastosuj funkcję `obierz()` do każdej marchewki z tej listy”.

### Składnia

`map(funkcja, iterable)`

- `funkcja` → co chcesz zrobić z każdym elementem
- `iterable` → np. lista, krotka, string

`map()` zwraca **iterator**, więc często owijasz go w `list()` żeby dostać gotową listę.

---

### Przykłady

**Podniesienie każdej liczby do kwadratu:**

`liczby = [1, 2, 3, 4] kwadraty = list(map(lambda x: x**2, liczby)) print(kwadraty)`

Wynik:

`[1, 4, 9, 16]`

**Konwersja liczb na stringi (używane w `.join()`):**

`liczby = [1, 2, 3] napisy = list(map(str, liczby)) print(", ".join(napisy))`

Wynik:

`1, 2, 3`