CSV - CSV (ang. comma-separated values, wartości rozdzielone przecinkiem) – format przechowywania danych w plikach tekstowych i odpowiadający mu typ MIME text/csv ...

## Odczytywanie

Gdy CSV nie zawiera nagłówków:
```czytanie_csv_bez_naglowkow
import csv

with open('server_logs.csv') as csv_file:
    csv_reader = csv.reader(csv_file)

    for row in csv_reader:
        print(row[numer kolumny])
```

Gdy CSV zawiera nagłówki:
```czytanie_csv_z_naglowkami
import csv  
  
with open('server_logs.csv') as csv_file:
    csv_reader = csv.DictReader(csv_file)  
  
    for row in csv_reader:  
        print(row['nazwa nagłówka'])
```

## Zapisywanie

Bez nagłówków:
```zapis_bez_naglowkow
with open('dogs.csv', 'w', newline='') as csv_file:  
    csv_writer = csv.writer(csv_file)  
    csv_writer.writerow(['dane literowe', dane cyfowe itp])
```

Z nagłówkami:
```zapis_z_naglowkami
with open('dogs.csv', 'w', newline='') as csv_file:
    nazwy_pol = ['pies', 'Rok ur.']
    csv_writer = csv.DictWriter(csv_file, fieldnames=nazwy_pol)
    csv_writer.writeheader()
    csv_writer.writerow({'pies': 'Tori', 'Rok ur.': 2019})
    csv_writer.writerow({'pies': 'Tori',
                         'Rok ur.': 2019})
```

rodzaj seperatora można określić w `csv.writer(...)` lub `csv.Dictwriter(...)` parameterem `delimiter='-'` gdzie domyślnie jest ustawiony przecinek `,` 

