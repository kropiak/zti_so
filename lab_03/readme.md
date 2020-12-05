# Lab 3 - Potokowanie poleceń. Wyrażenia regularne.

## 1. Uruchamianie zadań w tle.

Jedną z wad pracy na jednym oknie konsoli jest konieczność uruchamiania poleceń po zakończeniu wykonania innej. Przynajmniej początkującym użytkownikom tak może się wydawać. Otóż istnieje mechanizm, który pozwala w prosty sposób uruchamiać zadania w tle i przełączać się między nimi.

Przykład uruchomienia zadania w tle:
```console
$ nano plik.txt &
[1] 7009
```

Pierwsza linia to wprowadzona przez nas komenda, która wygląda standardowo poza znakiem $ na końcu. Ten znak mówi powłoce, że zadanie ma być uruchomione w tle. Linia druga to wynik zwrócony przez powłokę informujący nas o numerze naszego zadania [1] oraz numerze identyfikacyjnym (PID) procesu w systemie 7009. Obie te liczby mogą się przydać.
Po wykonaniu powyższej metody efekt jego działania, który normalnie powinien pojawić się w ekranie konsoli nie jest widoczny. Aby przywołać zadanie uruchomione w tle należy wykonać polecenie:
```console
$ %1
```
gdzie % informuje powłokę, iż mamy do czynienia z mechanizmem przywołania zadania a 1 to numer zadania do przywołania.
Innym poleceniem, które przywołuje zadanie w tle jest komenda `fg` (ang. foreground). Analogicznie aby zadanie ponownie umieścić w tle używamy `bg` (ang. backgroud).
Innym sposobem jest wybranie CTRL + Z podczas pracy jakiegoś zadania (np. w oknie edytora nano) co spowoduje wyświetlenie linii podobnej do:
```console
[2]+ Stopped nano plik.txt
```
[2] to numer zadania, które ponownie możemy przywrócić.
Żeby samodzielnie zakończyć zadanie w tle możemy skorzystać z komendy:
```console
kill %1
lub
kill 7009
```
W pierwszym przypadku podejmujemy próbę zakończenia zadania w tle o numerze 1 a w kolejnym procesu o identyfikatorze 7009.


## 2. Potoki i polecenia

Potokowanie poleceń podobnie jak przekierowanie standardowego strumienia np. do zewnętrznego pliku pozwala na przekazanie wyników polecenia do innego polecenia jako potoku wejściowego. Oznacza to możliwość wykonania łańcucha poleceń gdzie wynik poprzedniego przekazywany jest do kolejnego, aż do ostatniego polecenia. Jednym z najczęstrzych przypadków jest wykorzystanie polecenia `grep` (Global Regular Expression Print), które przeszukuje ciąg tekstowy w poszukiwaniu wskazanych wartości określonych przez wzorzec. Na początek jednak przykład z poleceniem `wc` (ang. word count).

```console
$ ls | wc -l
```

Polecenie to przekaże wynik działania komendy `ls` jako wejście do komendy `wc -l`, które to zlicza ilość linii w przekazanym strumieniu.
Taki wynik możemy oczywiście przekierować do zewnętrznego zasobu, np. pliku.

```console
$ ls | wc-l > ile_zasobow.txt
```

Jeżeli potrzebujemy jednocześnie wyświetlić wartość, która została przekierowana do pliku użyjemy polecenia `tee`.

```console
$ ls | wc -l | tee pliczek.txt
# lub z dopisaniem do pliku
$ ls | wc -l | tee -a pliczek.txt
```
Zwróćmy uwagę, że wyświetlana jest wartość wyjściowa ostatniego polecenia, czyli polecenia `wc -l`.

Aby efektywnie korzystać z potokowania przy wyszukiwaniu zasobów w systemie należy poznać trochę bliżej tematykę wyrażeń regularnych. W lab 01 przedstawione zostały już podstawowe składowe oraz kilka przykładów. Dla przypomnienia:

`.` – kropka zastępuje dowolny jeden znak tekstu  
`[abc]` – oznacza wystąpienie dowolnego znaku spośród podanych – a,b lub c - wielokrotnie i w różnych kombinacjach  
`[a-z]` – oznacza dowolny znak z zakresu od a do z  
`[^]` – dopełnienie zbioru np.: `[^aA]` – oznacza dowolny znak nie będący a lub A, natomiast  
`[^a-z]` - oznacza dowolny znak różny od a do z  
`^` – takie oznaczenie będzie znaczyło 'zaczyna się od', np.  
`^po` - oznacza zaczyna się od liter `po`.  
`$` – oznacza "kończy się na", np. `ski$` - kończy się na `ski`.  
`\` - interpretuje kolejny znak dosłownie, np. '\\.' szuka kropki zamiast dowolnego znaku  

To nie są jeszcze wszystkie możliwości jakimi dysponujemy, ale rozpatrzmy kilka przykładów z wykorzystaniem powyższych elementów.

Polecenie 
```console
$ ls | grep '[AOE]'
```
dopasuje pliki, których nazwa zawiera A,O lub E.
```console
$ ls | grep '^[AOE]'
```
dopasuje wszystkie pliki, których nazwa rozpoczyna się od A,O lub E.

```console
$ grep '[AOE]la' imiona.txt
```
znajdzie wszystkie linie w pliku imiona.txt, które zawierają słowa Ala, Ola lub Ela. Również takie, których powyższe wartości są częścią, np. Olaf. Aby ograniczyć wyszukiwanie tylko do pełnych wyrazów dodajemy opcję `-w` do polecenia `grep`.

```console
$ grep -w '[AOE]la' imiona.txt
# wyniki możemy również posortować potokując do polecenia sort
$ grep '[AOE]la' imiona.txt | sort
```

Opcja `-w` dla polecenia `grep` powoduje, że szukane są słowa, które w całości pasują do wzorca a nie tylko go zawierają. 

Polecenie `sort` posiada również przydatne opcje (sprawdź w manualu) a między innymi:
* `-o` (output) - zapisanie wyniku posortowania w pliku o podanej nazwie
* `-r` (reverse) - odwraca rezultat porównania  
* `-u` (unique) - usunięcie duplikatów linii

Łącząc teraz narzędzia `grep` i `sort` możemy znaleźć wszystkie unikalne wartości w pliku, które spełniają warunek, posortować je oraz policzyć.
```console
$ grep '[AOE]la' imiona.txt | sort | wc -l
```

Samo polecenie `grep` również umożliwia zliczenie odnalezionych dopasowań, ale użycie go z potokowaniem do polecenia sort nie sprawdzi się w tym przypadku.

Poniższe polecenie wyszuka nazwy wszystkich plików w katalogu `/var/log`, których nazwa zawiera dowolne cyfry. Równoważnie możemy użyć polecenia:
```console
$ ls /var/log | grep '[0-9]'
```

Dzięki wykorzystaniu negacji możemy wyszukiwać dopełnień zbioru określonego przez wzorzec.
```console
$ ls /var/log | grep '[^0-9]'
```
Jednak to polecenie może dla niektórych działać niezgodnie z oczekiwaniami. Otóż wróćmy do przykładu poprzedniego `$ ls /var/log | grep [0-9]` - oznacza wyszukaj wszystkie wartości, które na dowolnej pozycji posiadają jedną z wartości - tu cyfrę. A teraz tłumacząc bieżące polecenie możemy powiedzieć "wyszukaj wszystkie wartości, które na dowolnej pozycji nie zawierają cyfry". Polecenie działa więc jak należy.
Żeby wyświetlić jednak wartości, które nie zawierają cyfr możemy użyć opcji `-v` polecenia `grep`:
```console
$ ls /var/log | grep -v '[0-9]'
```

Takim zapisem możemy również wyszukać wartości, które posiadają dwie cyfry na sąsiadujących pozycjach:
```console
$ ls /var/log | grep '[0-9][0-9]'
```

Są jednak specjalne znaczniki, które służą do określania liczności wystąpień danej pozycji wzorca. Można je również znaleźć w manualu polecenia `grep`: http://manpages.ubuntu.com/manpages/focal/pl/man1/grep.1.html.

Warto również wspomnieć w tym miejscu o rozszerzonej wersji wyrażeń regularnych, których krótki opis można znaleźć pod adresem: https://en.wikipedia.org/wiki/Regular_expression#POSIX_extended oraz w manualu polecenia `grep` oraz `regex`.

|Znacznik|Opis|
|---|---|
|?|      Poprzedzający element jest opcjonalny i dopasowany co najwyżej jeden raz.
|*  |    Poprzedzający element będzie dopasowany zero lub więcej razy. Nazywane dopełnieniem Kleene'a.
|+  |    Poprzedzający element będzie dopasowany jeden lub więcej razy.
|{n} |   Poprzedzający element pasuje dokładnie n razy.
|{n,} |  Poprzedzający element pasuje n lub więcej razy.
|{,m}  | Poprzedzający element pasuje co najwyżej m razy. Jest to rozszerzenie GNU.
|{n,m}  |Poprzedzający element pasuje co najmniej n razy, ale nie więcej niż m razy.

**Przykłady**
```console
# możemy zastąpić wyszukanie ciągu dwucyfrowego z jednego z przykładów powyżej zapisem
# tu użycie opcji -E jest już konieczne
$ ls | grep -E '[[:digit:]]{2}'
```

Z kolei opcja `-x` działa podobnie, ale powoduje zastosowanie wzorca dla całej linii - to oznacza, że zapis:
```bash
$ ls | grep -x '[a-z]\+'
$ ls | grep -x '[a-z0-9]\+'
```

wypisze tylko linie, które składaja się z co najmniej jednaj małej litery (lub więcej). Kolejny przykład to tylko małe litery i cyfry.

Do rozszerzonej wersji wyrażeń należą między innymi klasy znaków.

| Klasa | Dopasowywane znaki |
|---|---|
[:alnum:] | Znaki alfanumeryczne
[:alpha:] | Znaki alfabetyczne
[:blank:] | Znaki spacji i tabulacji
[:cntrl:] | Znaki sterujące
[:digit:] | Znaki numeryczne
[:punct:] | Znaki przestankowe

Należy pamiętać o użyciu podwójnych nawiasów kwadratowych, np.:
```console
$ ls /var/log | grep '[[:digit:]]'
# z jawnym użyciem opcji -E - extended expression
$ ls /var/log | grep -E '[[:digit:]]'
```

Operator `|` uzywany jest jako wyrażenie `OR` dzięki czemu możemy:
```console
$ ls | grep -wE 'Ola|Ala'
```
wyświetlić wszystkie wartości, które zawierają słowo Ola lub Ala.

Dołożenie nawiasów `()` pozwala na grupwanie wzorców i używanie ich w następujacy sposób:
* wzorzec 'Fizy(k|cy)' oznacza Fizyk i Fizycy
* 'pi[wk]o' oznacza wzorce piwo i piko

Pamiętajmy również o tym, że możemy wartość zwróconą przez `grep` ponownie potokowac do polecenia `grep`.




**Zadania**  

1. Wyświetl wszystkie nazwy zasobów ze swojego folderu domowego, których nazwa rozpoczyna się od wielkiej litery.
2. Ze swojego folderu domowego wyświetl wszystkie zasoby, których nazwa rozpoczyna się od znaku '.' (kropka).
3. Zlicz wszystkie pliki z zadania 2.
4. Wyświetl wszystkie nazwy ze swojego folderu domowego, które zawierają tylko duże i małe litery.
5. Wyświetl wszystkie nazwy ze swojego folderu domowego, które nie zawierają wielkich liter.
6. Wyświetl wszystkie nazwy ze swojego folderu domowego, które nie zawierają 3 literowego rozszerzenia.
7. W folderze /etc znajdź wszystkie nazwy, które rozpoczynają się od 'c' a kończą na 'y'.
8. W folderze /etc odnajdź wszystkie nazwy, które posiadaja dwie sąsiadujące ze sobą litery 's'.
9. Zbuduj wyrażenie, które wybierze tylko nazwy, które zawierają ciągi znaków zaczynające się literą w lub W, następnie dwa dowolne znaki, potem znak dolara ($) i dalej dowolne znaki aż do końca linii.
10. Zbuduj wyrażenie, które wybierze nazwy, które zawierają dowolny ciąg znaków, rozpoczynający się samogłoską.
11. Zbuduj wyrażenie, które wybierze nazwy, które zawierają sześcioliterowy ciąg znaków, z których pierwszy, trzeci i piąty znak może być dowolny, drugi i czwarty musi być wielką literą, szósty musi być literą e.
12. Z folderu domowego wyświetl wszystkie nazwy, które mają dokładnie 4 znaki długości (małe i wielkie litery oraz cyfry).
13. Z folderu `/var/log` wyświetl wszystkie nazwy, które mają rozszerzenie 'log'.
14. Wyświetl linie oraz ich numery przeszukując plik `/var/log/auth.log.1` w poszukiwaniu swojej nazwy użytkownika.
15. To samo co w zadaniu poprzednim, ale przeszukując również podfoldery.
16. Z dołączenego pliku 'imiona.txt' wyświetl wszystkie imiona rozpoczynające się literami od A do K.
17. Z pliku 'imiona.txt' wyświetl tylko te imiona, których nazwa ma 4 lub 5 liter długości.
18. Z pliku 'imiona.txt' policz ile jest unikalnych imion kończących się literą 'a'.
19. Z pliku `/etc/passwd` wyświetl wszystkie linie, których użytkownicy posiadają uid co najmniej 4-cyfrowy. *
20. Z pliku `/etc/shadow` wyświetl wszystkie linie, gdzie użytkownik ma nieważne hasło (patrz lab 02). *
21. Poleceniem wget pobierz zawartość strony www.onet.pl (tylko tej) i następnie poleceniem grep wyszukaja wszystkie linki i zapisz je do pliku o nazwie 'linki.txt'. *
