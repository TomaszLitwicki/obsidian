- REST - Representational State Transfer (przekazanie stanu)
- API - Application Programming Interface

Czym jest REST API? - Sposób komunikacji 

Nie jest to technologia tylko sposób w jaki się komunikujemy, styl architektury aplikacji który określa jak poszczególne serwisy mają się ze sobą komunikować w ramach danej aplikacji.

MONOLIT: Wszystkie funkcjonalności w jednej aplikacji
MIKROSERWISY: Każda funkcjonalność jako osobna aplikacja (osobny serwis)

## Komunikacja
[Protokoły HTTP](https://pl.wikipedia.org/wiki/HTTP): (ang. Hypertext Transfer Protocol) – protokół stworzony przez Tima Bernersa-Lee na potrzeby komunikacji między klientem a serwerem w sieci [WWW](https://pl.wikipedia.org/wiki/World_Wide_Web) (ang. World Wide Web).

1. GET – pobranie zasobu wskazanego przez URI, może mieć postać warunkową, jeśli w nagłówku występują pola warunkowe takie jak „If-Modified-Since”
2. HEAD – pobiera informacje o zasobie, stosowane do sprawdzania dostępności zasobu
3. PUT – przyjęcie danych przesyłanych od klienta do serwera, najczęściej, aby zaktualizować wartość encji
4. POST – przyjęcie danych przesyłanych od klienta do serwera (np. wysyłanie zawartości formularzy)
5. DELETE – żądanie usunięcia zasobu, włączone dla uprawnionych użytkowników
6. OPTIONS – informacje o opcjach i wymaganiach istniejących w kanale komunikacyjnym
7. TRACE – diagnostyka, analiza kanału komunikacyjnego
8. CONNECT – żądanie przeznaczone dla serwerów pośredniczących pełniących funkcje tunelowania
9. PATCH – aktualizacja części danych

[Kody odpowiedzi](https://pl.wikipedia.org/wiki/Kod_odpowiedzi_HTTP)

ENDPOINT - Adresy pod którymi są dostępne metody
