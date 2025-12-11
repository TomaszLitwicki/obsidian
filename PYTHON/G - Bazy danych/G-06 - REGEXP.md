## Kotwice (początek/koniec tekstu)

`^` – początek tekstu  
`$` – koniec tekstu

Przykład:

```
REGEXP '^[0-9]+$'
```

— cały tekst to cyfry.

---

## Klasy znaków

`[abc]` – dowolny z: a, b lub c  
`[^abc]` – dowolny oprócz a, b, c  
`[0-9]` – zakres cyfr  
`[A-Za-z]` – litery bez polskich znaków

Standardowe klasy SQL:

- `[:digit:]` – cyfry
    
- `[:alpha:]` – litery
    
- `[:alnum:]` – litery lub cyfry
    
- `[:space:]` – białe znaki (spacja, tab)
    
- `[:punct:]` – znaki interpunkcyjne
    

Przykład:

```
REGEXP '^[[:alpha:]]'
```

---

## Ilości

`+` – jeden lub więcej  
`*` – zero lub więcej  
`?` – zero lub jeden  
`{n}` – dokładnie n  
`{n,}` – n lub więcej  
`{n,m}` – między n a m

Przykłady:

```
'[0-9]+'      → jedna lub więcej cyfr  
'[A-Z]{3}'    → dokładnie 3 wielkie litery  
'[0-9]{2,4}'  → 2–4 cyfry  
```

---

## Kropka (dowolny znak)

`.` – jeden dowolny znak

Przykład:

```
'.{3}'   → dokładnie trzy dowolne znaki
```

---

## Alternatywa

`a|b` – a lub b

Przykład:

```
REGEXP '^(kot|pies)$'
```

---

# Przykłady praktyczne

**Tekst zaczyna się od cyfry**

```
REGEXP '^[0-9]'
```

**Tekst kończy się literą**

```
REGEXP '[A-Za-z]$'
```

**Zawiera spację**

```
REGEXP ' '
```

**Zawiera jakąkolwiek cyfrę**

```
REGEXP '[0-9]'
```

**Tylko cyfry, nic więcej**

```
REGEXP '^[0-9]+$'
```

---

Chcesz, mogę ci przygotować też ściągę z praktycznymi wzorcami typu „NIP”, „PESEL”, „kod pocztowy”, „email”, „numer telefonu”, wszystko w MySQL REGEXP.