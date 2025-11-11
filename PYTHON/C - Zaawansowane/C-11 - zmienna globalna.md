```global
imie='Tomasz'
def change_name():
    global imie
    imie= 'Kasia'
change_name()
```

polecenie `global` wewnątrz funkcji zmienia zmienną globalną, a nie tylko lokalną wewnątrz funkcji. 

Jest to potrzebne na przykład do liczników wywołania funkcji:
```liczniki
counter = 0
count_a = 0
count_b = 0

def function_a():
    global counter, count_a
    counter += 1
    count_a += 1

def function_b():
    global counter, count_b
    counter += 1
    count_b += 1

for i in range (10):
    rand = random.choice(['A','B'])
    if rand == 'A':
        function_a()
    else:
        function_b()

print(f'Wykonano {counter} powtórzeń pętli')
print(f'Funkcja A została wywołana razy {count_a}')
print(f'Funkcja B została wywołana razy {count_b}')
```
