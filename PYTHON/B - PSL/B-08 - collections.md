# Counter
`from collections import Counter
Liczy wystąpienia poszczególnych elementów w tekście, oraz które występuje najczęściej?
Obiekt klasy Counter

ilość wystąpień każdej litery w tekście:
```
tekst = "mężny bądź chroń pułk twój i flag sześć"  
licznik = Counter(tekst.replace(" ",""))  - replace usuwa spacje
print(licznik.most_common()) - liczy wszystkie znaki

licznik = Counter(tekst.split()) - dzieli względem spacji
print(licznik.most_common()) - liczy już wszystkie słowa
```
most_comon() - zwraca krotkę, która jest także indeksowana.
`licznik.most_common()[0]` - zwraca pierwszy element z krotki, czyli słowo występujące najczęściej.
`licznik ['słowo']` - sprawdza ile razy wystąpiło "słowo"

# namedtuple
`from collections import namedtuple
nazywa krotkę i można się odwoływać po nazwie, a nie po indeksie. 
```
Czlowiek = namedtuple('Osoba', ['imie', 'wiek'])  
czlowiek = Czlowiek(imie='Tomasz', wiek=38)  
print(czlowiek.wiek)
```
imie i wiek - to pola (atrybuty obiektu)
Tomasz i 38 - to przypisane wartości do pola


