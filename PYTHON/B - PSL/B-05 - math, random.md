### moduł związany z matematyką
`import math

`math.factorial(liczba)` - silnia ! - 
`math.floor(liczba)` - zaokrąglanie w dół
`math.ceil(liczba)` - zaokrąglanie w górę
`math.radians(stopnie)` - konwertowanie stopni na radiany

### maduł związany z losowością
`import random`

Generowanie liczb pseudolosowych!
Działanie w sposób deterministyczny. Wynik jest symulowany, aby sprawiał wrażenie losowości i jest to zawsze działanie algorytmu determinowanego początkową wartością - ziarnem (seed)
seed - często jest uzależniony od daty.

`random.seed(liczba ziarna)` - kontrolowanie losowania
`random.random()` - od 0 do 1
`random.uniform(od, do)` - losowanie liczby zmiennoprzecinkowej
`random.randint(od, do)` - losowanie liczby całkowitej
`random.choice(lista)` - losowanie elementu z listy
`random.choices(lista, k=ilość, weights=[i])` - losowanie k-elementów z listy z i-częstotliwością wybierania każdego elementu(kolejność)
`random.sample(lista, i)` - losowanie i-elementów z listy bez powtarzania elementów (jakby wyciąganie z elementów z listy i kolejne losowanie z pozostałych)
`random.shuffle(lista)` - układanie listy w losowej kolejności


