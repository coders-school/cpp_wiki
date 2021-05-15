### Projekt: System rezerwacji stadionowej. ###

Dla celów rozwojowych w Coders School jako zadanie poboczne.
Szkic v0.14c.

Załóżmy ze mamy stadion z trybunami rozmieszczonymi na 3 piętrach. Na każdym pietrze jest 5 rzędów po 30 krzesełek. W tym w pierwszym rzędach każdego poziomu mamy miejsca VIP (od 10 do 20) a na skrajnych 5 krzesłach drugich rzędów każdego piętra miejsca dla niepełnosprawnych. Reszta to miejsca standard. Celem projektu jest stworzenia systemu rezerwacji miejsc z graficzna interpretacja (w konsoli ;)).
Projekt podzieliłem na moduły i rozpocząłem wstępne szkice jak mniej więcej mogłoby to wyglądać.


### Moduł menu
Główna pętla programu przyjmująca od użytkownika dane wejściowe operacji np. 
- 1. Rezerwuj miejsce, 
- 2. Anuluj rezerwacje, 
- 3. Zamień miejscami, 
- 4. Rezerwuj następne wolne miejsce, 
- 4. Wyszukaj miejsce po personaliach,
- 5. Znajdź osobę,
- 6. Import danych z pliku,
- 7. Zapis do pliku,
- 8. Wyjście z programu

W module tym należało by zaprogramować system interakcji który będzie reagował na polecone mu zadania i odsyłał do odpowiednich funkcji. System powinien być odporny na niepoprawne dane np. Stringi zamiast liczby oraz zwracać komunikat błędu gdy zostanie podana liczba która nie ma przypisanej operacji. Po każdej wykonanej akcji powinno zostać wykonane przeładowanie widoku za pomocą funkcji reloadView().

Funkcje od 1-3 powinny mieć możliwość wskazania danego siedzenia za pomocą dwóch opcji: 
- unikalny ID siedzenia
- współrzędne siedzenia

oraz po uruchomieniu takiej operacji wyświetlić graficznie o jakie miejsce chodzi i zapytać o potwierdzenie.

Np. 
>

     Czy anulować rezerwacje miejsca o numerze ID 5 (2,2). 
     [S] | [S]
      - {S} -
     [S] | [S]

Pamiętamy ze numerujemy wszystko od 1 ;)

Funkcja reloadView() to graficzne przedstawienie wolnych, zajętych miejsc oznaczonych odpowiednimi standardami. V - miejsce VIP, D - miejsce dla niepełnosprawnych, S - miejsce standard. Miejsca wolne powinny być wyświetlane jako objęte kwadratowymi nawiasami za to zajęte przez : 
Np.
> 
     
     [S] [S] [S] [S] [S] [S] [S]
     [S] [S] :S: [V] :V: :V: [V]

Dodatkowo powinna wyświetlać ilość zajętych miejsc, ilość wolnych miejsc sumarycznie ogółem oraz osobno dla każdej klasy.

### Moduł stadium
 Główny obiekt zarządzający stadionem. Powinien zawierać najważniejsze funkcje i być całkowicie rozłączny aby w miarę potrzeby można go przenieść do innego projektu np. Podmienić konsole na GUI.

Jako składniki obiekt powinien mieć
- spis osób (klasy Person) 
- rozkład wszystkich miejsc na stadionie (pochodnych klasy seat zgodnie ze standardem danego miejsca). Implementacja powinna zawierać trzy vectory zawierane w sobie które domyślnie zapełniają się wolnymi obiektami(smart pointerami) klas pochodnych zgodnie z przyjętym schematem. Kazde miejsce powinno być ponumerowane zaczynajac od 1 (zawierać swoje ID).
- Tzw. Black list czyli listę osób z zakazem stadionowym które nie mogą dokonać rezerwacji miejsca.
- Mapę pozwalająca zdefiniować ceny dla danej klasy miejsc.

Dodatkowo jest do zaimplementowania funkcjonalność backupu - odczytu i zapisu danych do pliku który automatycznie wypełni miejsca według przypisanych do personaliów miejsc z pliku (parsowanie dowolne ;) może być JSON ale tez własne jeżeli ktoś ma już stworzony jakiś ciekawy parser i podłączy go do repozyitorium jako submodule). W dalszych wersjach rozwoju numer paszportu powinien być przechowywany w pliku jako zahashowany. Ewentualnie całkowita migracja do bazy danych SQL lub mongoDB ;)

### Moduł seat
Klasa seat powinna być dziedziczona dla poszczególnych typów siedzien i domyślnie mieć inne cechy jak nazwa i cena miejsca. 
Dla klasy VIP cena to 1000zl, dla klasy Disabled cena to 250zl, a dla klasy Standard cena to 370zl. Siedzenia powinny być łączone funkcja reserveSeat() z dana osoba i wpisywać do składnika TicketOwner tylko wskaźnik do danej osoby (obiektu Person z listy osób). Funkcja ta powinna zwracać powodzenie lub porażkę takowej akcji. Np. False gdy osoba jest na czarnej liście, nie była szczepiona na COVID lub nie ma skończonych 18 lat. Po wykonaniu takowej operacji obiekt menu powinien wykonać przeładowanie widoku by pokazać czy coś się zmieniło czy nie. W celu sprawdzenia czy miejsce jest zajęte czy nie funkcja isBusy() powinna zwracać prawdę jeżeli miejsce jest zajęte i fałsz jeżeli jest wolne (sprawdzenie czy TicketOwner zawiera jakiś niepusty wskaźnik)


### Moduł Person
Obiekt powinien zawierać imię, nazwisko, liczbę posiadanych miejsc, nr telefonu, e-mail, numer dokumentu w formacie np. MXM900303 (dla przykładu zamiast numeru pesel. Numer po literce to data urodzenia) oraz czy dany człowiek był szczepiony na COVID. Funkcja getAge() powinna zwracać wiek osoby na podstawie numeru paszportu.
 
### Moduł Log
Obiekt powinien logować wszystkie wydarzenia wykonane przez system i dopisywać to do pliku o nazwie z aktualną datą w folderze logs/

### Podsumowanie
Wstępna wersja demonstracyjna obejmuje ubogą implementacje do rozbudowy. 

## Link do kodu:
https://github.com/piotrku91/stadium-system-res

### Zapraszam do forkowania, dyskusji i rozbudowy ;)
