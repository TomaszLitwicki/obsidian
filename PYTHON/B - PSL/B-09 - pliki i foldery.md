# os (starszy)
`import os
`os.getcwd()` - wyświetla aktualny folder w jakim się znajdujemy (get current working directory)
`os.listdir(katalog)` - pokazuje jakie są pliki i foldery w tym katalog'u (można połączyć getcwd)
`new_directory = os.path.join(current_directory, 'nowy_katalog')` - łączy nazwę w nową ścieżkę. Może być ścieżka bezpośrednia lub pośrednia 
`os.mkdir(new_directory)` - tworzy katalog (make directory) na podstawie nowej ścieżki (jeśli już istnieje wywala error)
`os.rmdir(ścieżka do katalogu)` - usuwa katalog (jeśli nie istnieje też wywala błąd "FileNotFoundError" )
`print(os.path.exists('nazwa katalogu'))` - zwraca True lub False czy dany katalog istnieje
	`os.path.isdir('nazwa')` - czy jest to katalog
	`os.path.isfile('nazwa')` - czy jest to plik
można też jednocześnie utworzyć katalog i nowy plik:
`with open(os.path.join(ścieżka z nowym katalogiem, 'nazwa_pliku.rozszerzenie'), 'x')
`pass` - bo nic nie robimy
`'x'` - bo tworzymy nowy plik

# path (nowszy)
`form pathlib import path

`aktualny_folder = Path.cwd()` - current working directory
`print(aktualny_folder.iterdir())` - zwraca obiekt, trzeba pętlę:
`for i in aktualny_folder.iterdir():` - print zwraca katalogi
`i.is_file()` - zwraca bool czy plik
`i.is_dir()` - zwraca bool czy folder
`Path('nazwa_katalogu').mkdir()` - make directory (zwraca Error gdy istnieje już)
`Path('nazwa_katalogu').mkdir(exist_ok=True)` - make directory (OK Nawet gdy istnieje już)
`Path('nazwa_pliku.rozszerzenie ').touch()` - make directory (gdy istnieje już nie zwraca żadnego błędu tylko aktualizuje datę modyfikacji pliku exist_ok=True domyślnie). Aby wyjątek się pojawiał, trzeba zmienić .touch(exist_ok=False)
