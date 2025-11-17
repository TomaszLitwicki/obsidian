Struktura typu KLUCZ - WARTOŚĆ
Elementy słownika nie posiadają kolejnych indeksów
Jest to struktura uporządkowana, a elementy są przechowywane w takiej kolejności, w jakiej zostały dodane
Nie może być w słoniku dwóch takich samych kluczy (N.p. dwóch Tomaszów nie może być, dodany kolejny Tomasz z nową wartością, nadpisuje tego pierwszego.)

```
my_dict = {
	'Tomasz' : 001,
	'Kasia' : 002
} 
```

Sposób pytania o wartość słownika:
`my_dict.get('Tomasz')` - NONE gdy nie figuruje
`my_dict.get('Andrzej', 'Nie ma')` - "Nie ma" gdy nie figuruje
`my_dict['Tomasz']` - błąd gdy nie figuruje

Dodawanie do słownika (i zmienianie):
`my_dict['dodaję klucz'] = wartość

Usuwanie ze słownika:
`del my_dict['KLUCZ']` - gdy KLUCZ nie istnieje zwraca błąd *KeyError*
`my_dict.pop['KLUCZ',co_jeśli_klucz_nie_istnieje]` - wartość usuniętego tak klucza można przypisać do jakiejś zmiennej lub zwrócić inną wartość, gdy dany KLUCZ nie istnieje

**WYŚWIETLANIE SŁOWNIKA**
`for el in my_dict:`
	`print(el)` - wyświetla tylko klucze

`for el in my_dict.values():  
    `print(el)` - wyświetla tylko wartości kluczy

`for el in my_dict.items():  
    `print(el)` - wyświetla i klucze i wartości. Zwraca [[KROTKI]] (ang TUPLES), które są już indeksowane, co można wykorzystać:
    `print(el[0] + " tekst " + str(el[1]))`
	
`for imie, rok_ur in my_dict.items():`
	`print(imie + " ur. " + str(rok_ur))`
	Przechodząc przez wszystkie elementy słownika spodziewamy się IMIENIA i ROKU URODZENIA. Zamiast odwoływać się do indeksów el\[0] odwołujemy się do nazw wprowadzonych w pętli. Rozwiązanie ponoć czytelniejsze i lepsze.
	
	








