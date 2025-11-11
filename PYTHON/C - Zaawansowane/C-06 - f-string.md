# Sposoby łączenia stringów ze zmiennymi
```
name = 'Tomasz'

print('Cześć',name,'!')
print('Cześć ' + name + '!')
print('Cześć %s!' %  name)
print('Cześć {}!'.format(name))
```

# f-string
Ala clue zagadnienia to f-stringi:
`print(f'Cześć {name}!)

na każdej zmiennej przekazanej do f-stringa można:
- wykonać metodę!
	`print(f'Cześć {name.upper()}!)
- wywołać funkcję:
```
def zmien_tekst(text):
    text = text.strip().upper()
    return text
      
print(f'Cześć {zmien_tekst(name)}!')
```
# instrukcje warunkowe
 `print(f'GRATULACJE! {'Wygrałaś' if name.endswith('a') else 'Wygrałeś'}...')`

**X if warunek else Y**

# liczby
## zaokrąglanie po przecinku
a = 0.123456789
`print(f'{a.3f}')` <- zaokrągla, nie odcina
*0.123*
## separator
a=514335475
`print(f'{a:,})` *514,335,475*
`print(f'{a:_})` *514_335_475*
## przeliczanie liczb na inne systemy
number = 255
- b - binarny: `print(f'{number:b}')`  - *11111111
- o - ósemkowy: `print(f'{number:o}')`  - *377*
- x - szenastkowy: `print(f'{number:x}')`  - *ff* 
- c - znak ASCII:  `print(f'{number:c}')`  - *ÿ*

## przeliczanie liczb na system dziesiętny
To się odbywa już za pomocą funkcji z drugim argumentem base-system
**int(str_liczby, base)**
```
int("11111111", 2) → binarny na dziesiętny
int("377", 8) → ósemkowy na dziesiętny
int("ff", 16) → szesnastkowy na dziesiętny
```
w f-stringach:
```
number = 255

print(f"""
Dziesiętnie:   {number}
Binarnie:      {number:b}   → {int(f"{number:b}", 2)}
Ósemkowo:      {number:o}   → {int(f"{number:o}", 8)}
Szesnastkowo:  {number:x}   → {int(f"{number:x}", 16)}
""")
```
## foratowanie daty

```
import datetime

now = datetime.datetime.now()
print(f'{now:%Y-%m-%d %H:%M:%S}')
```
*2025-09-09 22:45:30*
## Justowanie
```
zwierzaki = {
    'Namira Nad Stawami': 'smooky cream',
    'wiosenka': 'myszata',
    'Moro': 'kary'
}

for name, color in zwierzaki.items():
    print(f'{name} {color}')
```

```bez_formatowania
Namira Nad Stawamismoky cream
wiosenkamyszata
Morokary
```

`print(f'{name:25} {color}')` - 25 znaków na {name}
```25_znakow
Namira Nad Stawami       smoky cream
wiosenka                 myszata
Moro                     kary
```

`print(f'{name:>25} {color}')` - {name} do prawej na 25 znaków
  ```do_prawej
  Namira Nad Stawami smoky cream
            wiosenka myszata
                Moro kary
```

`print(f'{name:^25} {color}')` - {name} wyśrodkowane na 25 znaków
```wysrodkowane
   Namira Nad Stawami     smoky cream
        wiosenka          myszata
          Moro            kary

```

`print(f'{name:_^25} {color}')` - znak wypełnienia _
```znak_wypelnienia
___Namira Nad Stawami____ smoky cream
________wiosenka_________ myszata
__________Moro___________ kary
```

`print(f'{name:_<25} {color:}')` - znak wypełnienia z wyrównaniem
```wypelnienie_wyrownanie
Namira Nad Stawami_______ smoky cream
wiosenka_________________ myszata
Moro_____________________ kary
```
## wyświetlanie nazwy zmiennej i wartości
`{zmienna=}` -> `zmienna='wartość'`



