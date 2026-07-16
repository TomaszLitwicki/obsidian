## Tworzenie 
### plików
`touch plik.txt` - tworzy pusty plik
`echo "tekst" > plik.txt` - tworzy plik i zapisuje w nim tekst
`echo "dodawanie" >> plik.txt` - dodaje na końcu w pliku

### katalogów
`mkdir nowy_katalog` - tworzenie nowego folderu
`mkdir katalog && cd $_` - tworzy katalog i od razu do niego przechodzi 
	`$_` - zmienna w linuxie, która podstawia ostatni argument
## wyświetlanie zawartości
`cat plik.txt` - wyświetla zawartość w konsoli
`less plik.txt` - otwiera długi plik i można przewijać strzałkami
`grep "czego szukam" nazwa_pliku.txt` - Global Regular Expression Print
	`-i` ignore case
	`-v` invert match
	`-n` number of line
	`-r` recursive(szuka we wszystkich plikach) `grep -r "szukam" .`
	
	` grep -E "class|def" lists/tests.py` - pokazuje wszystkie klasy i funkcje w pliku tests.py w katalogu lists
## vim - edytor
`vim plik.txt` -> otwarcie/utworzenie pliku
`i` -> --INSER-- tryb edycji
`Esc` -> zamknięcie trybu edycji
`:wq` -> zapisanie i wyjście
`:q!` -> wyjście bez zapisywania
`u` -> undo, cofnięcie zmian
`dd` -> delete całej linii gdzie stoi kursor
## Flagi
`ls -l` :(long) wyświetla w słupku
`ls -h` :(human readable) rozmiar w MB nie w b
`ls -a` :(all) pokazuje pliki ukryte
`rm -r` :(recursive) usuwa katalog wraz z zawartością


---
## zapisywanie pracy
`history 10 > moje_komendy.txt`
`script sesja.log`
	praca która jest rejestrowana
exit
