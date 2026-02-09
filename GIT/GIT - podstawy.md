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
7. 

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

