Procesowanie danych:
1. txt - nieustrukturyzowane dane
2. csv - niewielka ilość ustrukturyzowanych danych
3. json - zwracanie danych przy pomocy REST API

A. Duża ilość danych
B. Dane powiązanie z innymi danymi

BAZA DANYCH:
- relacyjne (SQL) - Structured Query Language
	-DIALEKTY SQL:
	- MySQL
	- PostgreSQL
	- Oracle
	- SQLite
- nierelacyjne (NoSQL)

KONWENCJA SQL - tabele nazywać korzystając z liczby pojedynczej rzeczownika (pojedynczej encji {bytu})

# POSTAĆ NORMALNA:
### **1NF**
- wartości atomowe
- brak powtarzających się grup
- Każdy wiersz danych musi mieć unikalny identyfikator PRIMARY KEY
### **2NF**
- brak funkcjonalnych zależności częściowych
- dotyczy tylko tabel z kluczem złożonym
- wszystkie kolumny wchodzą w skład klucza głównego lub PK jest jedną kolumną
### **3NF**
- brak zależności przechodnich
- kolumny niekluczowe zależą tylko od klucza głównego
### **BCNF**
- każdy determinant jest kluczem kandydującym
- bardziej restrykcyjna niż 3NF

## **Krótka esencja różnic zależności**

### **Zależność funkcjonalna**
A → C  
(ogólne pojęcie; normalizacja tutaj nic nie mówi)
### **Częściowa zależność**
(A, B) → C  
ale A → C lub B → C  
(dotyczy tylko klucza złożonego → łamie 2NF)
### **Zależność przechodnia**
A → B  
B → C  
→ A → C (pośrednio)  
(łamie 3NF)

## Dane ATOMOWE:
1. W kolumnie z danymi atomowymi w jednym wierszu tabeli nie może się znajdować kilka wartości tego samego typu.
2. W tabeli zawierającej dane atomowe nie może być kilku kolumn zawierających dane tego samego typu.