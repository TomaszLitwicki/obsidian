## Otwieranie pliku metodą ręczną:
```
file = None  
  
try:  
    file = open('text.txt', encoding='utf-8')  
    file_content = file.read()  
    print(file_content)  
except Exception:  
    pass  
finally:  
    if file is not None:  
        file.close()
```

## Obsługa pliku przez instrukcję WITH:

`with open('text.txt', 'r', encoding='utf-8') as file:
	DOSTĘPNE INNE PARAMETRY:
	'r' - read/tylko do odczytu
	'w' - write/nadpisuje cały plik
	'a' - append/dopisywanie
	'w+' - nadpisywanie i odczytywanie pliku
	'a+' - dopisywanie i odczytywanie pliku
	'r+' - odczytywanie pliku i dopisywanie, ale UWAGA![^1]
	'x' - tworzy nowy plik. Jeśli istnieje już zwraca error: "FileExistsError"

'r' - ale może wystąpić błąd próby otwarcia nieistniejącego pliku:
![[B-01 - Wyjątki#^bladotwarciapliku]]
'a' i 'w' - przy zapisywaniu pliku takiego błędu już nie ma, Python sam tworzy plik

## .seek(arg1, arg2)

Ta metoda pozwala **przestawić kursor pliku** (ang. file pointer).  
Kursor wskazuje miejsce, z którego będą wykonywane kolejne operacje (np. odczyt, zapis).

- **`arg1`** → ile bajtów przesuwasz kursor.
- **`arg2`** → skąd liczysz ten przesuw (tzw. `whence` = punkt odniesienia).
	- 0 - od początku (domyślnie)
	- 1 - od bieżącej pozycji
	- 2 - od końca
```
f.seek(0)     # ustaw się na początku pliku
f.seek(0, 2)  # ustaw się na końcu pliku
f.seek(-5, 2) # cofnij się 5 bajtów od końca
```


## Czytanie pliku

`.read()` - cały plik w 1 stringu
`.readlie()` - tylko jedna linia (każde wywołanie to czytanie nowej linii)
`.readlines()` - zapisuje każdą linię do osobnego elementu LISTY!!!

`read(n)` → dokładnie n znaków,
`eadline(n)` → max n znaków z 1 linii,
`readlines(n)` → całe linie, aż ich suma znaków ≈ n

Zasysanie do kodu tylko jednej linii na każde powtórzenie pętli:
```czytanie_tylko_jednego_wiersza
with open('text.txt', encoding='utf-8') as file:  
    for line in file:  
        print(line)
```

## Zapisywanie pliku

```
with open('text.txt', encoding='utf-8') as file:  
    file.write('Tekst jaki zostanie wprowadzony do pliku')
```

## inne operacje na plikach

`.truncate()` - usuwa resztę zawartości pliku

## wyświetlanie znaków
`jakiś_tam_string [:i]` - kasuje od i znaku od początku
`jakiś_tam_string [:-i]` - kasuje i ostatnich znaków (od końca)
`jakiś_tam_string [i:]` - kasuje i pierwszych znaków (od początku)
`jakiś_tam_string [-i:]` - kasuje do i znaków od końca

## Path

	pathlib - moduł
	Path - klasa

```
from pathlib import Path  
  
FILE_NAME = 'biblioteka_filmow.csv'  
FILE_FOLDER = 'C:/Users/tomas/PycharmProjects/Film-Library/biblioteka/'  
  
folder = Path(FILE_FOLDER)  
folder.mkdir(parents=True, exist_ok=True)  
  
scieza = Path(FILE_FOLDER + FILE_NAME)  
  
if not scieza.is_file():  
    with open(FILE_FOLDER + FILE_NAME, 'x'):  
        pass
```

[^1]: w trybie r+ nadpisujemy  tam, gdzie znajduje się kursor