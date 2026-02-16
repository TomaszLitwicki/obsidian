GIT - Rozproszony System Kontroli Wersji (ang. git - palant/głupek lub Global Information Tracker)

## Instalacja
`git --version` sprawdzenie w konsoli, czy jest zainstalowany już.

## Repozytorium
PPM -> Pokaż więcej opcji -> Open Git Bash here

GLOBAL REPOSITORY <-- LOCAL REPOSITORY <-- STAGING AREA <-- WORKING DIRECTORY (TREE)
........................git pull--><--git push.............git log<--git commit............<--git add...........................git status<-- git init

working tree - drzewo robocze z plikami
stage: staging area - pliki czekające na trwałe zapisanie w repozytorium
repository - trwałe miejsce przechowywania commitów i wersji projektu
branche:
- master (main) - główna
- inne - klony do bezpiecznej pracy

pliki po git status:
1. untracked - nie śledzony
2. tracked - śledzony


### Status projektu
`git status`

Pokazuje:
- na jakim branchu jesteś
- które pliki są untracked, modified, staged
### Dodawanie
`git add nazwa.pliku`
wielu plików:
1. `git add plik-pierwszy.txt plik-drugi.txt` - dodaje wypisane pliki
2. `git add .` - dodaje wszystkie pliki (zmodyfikowane i nowe)
3. `git add *` - dodaje tylko pliki z bieżącego folderu, bez podkatalogów, ignoruje pliki ukryte.
4. `git add -u` - dodaje tylko modyfikowane pliki bez nowych i nieśledzonych (untracked)
5. `git commit -m "opis zmiany"` - commitowanie do repozytorium
6. `git commit -am"Wiadomość commita."` - łączy add i commit dla wszystkich zmienionych plików, ale tylko tych, które są już śledzone- tracked.
7. `git commit -a` - commitują się tylko edytowane pliki i uruchamia się Vim do napisania komentarza:
	1. `i` - wchodzi w tryb insert, czyli pisania
	2. `ESC` - wychodzi z trybu INSERT
	3. `:wq` - write and quit
	4. `:q!` - quit bez zapisywania

## Branche
`git branch` - lista branchów i roboczy oznaczony \*aktualny
`git switch -c "nazwa"` - tworzenie nowego brancha c-create
`git switch nazwa` - przełączanie się na brancha nazwa
`git merge nowy` - łączenie aktualnego brancha(roboczego) z branchem nowy

## Komendy

| komenda                                      | działanie                                       |
| -------------------------------------------- | ----------------------------------------------- |
| `git init`                                   | tworzenie repozytoriun                          |
| `git status`                                 | info o branchu                                  |
| `git add`                                    | dodawanie pliku do śledzenia                    |
| `git commit`                                 | commitowanie do repozytorium                    |
| `git log`                                    | historia commitów na danym branchu              |
| `git branch`                                 | pokazuje aktualne branche i \* roboczy          |
| `git switch nazwa`                           | przechodzi do brancha nazwa                     |
| `git switch -c "nazwa"`                      | tworzenie brancha                               |
| `git checkout -b`                            | stara komenda na tworzenie brancha              |
| `git merge nowy`                             | łączenie z mastera z branchem nowy              |
| `git push`                                   | wypychanie repozytorium do globalnego           |
| `git pull`                                   | zaciąganie globalnego repozytorium do lokalnego |
| `git clone http://github.com/nazwa/repo.git` | klonowanie repo z GitHub'a                      |
| `git fetch`                                  | porównanie z GitHub'em                          |

## Weryfikacja stanu repozytorium

| Komenda      | Operuje na        | Cel                   |
| ------------ | ----------------- | --------------------- |
| `git log`    | historia commitów | analiza przeszłości   |
| `git diff`   | różnice w plikach | analiza zmian         |
| `git show`   | jeden commit      | szczegóły zmiany      |
| `git branch` | struktura repo    | zarządzanie gałęziami |

---
### `git log`

Wyświetla historię commitów w aktualnym branchu.

* SHA (hash) commita
* autora
* datę
* wiadomość commit

Najważniejsze warianty

```bash
git log --oneline
```
→ skrócony zapis (hash + message)

```bash
git log --graph --all --oneline
```
→ wizualizacja rozgałęzień (bardzo przydatne przy Twojej analizie historii)

```bash
git log -p
```
→ pokazuje zmiany w plikach (patch)

```bash
git log nazwa_pliku.py
```
→ historia tylko jednego pliku

---
### `git diff`

Pokazuje różnice (diff) między stanami repozytorium.

```bash
git diff
```
→ różnice między working directory a staging area

```bash
git diff --staged
```
→ różnice między staging area a ostatnim commitem

```bash
git diff main origin/main
```
→ różnice między lokalnym a zdalnym branchem

Jak czytać diff:
```
- linia usunięta
+ linia dodana
```

---
### `git show`

Pokazuje szczegóły konkretnego commita.

```bash
git show
```
→ pokazuje ostatni commit

```bash
git show 67f7579
```
→ pokazuje wskazany commit

Zawiera:
* metadata
* diff
* autor
* datę

To w praktyce: `git log + git diff` dla jednego commita.

---
### `git branch`

Zarządzanie branchami.

```bash
git branch
```
→ lista lokalnych branchy
`*` oznacza aktualny branch

```bash
git branch -r
```
→ branche zdalne

```bash
git branch -a
```
→ wszystkie (lokalne + zdalne)

```bash
git branch nowy_branch
```
→ tworzy nowy branch

```bash
git checkout nowy_branch
```
→ przełącza na branch
(w nowszych wersjach: `git switch`)

---
