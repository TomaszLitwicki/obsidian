...i podpowiedzi typów.
powiązanie dowolnych wyrażeń z argumentami funkcji i zwracanymi wartościami
`def funkcja (a: 'adnotacja paramteru') -> 'anotacja fukcji':`
`a: 'adnotacja zmiennej' = wartość

w PEP 484 adnotacje jako podpowiedzi typów, ponieważ Python jest językiem dynamicznie typowanym a typ zmiennej jest określany w trakcie działania programu. Adnotacje podpowiadają typ użytkownikowi kodu.

`def funkcja(a: int, b: typ_zmiennej) -> int:
`c: int = 2`
`zmienna: typ_zmiennej = wartość

Gdy funkcja niczego nie zwraca, tylko np wydruk, oczekujemy None
`def print() -> None:
	`print("hello!")

# mypy
instalacja w terminalu:
`pip install mypy`

Statyczne sprawdzanie typów
w terminalu:
`mypy nazwa_pliku.py`

inne narzędzia do sprawdzania kodu:
[https://blog.codacy.com/]
[https://blog.codacy.com/python-static-analysis-tools]

