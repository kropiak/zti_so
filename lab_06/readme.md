# Lab 6. Zarządzanie usługami. Zaliczenie przedmiotu.

## **0. Zaliczenie przedmiotu.**

* ### **0.1. Napisz skrypt według poniższych wymagań.**

Napisz skrypt, który po uruchomieniu będzie wyświetlał menu z opcjami do wyboru postaci: 

Wybierz figurę, której pole powierzchni chcesz obliczyć:  
[1] kwadrat  
[2] prostokąt  
[3] koło  
[q] wyjście z programu  

W zależności od wybranej figury użytkownik zostanie poproszony o podanie wartości dla jej parametrów – dla kwadratu, długość boku a, dla prostokąta długość boków a i b, dla koła promień. 
Po podaniu tych wartości skrypt obliczy pole powierzchni i ponownie wyświetli menu.

* ### **0.2 Test**
Wypełnij test dostępny pod adresem https://forms.gle/Jei8fC7QdvDY8Lck9


## **1. Na rozluźnienie :)**

https://www.tecmint.com/20-funny-commands-of-linux-or-linux-is-fun-in-terminal/
http://smashingtips.com/linux/cool-terminal-commands-for-linux


## **2. Zarządzanie usługami w systemie Linux.**

Usługi w systemie Ubuntu można uruchamiać i zatrzymywać przy użyciu kilku różnych poleceń powłoki. Aby wyświetlić listę dostępnych usług oraz ich status możemy użyć poniższego polecenia.

```bash
service --status-all
```

Obecnie zalecane jest aby w systemie Ubuntu używać polecenia `systemctl` ([manual](http://manpages.ubuntu.com/manpages/focal/en/man1/systemctl.1.html)) do zarządzania usługami systemu. Jest to narzędzie bazujące na narzędziu systemd ([manual](http://manpages.ubuntu.com/manpages/focal/en/man1/systemd.1.html)), które występuje w każdej dystrybucji systemu Linux.

Ogólna postać polecenia:
```bash
sudo systemctl [akcja] [nazwa usługi]
```

Przykłady użycia polecenia systemctl.
```bash
# wyświetlenie statusu usługi
sudo systemctl status ufw

# bez podania nazwy usługi wyświetla status całego systemu, można połączyć
# z opcją -all
sudo systemctl status [--all]

# zatrzymanie usługi
sudo systemctl stop ufw

# uruchomienie usługi
sudo systemctl start ufw

# restart usługi
sudo systemctl restart ufw

```

**Zadanie 1**  
Wyświetl listę wszystkich usług w systemie. Sprawdź czy firewall jest włączony. Jeżeli nie to uruchom tę usługę. Sprawdź na komputerze hoście czy możesz połączyć się usługą telnet do maszyny wirtualnej na porcie 80. Tutaj może być konieczne przełączenie sieci maszyny wirtualnej na sieć mostkowaną. Wyłącz usługę firewall i spróbuj jeszcze raz. Włącz firewall ponownie.

**Zadanie 2**  
Sprawdź czy możesz z komputera hosta połączyć się z narzędziem webmin poprzez przeglądarkę internetową (domyślnie port 10000). Posiłkując się poradnikiem ze strony https://www.arubacloud.pl/poradnik/jak-ustawic-i-skonfigurowac-firewall-ufw-na-ubuntu-18-04.aspx dodaj regułę, która pozwoli z adresu komputera hosta połączyć się z usługą webmin. Sprawdź zapisy w logu usługi firewall.


## **3. Dodanie i zamontowanie nowej partycji.**

**Krok 1** - Dodanie nowego wirtualnego dysku.  
Należy swtorzyć nowy wirtualny dysk twardy a następnie dodać go do kontrolera aktualnego wirtualnego dysku (z systemem operacyjnym). Robimy to poprzez wybór odpowiednich opcji we właściwościach maszyny wirtualnej. Dysk możemy dodać do kontrolera przy wyłączonej maszynie wirtualnej.

**Krok 2** - Stworzenie nowej partycji.  

Najpierw wyświetlamy listę partycji w systemie.
```bash
sudo fdisk -l
```

Tutaj może pojawić się wiele pozycji, których możemy nie potrzebować. Możemy ograniczyć widok do wybranych urządzeń.

```bash
sudo fdisk -l /dev/sda
```

W naszym systemie powinien pojawić się drugi dysk, prawdopodobnie jako urządzenie `/dev/sdb`.

Wybieramy więc nowy dysk.
```bash
sudo fdisk /dev/sdb
```

Warto zwrócić uwagę, że dopóki nie wybierzemy opcji `write` zmiany nie zostaną faktycznie zastosowane na dysku. Opcja `m` wyświetla przydatny help.

Dodajemy nową partycję wybierając opcję `n` i określajac kolejne wartości w kreatorze - domyslne opcje stworzą partycje na całym dostępnym obszarze. Ponownie możemy sprawdzić listę partycji na nowym dysku.

```bash
sudo fdisk -l /dev/sdb
```
**Krok 3** - Formatowanie partycji.

```bash
sudo mkfs -t ext4 /dev/sdb1
```

**Krok 4** - Zamontowanie partycji do istniejącego systemu plików.

Tworzymy punkt montowania.
```bash
sudo mkdir -p /dysk2
```

Montujemy partycję do punktu montowania.
```bash
sudo mount -t auto /dev/sdb1 /dysk2

# listę partycji i punktów montowania można wyświetlić poleceniem
df
# tutaj możemy również dowiedzieć się o wykorzystanej przestrzeni dyskowej
```


## **4. Reset hasła superużytkownika.**

Czasami zdarza się utracić dostęp do jedynego konta superużytkownika systemu. Jego zmiana nie jest jednak aż tak bardzo trudna.

**Krok 1: Bootowanie do Recovery Mode**

Zrestartuj system i tuż po uruchomieniu maszyny przytrzymaj klawisz `Shift`. Powinien pojawić się ekran z wyborem opcji logowania. Wybierz `opcje zaawansowane` i później (recovery mode).

**Krok 2: Uruchomienie powłoki**

Wybierz opcję `root` i wciśnij klawisz `Enter`.

**Krok 3: Zamontowanie systemu plików z prawem do zapisu**

W terminalu wpisujemy:
```bash
mount –o rw,remount /
```

**Krok 4: Zmiana hasła.**

```bash
passwd user

# i podajemy dwa razy nowe hasło
# pozostał juz tylko restart i sprawdzenie nowego hasła

shutdown –r
```


## **5. Co dalej ?**

Aby dalej rozwijać swoje kompetencje w zakresie pracy z sytemem Linux najlepiej jest robić to poprzez wdrażanie kolejnych usług.
Można to robić w ramach systemu Ubuntu, ale w przypadku usług typowo serwerowych proponuję wykorzystać dystrybucje, które zostały specjalnie do tego celu stworzone takie jak np. Debian, CentOS.

Wybrane usługi, które warto wdrożyć:
* udostępnianie zasobów poprzez narzędzie `samba` - tutaj można wypróbować również integrację z domeną Active Directory
* uruchomienie serwera WWW np. Apache, nginx.
* uruchomienie serwera bazy danych np. MySQL, PostgreSQL
* uruchomienie usługi SFTP
* konfiguracja serwera DNS np. bind
  
Moim zdaniem warto również dać szansę temu systemowi jako podstawowemu systemowi jeżeli planujemy pracę z programowaniem lub pracę z danymi (data science).



Pomysły:

* instalacja serwera MySQL i podłączenie z zewnątrz ?
* udostępnianie zasobów sieciowych ? (samba ? )
* dodatkowe zadania z wyrażeniami regularnymi ?
* dokończenie zadań związanych z kontami użytkowników i uprawnieniami ?
