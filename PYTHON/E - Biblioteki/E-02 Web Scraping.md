'/robots.txt' do adresu strony (np. 'https://example.com/robots.txt' ) pozwala zobaczyć, jakie zasady administrator witryny ustawił dla robotów (czyli automatycznych skryptów — crawlerów, scraperów, botów indeksujących, itd.).

https://www.scrapethissite.com/
https://realpython.github.io/fake-jobs/
https://toscrape.com/

## Pakiet requests

`import requests`

Służy do pobierania danych z internetu poprzez wysyłanie zapytań HTTP (GET, POST itp.).
Pozwala łatwo pobrać kod HTML strony, który potem analizuje się innymi narzędziami.
## Pakiet beautifulsoup4

`from bs4 import BeautifulSoup`

Służy do parsowania i przeszukiwania kodu HTML lub XML.
Zamienia surowy HTML w „drzewo obiektów”, dzięki czemu można łatwo znaleźć konkretne elementy (np. tytuły, linki, opisy).

## Przykład
```Python
from bs4 import BeautifulSoup
import requests

class Country:
    def __init__(self, name, population):
        self.name = name
        self.population = population

def get_population(country):
    return int(country.population)

page = requests.get('https://www.scrapethissite.com/pages/simple/')
soup = BeautifulSoup(page.content, 'html.parser')

divs = soup.find_all('div', class_='country')

countries = []

for i in divs:
    name = i.find('h3', class_='country-name').get_text(strip=True)
    population = i.find('span', class_='country-population').get_text(strip=True)
    countries.append(Country(name, population))

sorted_countries = sorted(countries, key=get_population)

for i in sorted_countries:
    print(f'{i.name:>45} - {i.population}')

```


```Python
from bs4 import BeautifulSoup
import requests

class Country:
    def __init__(self, name, population):
        self.name = name
        self.population = int(population)

    def __lt__(self, other):
        return self.population < other.population

page = requests.get('https://www.scrapethissite.com/pages/simple/')
soup = BeautifulSoup(page.content, 'html.parser')

divs = soup.find_all('div', class_='country')

countries = []

for i in divs:
    name = i.find('h3', class_='country-name').get_text(strip=True)
    population = i.find('span', class_='country-population').get_text(strip=True)
    countries.append(Country(name, population))

sorted_countries = sorted(countries)

for i in sorted_countries:
    print(f'{i.name:>45} - {i.population}')
```

```Python
from bs4 import BeautifulSoup
import requests

class Job:
    def __init__(self, title, location, description):
        self.title = title
        self.location = location
        self.description = description

    def __str__(self):
        return f'{self.title} - {self.location}\n{self.description}\n'

page = requests.get('https://realpython.github.io/fake-jobs/')
soup = BeautifulSoup(page.content, 'html.parser')
cards = soup.find_all('div', class_='card-content')

for i in cards:
    title = i.find('h2', class_='title is-5').get_text(strip=True)

    if 'python' in title.lower():
        link = i.find('a', string='Apply').get('href')
        subpage = requests.get(link)
        subsoup = BeautifulSoup(subpage.content, 'html.parser')
        description = subsoup.find('p', attrs={"class": None, "id": None}).get_text(strip=True)
        location = subsoup.find('p', id='location').get_text()

        job = Job(title, location, description)
        print(job)

```