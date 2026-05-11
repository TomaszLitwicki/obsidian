## 0. WSL
`cd /mnt/c/projekty` - montowanie dysków windowsa na linuxie
## 1. Sprawdzanie wersji Pythona

| Zadanie | Windows | Linux / macOS |
| --- | --- | --- |
| Wersja domyślna | `python --version` | `python3 --version` |
| Skrócona komenda | `py -V` | `python3 -V` |
| Lokalizacja pliku | `where python` | `which python3` |

## 2. Uruchamianie skryptów

| Zadanie | Windows | Linux / macOS |
| --- | --- | --- |
| Uruchomienie `app.py` | `python app.py` | `python3 app.py` |
| Przez launcher (Windows) | `py app.py` | - |
| Tryb interaktywny po skrypcie | `python -i app.py` | `python3 -i app.py` |

## 3. Środowisko wirtualne (`venv`)

Zawsze twórz `venv` wewnątrz katalogu projektu, aby odizolować biblioteki.
### Krok 1: Tworzenie

* **Windows/Linux/macOS:** `python -m venv venv`
*(Drugie `venv` w komendzie to nazwa folderu – standardowo używamy tej samej).*

### Krok 2: Aktywacja

* **Windows (CMD):** `venv\Scripts\activate`
* **Windows (PowerShell):** `.\venv\Scripts\Activate.ps1`
* **Linux / macOS:** `source venv/bin/activate`

### Krok 3: Dezaktywacja (wyjście)

* **Wszystkie systemy:** `deactivate`

## 4. Zarządzanie pakietami (`pip`)

Używaj tych komend **tylko przy aktywnym** środowisku wirtualnym.

* **Instalacja biblioteki:** `pip install <nazwa>`
* **Podgląd (tabelka):** `pip list`
* **Generowanie wymagań (produkcyjne):** `pip freeze > requirements.txt`
* **Instalacja z pliku (u kogoś):** `pip install -r requirements.txt`

---

## 💡 Pro-tipy na rozmowę (Q&A)

**Q: Dlaczego `pip freeze` jest lepszy niż `pip list` do tworzenia requirements.txt?**
*A: `pip freeze` generuje format gotowy do instalacji (`nazwa==wersja`) i pomija pakiety administracyjne, takie jak sam `pip` czy `setuptools`.*

**Q: Co to jest "Pinning" wersji?**
*A: To zapisywanie konkretnej wersji biblioteki (np. `requests==2.31.0`) zamiast samej nazwy. Gwarantuje to, że kod nie "wywali się" w przyszłości, gdy wyjdzie niekompatybilna aktualizacja biblioteki.*

**Q: Czy wysyłamy folder `venv/` na GitHub?**
*A: Absolutnie nie! Folder `venv` dodajemy do `.gitignore`. Współpracownicy odtworzą środowisko u siebie za pomocą Twojego pliku `requirements.txt`.*

**Q: venv czy Docker?**
*A: `venv` izoluje tylko biblioteki Pythona. `Docker` izoluje cały system operacyjny, bazy danych i inne zależności systemowe. Na start projektu `venv` zazwyczaj wystarcza.*