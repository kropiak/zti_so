## Skrypty w powłoce Bash

### Zadania, część 3 - polecenie test oraz instrukcja warunkowa

**Zadanie 1**  
Stwórz skrypt, który przyjmuje dwa parametry wejściowe, które powinny być ścieżką do plików. Jeżeli ścieżki wskazują na pliki wyświetl komunikat, który poinformuje, który plik został zmodyfikowany wcześniej (jest "starszy").

**Zadanie 2**  
Napisz skrypt, który będzie pozwalał na wyszukiwanie plików lub ich zawartości w poszukiwaniu określonego słowa. Za pomocą `read` skrypt najpierw pobiera ścieżkę a następnie frazę do wyszukania. Jeżeli ścieżka prowadzi do istniejącego pliku to przeszukana zostanie jego zawartość w poszukiwaniu frazy a jeżeli ścieżka prowadzi do folderu to przeszukana zostanie lista nazw plików i folderów w poszukiwaniu zadanej frazy.

**Zadanie 3**  
Napisz skrypt o nazwie `sortuj.sh`, do którego przekazywany będzie parametr w postaci ścieżki do pliku. Następnie poprzez komunikat na konsoli i polecenie `read` wczytana zostanie wartość zmiennej, która będzie odwpowiadała za określenie kierunku sortowania. Jeżeli podamy wartość `rosnąco` dane zpliku zostaną posortowane rosnąco - wartość `malejąco` posortują dane malejąco. Dane zapisz w posortowanej formie w tym samym pliku.

**Zadanie 4**  
Napisz skrypt, który będzie oczekiwał (poprzez read) na podanie nazwy pliku, którego zawartość ma zostać skopiowana a następnie będzie ją kopiował do wskazanego pliku.
Zacznij od wyświetlenia komunikatu:
"Podaj nazwę pliku, którego zawartość chcesz skopiować."
Następnie porzez `read` pobierz ścieżkę do pliku. Sprawdź czy plik istnieje i jeżeli tak to wyświetl monit o podanie ścieżki do pliku docelowego. Sprawdź czy plik istnieje oraz czy posiadasz prawo do zapisu. Stwórz plik lub dopisz do niego zawartość jeżeli plik już istnieje i posiadasz prawo do zapisu.