### Projekt: System rezerwacji stadionowej. 

Dla celów rozwojowych w Coders School jako zadanie poboczne.
Szkic v0.1

Załóżmy ze mamy stadion z trybunami rozmieszczonymi na 3 piętrach. Na każdym pietrze jest 5 rzędów po 30 krzesełek. W tym w pierwszym rzędach każdego poziomu mamy miejsca VIP (od 10 do 20) a na skrajnych 5 krzesłach drugich rzędów każdego piętra miejsca dla niepełnosprawnych. Reszta to miejsca standard. Celem projektu jest stworzenia systemu rezerwacji miejsc z graficzna interpretacja (w konsoli ;)).
Projekt podzieliłem na moduły i rozpocząłem wstępne szkice jak mniej więcej mogłoby to wygadać.


##Moduł menu## - główna pętla programu przyjmująca od użytkownika dane wejściowe operacji np. 
- 1. Rezerwuj miejsce, 
- 2. Anuluj rezerwacje, 
- 3. Zamień miejscami, 
- 4. Pokaz tylko wolne miejsca, 
- 4. Wyszukaj miejsce po personaliach 
- 5. Znajdź osobę 
- 6. Import danych z pliku 
- 7. Zapis do pliku. 
- W module tym należało by zaprogramować system interakcji który będzie reagował na polecone mu zadania i odsyłał do odpowiednich funkcji. System powinien być odporny na niepoprawne dane np. Stringi zamiast liczby oraz zwracać komunikat błędu gdy zostanie podana liczba która nie ma przypisanej operacji. Po każdej wykonanej akcji powinno zostać wykonane przeładowanie widoku w klasie stadium.

##Moduł stadium## - Główny obiekt zarządzający stadionem. Jego najważniejsza funkcja jest graficzne przedstawienie wolnych (reloadView) , zajętych miejsc oznaczonych odpowiednimi standardami. V - miejsce VIP, D - miejsce dla niepełnosprawnych, S - miejsce standard. Miejsca wolne powinny być wyświetlane jako objęte kwadratowymi nawiasami za to zajęte przez : .
Np. [S] [S] :S: [V] :V: :V: [V]

Jako składniki obiekt powinien mieć
- spis osób (klasy Person) 
- rozkład wszystkich miejsc na stadionie (pochodnych klasy seat zgodnie ze standardem danego miejsca). Implementacja powinna zawierać trzy vectory zawierane w sobie które domyślnie zapełniają się wolnymi obiektami klas zgodnie z przyjętym schematem. Kazde miejsce powinno być ponumerowane zaczynajac od 1 (zawierać swoje ID).
- Tzw. Black list czyli listę osób z zakazem stadionowym które nie mogą dokonać rezerwacji miejsca.
Dodatkowo jest do zaimplementowania funkcjonalność backupu - odczytu i zapisu danych do pliku który automatycznie wypełni miejsca według przypisanych do personaliów miejsc z pliku (parsowanie dowolne ;) może być JSON ale tez własne jeżeli ktoś ma już stworzony jakiś ciekawy parser i podłączy go do repozyitorium jako submodule). W dalszych wersjach rozwoju numer paszportu powinien być przechowywany w pliku jako zahashowany. 

##Moduł seat## - Klasa seat powinna być dziedziczona dla poszczególnych typów siedzien i domyślnie mieć inne cechy jak nazwa i cena miejsca. 
Dla klasy VIP cena to 1000zl, dla klasy Disabled cena to 250zl, a dla klasy Standard cena to 370zl. Siedzenia powinny być łączone funkcja reserveSeat() z dana osoba i wpisywać do składnika TicketOwner tylko wskaźnik do danej osoby (obiektu Person z listy osób). Funkcja ta powinna zwracać powodzenie lub porażkę takowej akcji. Np. False gdy osoba jest na czarnej liście lub nie była szczepiona na COVID. Po wykonaniu takowej operacji obiekt stadium powinien wykonać przeładowanie widoku by pokazać czy coś się zmieniło czy nie. W celu sprawdzenia czy miejsce jest zajęte czy nie funkcja isBusy() powinna zwracać prawdę jeżeli miejsce jest zajęte i fałsz jeżeli jest wolne (sprawdzenie czy TicketOwner zawiera jakiś niepusty wskaźnik)

##Moduł Person## - obiekt powinien zawierać imię, nazwisko, liczbę posiadanych miejsc, nr telefonu, e-mail, numer paszportu Polsatu (dla przykładu zamiast numeru pesel) oraz czy dany człowiek był szczepiony na COVID. 


Wstępna wersja demonstracyjna obejmuje ubogą implementacje do rozbudowy. 
