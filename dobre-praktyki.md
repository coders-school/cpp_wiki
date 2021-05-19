# Dobre praktyki <!-- omit in toc -->

- [Zasady SOLID](#zasady-solid)
- [KISS](#kiss)
- [DRY](#dry)
- [YAGNI](#yagni)
- [Reguła 5](#reguła-5)
- [Reguła 0](#reguła-0)
- [Kolejność załączania plików nagłówkowych](#kolejność-załączania-plików-nagłówkowych)
- [Inne](#inne)

## Zasady SOLID

Stworzony przez Roberta C. Martin'a (uncle Bob) zestaw wytycznych które powinno stosować się podczas pisania programów w sposób obiektowy.

**S - Single responsibility**

* Klasa powinna mieć tylko jedną funkcjonalność, którą realizuje. Wszelkie modyfikacje klasy związane są z tą jedną konkretną funkcjonalnością.
* Przykładowo klasa `Journal` powinna pozwalać tworzyć i przeglądać notatki
* Chcąc dodać nową funkcjonalność, np. zapisywanie do pliku, należy stworzyć nową klasę `ZapisywanieDoPliku`. Pozwoli to na uniknięcie sytuacji, gdy trzeba zmienić sposób zapisu i nagle trzeba zmodyfikować wszystkie klasy, które mają taką funkcjonalność.

```cpp
class Journal {
public:
    Journal(const std::string& title) : title_(title) {}

    void addEntry(const string& entry) {
        entries_.push_back(entry);
    }

private:
    std::string title_;
    std::vector<std::string> entries_;
};
```

**O - Open/Close principle**

* Kod, który tworzymy powinien być “możliwy do rozszerzania i zamknięty na modyfikacje”, tzn. żeby możliwe było rozszerzanie funkcjonalności bez konieczności modyfikacji istniejącego kodu
* Rozszerzanie funkcjonalności klasy powinno opierać się na abstrakcji i polimorfiźmie

**L - zasada Barbary Liskov**

* W miejscu klasy bazowej zawsze można użyć dowolnej klasy pochodnej
    * Pełna zgodność interfejsu i wszystkich metod
    * Jeśli w wyniku dziedziczenia otrzymujemy cechy lub metody które są niezgodne z klasą pochodną to łamiemy tę zasadę
* Klasy pochodne nie powinny modyfikować metod z klas bazowych, ewentualnie mogą je rozszerzać
* Przykład niezgodności ilustruje klasyczny przykład Prostokąt - Kwadrat
  * Kwadrat re-definiuje settery (użycie któregokolwiek settera ustawia oba boki na tą samą wartość) w efekcie czego metoda obliczają

```cpp
class Rectangle {
protected:
    int width_, height_;

public:
    Rectangle(int width, int height) : width_(width), height_(height) {}

    int getWidth() const { return width_; }
    int getHeight() const { return height; }

    virtual void setWidth(int width) { width = width_; }
    virtual void setHeight(int height) { height = height_; }

    int area() { return width_ * height_; }
};

class Square : public Rectangle {
public:
    Square(int size): Rectangle(size, size) {}

    void setWidth(int width) override {
        width_ = width;
        height_ = width;
    }

    void setHeight(int height) override {
        height_ = height;
        width_ = height;
    }
};

void process(Rectangle& r) {
    int w = r.getWidth();
    r.setHeight = 10;

    std::cout << "Expected area = " << (w * 10) << ", got " << r.area() << std::endl;
}

int main() {
    Rectangle r{3, 4};
    process(r);     // prints "Expected area = 30, got 30"

    Square sq{5};
    process(r);     // prints "Expected 50, got 100"
}
```

**I - segregacja interfejsów**

* Powinno tworzyć się wiele małych interfejsów
* Klasa która implementuje interfejs nie może być zmuszana do implementowania metod, których nie potrzebuje

```cpp
struct IPrinter {
    virtual void print(Documen& doc) = 0;
};

struct IScanner {
    virtual void scan(Document& doc) = 0;
}

struct IFax {
    virtual void fan(Document& doc) = 0;
}

struct Fax : public Fax {
    /* ... */
}

struct IMultiFunctionMachine : public IPrinter, public IScanner, public IFax {}

struct Machine : public IMultiFunctionMachine {
    /* ... */
}
```

**D - zasada odwrócenia zależności**

* Wszystkie zależności powinny w jak największym stopniu zależeć od abstrakcji a nie od konkretnego typu.
* Należy stosować interfejs polimorficzny wszędzie tam gdzie jest to możliwe, szczególnie w parametrach funkcji
* Skutkiem tego jest fakt, że kod jest prostszy w rozbudowie i konserwacji

## KISS

Keep It Simple, Stupid - Zrób to prosto, idioto
* Zasada mówi, że nasz kod powinien być jak najprostszy w zapisie, bez skomplikowanych udziwnień, które zaciemniają jego zrozumienie.
* Chodzi tutaj nie tylko o sam sposób tworzenia jak i zapisu kodu ale również o nazewnictwo naszych klas, metod, zmiennych oraz obiektów

## DRY

Don't repeat yourself - Nie powtarzaj się
* Należy unikać niepotrzebnych powtórzeń w kodzie, kodu, który robi to samo i się powtarza.

## YAGNI

You ain't gonna need it - Nie będziesz tego potrzebował
* Zasada ta mówi, że w naszym programie powinniśmy umieszczać najistotniejsze funkcjonalności, które w danej chwili będą nam potrzebne.
* Nie należy pisać kodu, który w danym momencie się nam nie przyda.

## Reguła 5

(Zastąpiła regułę 3 w momencie wprowadzenia semantyki przenoszenia)<br>
Jeśli definiujemy jeden z poniższych:
* Destruktor
* Konstruktor kopiujący
* Operator przypisania kopiujący
* Konstruktor przenoszący
* Operator przypisania przenoszący

... to należy zdefiniować wszystkie 5 po to by mieć pewność, że nasz kod (który potem może być używany przez kogoś innego) zawsze będzie miał rozpatrzone przypadki. Autor może na przykład potrzebować kopiowania, a później.

Należy napisać własne implementacje powyższych funkcji **zawsze kiedy używa się gołych wskaźników**.

## Reguła 0

Nie należy implementować żadnej z powyższych funkcji samodzielnie tylko zdać się na implementacje kompilatora. Jest to możliwe w przypadku używania inteligentnych wskaźników.


## Kolejność załączania plików nagłówkowych

Zasada jest taka: Jeżeli masz plik example.cpp i inludujesz jego header #include "example.hpp" to plik taki powinien byc na samej górze, ponieważ odczytasz wtedy wszystkie include z tego pliku i nie musisz ich powtarzać w pliku .cpp. Następnie dajesz include plików z biblioteki standardowej jak `<vector>`, etc. Na koniec podajesz pozostałe nagłówki z projektów. Dlatego w main example.hpp jest pod `<iostream>` natomiast w example.cpp jest nad `<iostream>`.

```cpp
// Here is place for header
#include "example.hpp"

// Here is place for standard library
#include <algorithm>
#include <iostream>
#include <vector>

// Here is place for other headers from your project.
#include "some_other_file.h"
#include "some_helper_file.h"
```

## Inne

Dobrą praktyką jest zawsze inicjalizować zmienne dlatego np. w klasie deklarując `int`-a warto go od razu go wyzerować:

```cpp
int a = {};

std::string word;   // string z kolei tego nie wymaga
```
---

Kolejność pól metod w klasie:

1. Funkcje dostępne (public)
2. Funkcje niedostępne (private)
3. Pola niedostępne (private)
