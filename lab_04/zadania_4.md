## Skrypty w powłoce Bash

### Zadania, część 4 - pętle oraz instrukcja select

**Zadanie 1**  
Napisz skrypt, który będzie symulował zgadywankę liczbową. Niech wartość odgadywanej liczby będzie zapisana na stałe w skrypcie. Jeżeli użytkownik poda zbyt małą lub dużą liczbę wyświetlaj stosowny komunikat. Jeżeli liczba zostanie odgadnięta również wyświetl tę informację i zakończ działanie skryptu.

**Zadanie 2**  
Napisz skrypt, który będzie przyjmował z klawiatury kolejne liczby (nie musisz na razie sprawdzać czy faktycznie są to liczby) dopóki nie zostanie wpisana litera `Q` lub `q`. Po jej wpisaniu wyświetl ile było tych liczb oraz jaka jest ich średnia z dokładnością do 2-óch miejsc po przecinku.

**Zadanie 3**  
Zadanie polega na stworzeniu prostego skryptu backupu (wykorzystamy jego zmodyfikowaną formę później). 
1. Skrypt nazywamy `backup.sh` i umieszczamy w podfolderze `bin` w głównym katalogu użytkownika (należy stworzyć folder, jeżeli go nie ma). 
2. Sprawdź w pliku ~/.profile czy istnieje odpowiednia instrukcja dodająca folder `bin` do zmiennej `PATH`. 
3. Uruchomienie skryptu z parametrem add i ścieżką do folderu/pliku (np. `backup add ~/skrypty/) doda wpis (linię) do pliku `backup_sources.conf`, który powinien znajdować się w tym samym folderze co skrypt backup.sh.
4. Niech 'kopie' będą umieszczane w folderze backup w głównym katalogu użytkownika.
5. Uruchomienie skryptu `backup.sh` powoduje stworzenie nowego folderu w folderze backup o nazwie równoważnej dacie jego uruchomienia w formacie RRRR-MM-DD_HH:MM:SS.
6. Jeżeli na liście znajduje się folder to kopiuj całą jego zawartość.