## `repr()` w Pythonie

**`repr(object)`** zwraca **oficjalną, techniczną reprezentację obiektu** (*official string representation*, *debug representation*).

```
repr(x) == x.__repr__()
```

### Cechy

* dla programisty, nie użytkownika
* pokazuje **dokładną strukturę danych**
* ujawnia znaki specjalne (`\n`, `\t`, itp.)
* często pozwala odtworzyć obiekt przez `eval()`

### `repr()` vs `str()`

|          | `str()`    | `repr()`                |
| -------- | ---------- | ----------------------- |
| Cel      | czytelność | diagnostyka (debugging) |
| Odbiorca | użytkownik | programista             |
| Styl     | przyjazny  | surowy, precyzyjny      |

### Przykład

```python
s = "Hi\nBob"
str(s)   # Hi
         # Bob
repr(s)  # 'Hi\nBob'
```

### W wyjątkach

```python
except Exception as e:
    print(repr(e))
```

Daje pełną informację o błędzie (typ + treść), co ułatwia **debugowanie**.
