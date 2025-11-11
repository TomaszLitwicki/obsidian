JavaScript Object Notation, JSON  – lekki format wymiany danych komputerowych. JSON jest formatem tekstowym, bazującym na podzbiorze języka JavaScript. Typ MIME dla formatu JSON to application/json.
# Wczytywanie

`import json
`json.loads( 'jakiś string' )` - oczekuje STRINGA
`json.load(plik.json)` - oczekuje PLIKU
#### I-metoda
`entries = json.loads('string z kodem json')
#### II-metoda
`with open('plik.json') as plik_json:
	`entries = json.load(plik_json)
### wyświetlanie 
`print(entries)` - wyświetla całość list
`print(entries[indeks])` - wyświetla jedną listę spod indeksu
`print(entries[indeks]['element'])` - wyświetla element z listy spod indeksu
`print(entries[indeks]['element'][indeks])` - i.t.d.

# Zapisywanie
`import json
`json.dumps('string')
`json.dump(plik.json)
```tworzenie_obiektu
person = {
<cały tekst JSON>
}
```
#### I-metoda stringa


```zpisywanie_obiektu
json_string = json.dumps(person, indent=4)
print(json_string)
```

#### II-metoda pliku
```funkcja_with
with open('nazwa_pliku.json', 'w', ) as plik:
	json.dump(person, file, indent=4)
```

indent=4 - wcięcie=ilość_wcięć - do ładnego formatowania pliku w formacie .json