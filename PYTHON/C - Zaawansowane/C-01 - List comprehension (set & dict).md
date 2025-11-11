**KOSTRUKCJA SKŁADNIOWA**
Tworzona jest w pamięci.
Lista składana:
`lista = ['Tomasz', 'Kasia', 'Figo', 'Tori', 'Wrangler', 'Namira', 'Wiosenka', 'Moro']
# Kopiowanie listy
`imiona = [element for element in lista]`
# Modyfikacja kopii listy
z każdym elementem po kolei można wywołać metodę np: .upper()
`imiona = [element.upper() for element in lista]`
# Filtrowanie elementów z listy
z listy pierwotnej można wyłuskać poszczególne elementy instrukcją if
`dziewczyny = [name for name in lista if name.endswith('a')]
co można połączyć od razu z modyfikacją .upper()
`dziewczyny = [name.upper() for name in lista if name.endswith('a')]
# Warunkowe filtrowanie i modyfikowanie elementów listy
zwracany element jest modyfikowany przez metody w zależności od wyniku instrukcji warunkowej
`dziewczyny = [name.upper() if name.endswith('a') else name.lower() for name in lista]`
Filtrowanie warunkowe może odbywać się w dwóch miejscach:
- w pętli for gdzie jest filtrowany element do nowej listy
- w przypisaniu, gdzie jest filtrowany element do wykonania metody.
`dziewczyny = [name.upper() if name.endswith('a') else name.lower() for name in lista if len(name) > 5]`
# inne możliwości
`cyfry = [i for i in range (1,11)]` - daje listę cyfr od 1 do 9
`kwadraty = [i**2 for i in cyfry]` - daje listę kwadratów cyfr
można też wcisnąć funkcję:
```
def power2 (number):  
    return number * number  
  
numbers = [power2(num) for num in range (1,101)]  
```
# Set Comprehension
powstaje zbiór elementów niepowtarzających się ale w dowolnej kolejności
```
lista = ['Tomasz', 'Kasia', 'Figo', 'Tori', 'Wrangler', 'Namira', 'Wiosenka', 'Moro', 'Tomasz', 'Kasia', 'Kasia']  
lista2 = {name for name in lista}
```
# Dict Comprehension
Powstaje słownik z kluczem i wartością tego klucza. Tutaj przykład z długością imion:
```
lista = ['Tomasz', 'Kasia', 'Figo', 'Tori', 'Wrangler', 'Namira', 'Wiosenka', 'Moro']  
dlugosci_imion = {name: len(name) for name in lista}

{'Tomasz': 6, 'Kasia': 5, 'Figo': 4, 'Tori': 4, 'Wrangler': 8, 'Namira': 6, 'Wiosenka': 8, 'Moro': 4}
```

