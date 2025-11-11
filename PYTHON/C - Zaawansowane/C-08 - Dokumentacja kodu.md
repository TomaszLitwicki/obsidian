Za pomocą  docstring """ opis """
[PEP 257](https://peps.python.org/pep-0257/)

```
def funkcja (arg: int) -> int:
	"""opis funkcji"""
	algorytm funkcji
	
print(funkcja.__doc__) -> doc strings
print(funkcja.__annotations__) -> adnotacje
```
*opis funkcji*
*{'arg': <class 'int'>, 'return': <class 'int'>}

`help(nazwa_modułu)`
