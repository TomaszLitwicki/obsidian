 ==Obiekt to byt z przypisanymi atrybutami i funkcjami nazywane metodami==
**OOP – object oriented programming**

4 filary programowania obiektowego:
Aby programowanie było w pełni "obiektowe", opiera się na czterech fundamentach: 

- **Abstrakcja:** Ukrywanie skomplikowanych szczegółów i pokazywanie tylko tego, co niezbędne.
- **Enkapsulacja (Hermetyzacja): **Zamykanie danych wewnątrz obiektu, aby chronić je przed niepożądanym dostępem z zewnątrz.
- **Dziedziczenie:** Możliwość tworzenia nowych klas na bazie już istniejących (np. klasa "Ciężarówka" może dziedziczyć cechy po klasie "Samochód").
- **Polimorfizm:** Pozwala różnym obiektom reagować na to samo polecenie w różny sposób.


Obiekt -> USER 
	ATRYBUTY ->(STAN) Login, Hasło, Wyświetlana Nazwa... 
	METODY ->(ZACHOWANIE) Do weryfikowania poprawności Loginu i Hasła...

Definiowany OBIEKT -> zwany KLASĄ -> szablon tworzenia egzemplarzy OBIEKTÓW -> to INSTANCJE

- KLASA - To projekt (np. domu) - szablon na podstawie którego tworzone są INSTANCJE
	- INSTANCJA to OBIEKT stworzony z KLASY - "dom" z "projektu" - posiada ATRYBUTY i METODY
		- ATRYBURY - elementy INSTANCJI - "ściany", "dach", "stropy"
		- METODY - czynności INSTANCJI - "ogrzewanie pomieszczeń", "oświetlanie pomieszczeń", "puszczanie wody do prysznicy"
 
KONSTRUKTOR to specjalna metoda uruchomiona podczas tworzenia nowej INSTANCJI KLASY: `__ init__(self, pozostale_atrybuty_instancji)`
- self - parametr konstruktora to odwołanie do samego siebie do tworzonej instancji klasy 
- `self.pozostale_atrybuty_instancji = 'jakas wartosc do przekazania'