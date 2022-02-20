# Lab 5 - kilka słów o manadżerze pakietów, narzędziu cron oraz zdalnym dostępie do hosta

**Ćwiczenie 1**  

Instalacja narzędzia webmin i krótka prezentacja jego możliwości.

Oficjalna strona projektu znajduje sie pod adresem http://www.webmin.com, a do instalacji wykorzystamy wpis w Webmin Wiki -> http://doxfer.webmin.com/Webmin/Installation.

**Ćwiczenie 2**  

W tym ćwiczeniu poznamy czym jest narzędzie cron i w jaki sposób dodać nowe zadanie do automatycznego uruchamiania.
Podręcznik online dla tego narzędzia znajduje się pod adresem http://manpages.ubuntu.com/manpages/focal/pl/man1/crontab.1.html

Poniżej kilka głównych parametrów polecenia cron:
* **crontab -e** – edycja pliku,
* **crontab -l** – wyświetlenie pliku,
* **crontab -r** – usunięcie pliku,
* **crontab -u user {-e|-l-r}** – edycja pliku zadanego użytkownika – wywołanie tylko
dla administratora,
* **crontab plik** – użycie gotowego pliku jako crontab.

W folderze `etc` systemu plików znajdziemy foldery `cron.daily`, `cron.hourly` itp., które pozwalają na umieszczenie skryptu, który będzie uruchamiany w odpowiednim interwale zamiast dodawać wpis do crontaba. Jednak taki skrypt musi być wykonywalny oraz jego nazwa musi pasować do wyrażenia `(^[a-zA-Z0-9_-]+$)` (m.in. brak rozszerzenia pliku).

Wpisy w pliku rozpoczynają się od podania harmonogramu a następnie (opcjonalnie) użytkownika i polecenia/skryptu do wykonania.

**Przykładowy wpis**
```bash
# uruchomienie skryptu co minutę każdej godziny każdego dnia każdego tygodnia i każdego miesiąca - w skrócie co minutę

* * * * * /home/kropiak/skrypt.sh
```

Domyślnie cron loguje wszystkie zdarzenia do pliku `/var/log/syslog`, więc możemy wyszukać odpowiednie wpisy narzędziem grep:
```bash
grep CRON /var/log/syslog
```

**Zakresy wartości dla każdej składowej**:  
[minuty (0-59)]  [godzina(0-23)] [dzień_miesiąca (0-31)] [miesiąc (1-12)] [dzień_tygodnia (0-7, 0 i 7 -niedziela)] [polecenie]

Możemy stosować:

`*` – zawsze (każda minuta, każda godzina, każdy dzień, każdy miesiąc, każdy dzień tygodnia)

**Zakres: x-y** – czyli od „x” do „y”, np. „1-5” to każda minuta/godzina/dzień/miesiąc od 1 do 5

**Przerwa**: */x – np. „*/8” to polecenie wykonane co 8 minut/godzin/dni…

**Kolejne wartości:** x,y,z – np. „2,5,8” może oznaczać polecenie wykonane w 2, 5 i 8 minucie/godzinie…

Wiele zadań w jednym wpisie: ; rozdzielamy polecenia, np.  
`* * * * * skrypt/polecenie1; skrypt/polecenie2`

Kilka przykładów, by lepiej zrozumieć zasadę budowy:

`*/5 * * * *` [Co 5 minut]  
`0 4 * * *` [O 4 rano]  
`0 5 * * 0` [O 5 rano w niedzielę (0 lub 7)]  
`0 8,10 10-20 3 *` [O 8:00 i 10:00, od 10 do 20 marca]  


Jeżeli zrozumienie mechanizmu określania częstotliwości wykonania jest wciąż trudne można posłużyć się dużą ilością gotowych przykładów dostępnych pod adresem https://crontab.guru/examples.html.

Kilka ciekawych przykładów wpisów do crontab: https://tecadmin.net/crontab-in-linux-with-20-examples-of-cron-schedule/


**Ćwiczenie 3**  

Podłączenie się do serwera poprzez SSH z klienta Putty oraz WinSCP.

**Ćwiczenie 4**

Instalacja serwera MySQL oraz dostęp do bazy poprzez tunelowanie portu poprzez putty.