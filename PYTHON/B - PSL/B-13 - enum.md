Moduł enum to narzędzie do tworzenia enumeracji — czyli zestawów nazwanych, stałych wartości. Idealne, kiedy coś ma mieć ograniczoną, z góry zdefiniowaną liczbę wariantów: role użytkownika, statusy zamówienia, typy zwierząt, kierunki ruchu w grze, itd.

To lepsze niż gołe stringi czy liczby, bo:
- nie da się przypadkiem stworzyć nieistniejącej wartości,
- porównania są bezpieczniejsze,
- kod jest czytelniejszy,
- IDE daje podpowiedzi.

## Tworzenie enumeracji

```python
from enum import Enum

class Animal(Enum):
    DOG = 1
    CAT = 2
    HORSE = 3
```

## Dostęp do nazwy i wartości

```python
Animal.DOG.name      # 'DOG'
Animal.DOG.value     # 1
```

## Iteracja

```python
for a in Animal:
    print(a)
```

## Tworzenie z wartości

```python
Animal(1)    # Animal.DOG
```

## Auto-wartości

```python
from enum import Enum, auto

class Status(Enum):
    NEW = auto()
    IN_PROGRESS = auto()
    DONE = auto()
```

## IntEnum (enum działający jak int)

```python
from enum import IntEnum

class Level(IntEnum):
    LOW = 1
    MEDIUM = 2
    HIGH = 3
```

## Flag / IntFlag (wartości bitowe)

```python
from enum import IntFlag

class Permission(IntFlag):
    READ = 1
    WRITE = 2
    EXECUTE = 4

p = Permission.READ | Permission.WRITE
```

## Przykład użycia w modelu danych

```python
class Health(Enum):
    HEALTHY = "healthy"
    SICK = "sick"

dog.status = Health.HEALTHY
dog.status.value   # "healthy"
```