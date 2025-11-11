**OPERATOR PRZYPISANIA WYRAŻENIOWEGO** (mors)
:=
**przypisuje i zwraca wartość jednocześnie**
`print (a := 2)
bo zapis `pirint(a = 2)` generuje błąd.

Aby wylosować 10 liczb i wybrać wszystkie większe od 5 Konstrukcją Składniową **comprehension** z C-01 potrzebne by były 2 linie kodu:
```
numbers = [random.randint(1, 10) for _ in range (10)]
[2, 3, 3, 3, 5, 1, 10, 9, 1, 1]  
  
numbers2 = [num for num in numbers if num > 5]
[10, 9]
```
Z operatorem Walrus := można w jednej:
`liczby = [num for _ in range(10) if (num := random.randint(1,10)) > 5]`

Inny przykład z pobieraniem danych od użytkownika: 
``` bez_Walrusa
wydatki = []  
wydatek = None  
  
while wydatek != 0:  
    wydatek = int(input("Dodaj wydatek (0 aby zakończyć): "))  
    if wydatek > 0:  
        wydatki.append(wydatek)  
```

``` z_Walrusem
expenses = []  
while (expense := int(input("Enter value: "))) != 0:  
    expenses.append(expense)
```

