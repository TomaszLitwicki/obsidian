```listy
names = ['Tomasz','Tori', 'Figo','Namira','Wiosenka', 'Kasia']
species = ['człowiek', 'pies', 'pies', 'koń', 'koń']
color = ['opalony','czarno-biały','czarno-biały','burokremowy','myszaty']
zaspoly = ['zespol_A', 'zespol_B']
```

# zip
Funkcja wbudowana w Python'a do łączenia list, zwraca obiekt zip, który można przekształcić na listę, co daje tuple.

`zipped = zip(lista1, lista2...)` ->zip object
`list(zipped)` -> daje listę krotek

na przykład łączenie list  imion z adresami e-mail, czy telefonami.

**`[('Tomasz', 'człowiek', 'opalony'), ('Tori', 'pies', 'czarno-biały'), ('Figo', 'pies', 'czarno-biały'), ('Namira', 'koń', 'burokremowy'), ('Wiosenka', 'koń', 'myszaty')]`**

# zip_longest
Dla list o różnych długościach:
`from itertools import zip_longest`
przy brakujących elementach wstawiana jest wartość NONE

`[('Tomasz', 'człowiek', 'opalony'), ('Tori', 'pies', 'czarno-biały'), ('Figo', 'pies', 'czarno-biały'), ('Namira', 'koń', 'burokremowy'), ('Wiosenka', 'koń', 'myszaty'), ('Kasia', None, None)]`

# cycle
`from itertools import cycle
zapętla listę wstawioną w funkcję cycle()
`zipped = zip(names, cycle(zespoly))
`[('Tomasz', 'zespol_A'), ('Tori', 'zespol_B'), ('Figo', 'zespol_A'), ('Namira', 'zespol_B'), ('Wiosenka', 'zespol_A'), ('Kasia', 'zespol_B')]`

Na przykład przypisywanie członków do zespołów ;)




