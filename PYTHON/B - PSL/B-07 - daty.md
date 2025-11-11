`import datetime

`datetime.datetime.now()` - wiadomka, aktualny czas bardzo dokładny(klasa małą literą)
`datetime.date(today)` - 2025-08-26

TIMESTAMP [strona do konwertowania]([Epoch Converter - Unix Timestamp Converter](https://www.epochconverter.com/))
`datetime.datetime.fromtimestamp(1756239928) 

formatowanie daty do wyświetlenia
zmienna_daty.strftime("%H:%M:%S") - 21:37:00
zmienna_daty.strftime("%d-%m-%Y) - 29-08-2025
[jak zapisywać daty](https://docs.python.org/pl/3.13/library/datetime.html#format-codes)

zmiana stringa na datę:
datetime.strptime(zmienna_daty, format daty{na przykład "%d.%m.%Y})

Liczenie czasu wykonania kodu
`import time
`start_time = time.time()` - czas w danym momencie.
