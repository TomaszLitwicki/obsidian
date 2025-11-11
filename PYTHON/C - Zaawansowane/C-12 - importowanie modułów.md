TOP-LEVEL CODE
`if __name__ == '__main__':
[Python docs](https://docs.python.org/3.13/library/__main__.html)


- importowanie całego modułu
	`import nazwa_modułu`
	wywołanie funkcji:
	`print(nazwa_modułu.nazwa_funkcji(argumenty))
	
- importowanie pojedynczych funkcji
	`from nazwa_modułu import nazwa_funkcji
	wywołanie funkcji:
	`print(nazwa_funkcji(argumenty))
	
- importowanie wszystkich funkcji z danego modułu
	BARDZO NIEPOLECANE - TYLKO DO TESTÓW
	`from nazwa_modułu import *
	
- aliasy, czyli zmiana nazwy importowanego modułu w konkretnym kodzie jako alias
	`import nazwa_modułu as inna_nazwa

