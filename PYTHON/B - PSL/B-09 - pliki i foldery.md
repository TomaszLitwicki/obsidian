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


### Dlaczego `pathlib` zrewolucjonizował pracę z plikami?

W starszym kodzie do obsługi plików używano modułu `os.path`. Ścieżki były traktowane jak zwykły ciąg znaków (tekst). `pathlib` wprowadza podejście **obiektowe**. Ścieżka staje się inteligentnym "obiektem", który sam w sobie posiada mnóstwo użytecznych funkcji i właściwości.

Oto dwie największe zalety klasy `Path`:

1. **Magiczny ukośnik `/`**: Nie musisz już używać niewygodnych funkcji do łączenia nazw folderów. Możesz użyć zwykłego znaku dzielenia, co wygląda bardzo naturalnie.
2. **Koniec wojen systemowych**: Windows używa do ścieżek ukośnika wstecznego (`\`), a Linux i macOS zwykłego (`/`). Obiekt `Path` sam rozpoznaje, na jakim systemie uruchomiony jest kod, i automatycznie konwertuje ścieżki na odpowiedni format. Twój kod po prostu działa wszędzie.

---

### Klasa `Path` w akcji: Jak z niej korzystać?

```Python
from pathlib import Path

# 1. TWORZENIE ŚCIEŻKI
# Zaczynamy od stworzenia obiektu Path. Możesz podać ścieżkę absolutną lub relatywną.
katalog_domowy = Path.home()  # Automatycznie pobiera folder domowy użytkownika
folder_projektu = Path("moje_projekty") / "python" / "nauka"
moj_plik = folder_projektu / "raport.csv"
# Zwraca ścieżkę do folderu, w którym aktualnie znajduje się Twój uruchomiony skrypt
obecny_folder = Path.cwd() # CWD to skrót od Current Working Directory
pelna_sciezka = folder_glowny / "dane" / "rok_2026" / "marzec.txt" # Łączymy obiekt Path ze zwykłymi stringami używając ukośnika

# 2. WYCIĄGANIE INFORMACJI Z OBIEKTU (Właściwości)
# Nie musisz ciąć tekstu, Path ma to wszystko gotowe:
print(moj_plik.name)       # Zwraca: raport.csv (cała nazwa pliku)
print(moj_plik.stem)       # Zwraca: raport (nazwa bez rozszerzenia)
print(moj_plik.suffix)     # Zwraca: .csv (samo rozszerzenie)
print(moj_plik.parent)     # Zwraca: moje_projekty/python/nauka (folder nadrzędny)

# 3. SPRAWDZANIE STATUSU (Metody)
# Bardzo przydatne przed wykonaniem jakiejś operacji:
if moj_plik.exists():
    print("Plik istnieje!")

if moj_plik.is_file():
    print("To jest plik, a nie folder.")
    
if folder_projektu.is_dir():
    print("To jest katalog.")
```

`BASE_DIR = Path(__file__).resolve().parent`
To klasyczny sposób na ustalenie folderu głównego projektu:
1. **`__file__`**: To wbudowana zmienna Pythona, która zawiera ścieżkę do aktualnie wykonywanego skryptu.
2. **`Path(__file__)`**: Zamienia ten napis na obiekt klasy `Path`.
3. **`.resolve()`**: Tworzy pełną, absolutną ścieżkę (usuwa wszelkie `..`, `.` oraz rozwiązuje skróty/linki symboliczne). Dzięki temu masz pewność, skąd skrypt startuje.
4. **`.parent`**: Przechodzi o jeden poziom wyżej – czyli z pliku `skrypt.py` dostajesz ścieżkę do folderu, w którym ten plik się znajduje.
5. 
### Proste operacje na plikach (Czytanie i Pisanie)

Wcześniej, aby zapisać coś do pliku tekstowego, trzeba było używać instrukcji `with open(...) as plik:`. Klasa `Path` potrafi zrobić to w jednej linijce!
```Python
from pathlib import Path

notatka = Path("szybka_notatka.txt")

# Zapisywanie tekstu bezpośrednio do pliku
notatka.write_text("Witaj świecie! Uczę się pathlib.", encoding="utf-8")

# Odczytywanie całego tekstu z pliku
zawartosc = notatka.read_text(encoding="utf-8")
print(zawartosc)
```

> **Wskazówka:** Metody `read_text()` i `write_text()` są świetne do mniejszych plików. Jeśli przetwarzasz gigabajty danych, wciąż lepiej użyć klasycznego `with open()`, aby czytać plik po kawałku (linijka po linijce) i nie zablokować pamięci komputera. Nawet wtedy możesz przekazać obiekt `Path` do funkcji `open(moj_plik)`.

### Przeszukiwanie folderów

Jedną z najpotężniejszych funkcji klasy `Path` jest iterowanie (przechodzenie) po zawartości katalogów za pomocą `.iterdir()` lub metody szukającej `.rglob()` (od "recursive glob", czyli przeszukiwanie w głąb).
```Python
from pathlib import Path

folder = Path("jakis_folder_z_danymi")

# Aby znaleźć wszystkie pliki z rozszerzeniem .yaml w folderze i jego podfolderach:
for plik in folder.rglob("*.yaml"):
    print(f"Znalazłem plik konfiguracyjny: {plik.name}")
```

---
