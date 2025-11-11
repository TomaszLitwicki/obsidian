a=0,3
b=0,1
a-b=0.19999999999999998
Wynika to z architektury komputerów, że nie da się zapisać 0,1 w skończonym ciągu binarnym... powstaje 0.1​=0.0001100110011001100... dla liczby FLOAT jest przewidziane 53 bity precyzji w mantysie.

import decimal
a = decimal.Decimal(0,3)
b=decimal.Decimal(0,1)
a-b=0.1999999999999999833466546306

więc zapisuje się liczby zmiennoprzecinkowe za pomocą STRINGA - '0,3'

```
from decimal import Decimal
a = Decimal('0,3')
b = Decimal('0,1')
a-b=0,2
```
