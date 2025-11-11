`if __name__ == "__main__":`
    ...

Atrybut Magiczny

- Zmienna specjalna `__name__` -> nazwa modułu
	- plik uruchomiony bezpośrednio -> `"__main__"`
	- moduł importowany -> nazwa importowanego modułu

Pod instrukcją warunkową wykona się kod tylko wówczas, gdy kod jest uruchamiany bezpośrednio w pliku, a zostanie pominięty, gdy moduł jest importowany z innego pliku.
