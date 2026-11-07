# Linux
$ cd -                              wraca z powrotem do ostatniego folderu
$ ctrl+R / ctrl+G          wyszukiwanie wsteczne komend / przerywanie wyszukiwania
$ du -sh * | sort -h       disk usgae, pokazuje co zajmuje dysk
$ df -h                            disk free, pokazuje ile zostało miejsca
$ ss                          Socket Statistics, dawniej netstat
    -t                     TCP
    -u                    UDP
    -l                      Listening
    -p                    Processess-działa tylko z sudo, pokazuje nazwę programu
    -n                    Numeric, numer portu zamiast nazwy
$ grep "co szukamy" /gdzie/szukamy/nazwa_pliku.abc
    -A3                 After 3 linijki
    -B3                 Before 3 linijki
    -C3                 Context 3 linijki
    -l                     List nazw plików
    -r                    rekurencyjnie (wgłąb katalogów)
    -i                     Ignore wielkość liter
    -w                   Word dokładnie słowo
    -v                    odwrotnie




rsync -avz src/ user@host:/dest     - kopiowanie plików na zdalny serwer
    -a (archive)
    -v (verbose-tryb gadatliwy, o wszystkim informuje)
    -z (compress-w locie)
