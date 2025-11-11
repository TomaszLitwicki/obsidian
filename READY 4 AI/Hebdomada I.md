# REST API

#RestAPI - Representational State Transfer Application Programming Interface
State - stan
Interfejs REST API jest **bezstanowy** (nie posiada stanu) czyli każde zapytanie jest niezależne od wszystkich pozostałych. Po raz zalogowaniu backend nie wie, że jesteśmy wciąż zalogowani. Serwer zwraca token, który oznacza, że jesteśmy zalogowani i możemy dokonywać jakiś zmian, ale ten otrzymany z serwera token wciąż na nowo zwracamy do serwera na potwierdzenie naszych uprawnień.

#request - Zapytanie
#response - odpowiedź
#HTTP - HyperText Transfer Protocol
Metody Protokołu HTTP:
Podastawowe:
	#GET - Metody protokołu - Pobieranie danych
	#POST - Dodawanie danych
	#PUT - aktualizacja danych (cały zasób)
	#PATCH - aktualizacja danych (część zasobu)
	#DELETE - usuwanie danych
Dodatkowe:
	- HEAD - Działa prawie jak GET, tylko serwer nie odsyła treści (body), a jedynie nagłówki odpowiedzi (przykład: sprawdzić, czy obrazek na stronie zmienił się od ostatniego pobrania?)
	- TRACE - „lustro”. Serwer odsyła dokładnie to, co dostał w żądaniu. (przykład: sprawdzić co zostało dodane po drodze przez serwery pośrednie)
	- CONNECT - używana głównie przez proxy do ustanowienia tunelu TCP/IP. (stosowana w HTTPS – kiedy chcesz nawiązać szyfrowane połączenie przez proxy, przeglądarka wysyła CONNECT, a potem przez ten tunel idzie już szyfrowany ruch TLS.)
	- OPTIONS - zapytania serwera: „jakie metody są dozwolone dla tego zasobu?”. Serwer odpowiada nagłówkiem Allow, np. Allow: GET, POST, OPTIONS. Zastosowanie: sprawdzanie możliwości API, a także mechanizm CORS (Cross-Origin Resource Sharing) w przeglądarkach – wtedy przeglądarka wysyła tzw. preflight request metodą OPTIONS, żeby zapytać, czy wolno wysłać później np. POST z niestandardowymi nagłówkami.

# LLM
Każde zapytanie do modelu AI jest niezależne, a sam model LLM jest BEZSTANOWY. Pamięć konwersacji jest kreowana sztucznie w sposób, że przy każdym zapytaniu w GPT "pod spodem" jest wysyłana informacja ze wcześniejszych zapytań. Okno kontekstu - ilość informacji, jaką może przetworzyć jest ograniczona. Po przekroczeniu jej wówczas wcześniejsze wiadomości są albo usuwane, albo streszczane co ważniejsze informacje. 

# Python
Do komunikacji z API potrzebna jest biblioteka
`requests`
