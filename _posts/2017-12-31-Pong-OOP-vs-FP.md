---
layout: post
title: OOP vs FP. Implementing "Pong" game.
tags: [object-oriented-programming, functional-programming, tdd, ddd]
fullview: false
comments: true
---


# Object oriented programming vs Functional programming: By example.

Gdy zaczynałem naukę programowania funkcyjnego nie mogłem znaleźć informacji jakie korzyści tak naprawdę daje ten
paradygmat w porównaniu do innych paradygmatów (głównie OOP). Co prawda jest masa informacji o tym, że FP jest
deklaratywne, że jest lepsza
kompozycyjność, zwięzłość kodu, niemutowalność, dużo prostsze concurrency itd. Ok, tyle, że mi to nic nie mówiło.
Np. dlaczego deklaratywny kod jest lepszy od imperatywnego? albo dlaczego niemutowalny kod jest prostszy w zrozumieniu?
Potrzebowałem solidnych argumentów wraz z przykładami. Nie znalazłem ich. Zacząłem po prostu pisać funkcyjnie wierząc, 
że to jest dobra droga i mój kod będzie lepszy. I tak było. Nauczyłem się pisać funkcyjnie i widzę, że mój kod stał się
lepszy.
W tym momencie zajmuję się programowaniem funkcyjnym od około czterech lat, a wciąż gdy ktoś mnie spyta
"co jest lepszego w tym Twoim FP?", to nie jestem w stanie tego wyjaśnić w prosty sposób, tak naprawdę chyba jeszcze
nikogo nie przekonałem do tego aby się zainteresował FP na poważnie.
Moje odczucia są takie, że gdy pisze w FP to zajmuje mi to mniej czasu. Gdy wracam do kodu po dłuższym czasie to od razu
wiem co ten kod robi (a w przypadku OOP było dużo ciężej). Widzę, też że mam sporo mniej błędów w kodzie.
Jednakże ciagle to są tylko moje odczucia na podstawie doświadczenia, które zdobyłem pisząc aplikacje FP.
Nie potrafię tego wyjaśnić osobie, która jeszcze w tym paradygmacie nie pisała.

Stąd narodził się pomysł aby napisać serię artykułów, która porówna dwa paradygmaty, OOP i FP, na przykładzie tej samej
aplikacji. Dzięki temu osoby, które nie wiedzą co można zyskać pisząc w FP, zobaczą jasno czym się różnią te dwa
paradygmaty.
Ponadto będę chciał zrobić swego rodzaju pomiary, np. ile czasu zajęła mi implementacja danego ficzera w OOP a ile w FP,
ile linii kodu mają dane implementacje, jak łatwo jest dodać nową funkcjonalność do istniejącego kodu, ile jest w kodzie
"elementów" itp. Wreszcie oprócz prywatnych "odczuć", będą też liczby. Sam jestem ciekaw wyniku tego eksperymentu, bo
być może okaże się, że OOP wcale nie wypada gorzej. Może problemem od zawsze był i będzie po prostu design, a nie paradygmat?
Mam nadzieję, że się tego dowiemy.

Chcę też zaznaczyć, że nie skupię sie tylko na różnicach pomiędzy paradygmatami. Cała seria będzie napisana w formie
poradnika opisującego proces powstawania aplikacji (obu implementacji). Także będzie też dużo o szeroko pojętym
designie, o testowaniu, trochę o planowaniu prac i paru innych aspektach. Wszystkie podejmowane przeze mnie decyzje będę
się starał wyjaśniać. Muszę też zaznaczyć, że piszę tego typu aplikację po raz pierwszy (jaką aplikacje dowiemy się
później) i też będę się uczył na bieżąco, weźcie to pod uwagę. Zwiększy to autentyczność całego eksperymentu. 
O czym nie będę pisał? Nie chcę się rozwodzić nad użytymi technologiami, bibliotekami i generalnie nad tymi rzeczami,
które można śmiało wrzucić do worka o nazwie "RTFM".

# Aplikacja

Zastanawiałem się jaką aplikację wybrać do tego eksperymentu aby najlepiej zaprezentować oba paradygmaty.
Aplikacja powinna mieć w miarę nietrywialną domenę (ale bez przesady), a jej wykonanie nie powinno zająć więćj niż
tydzień czasu pracując po godzinach, tak aby ta seria artykułów nie rozwinęła się w trzy tomową powieść, bo nikt tego
nie przeczyta. Jednocześnie powinna być to aplikacja jakiej jeszcze nie pisałem aby podejść do problemu z czystą głową.

Zanim napiszę jaką aplikację wybrałem, chcę zrobić mały eksperyment dla Was. Mianowicie poniżej przedstawię wam
wszystkie testy akceptacyjne z końcowej wersji aplikacji i chciałbym abyście spróbowali dzięki nim odgadnąć sami jaką
aplikację będę pisać. Ma to na celu sprawdzenie czy odpowiednio napisane testy akceptacyne rzeczywiście mogą być 
"living documentaion". Powinno się udać.

TUTAJ TESTY AKCEPTACYJNE

Ok. Mam nadzieję, że się udało i wszyscy już wiedzą, iż aplikacja którą będziemy tworzyć to klasyczna gra o nazwie
"Pong".
Opiszmy wymagania naszej aplikacji.

# Wymagania.

Prosta wersja gry Pong.
W widoku gry o ustalonej wysokości i szerokości widzimy dwie paletki po przeciwnych stronach widoku (lewo i prawo). 
Lewą paletką steruje gracz. Może nią poruszać w pionie z ustaloną prędkością za pomocą klawiszy 'W' (w górę) i 'S'
(w dół). Paletka nie może wyjść poza krawędzi widoku (górna i dolna krawędź).
Prawą paletką steruje komputer w takim samym zakresie.
Gdy gra się uruchomi obie paletki mają być na środku wysokości widoku gry, a w centrum widoku pojawia się piłka i
zaczyna się poruszać w losowym kierunku (lecz nie pionowo) z ustaloną prędkością.
Gdzieś na widoku gry powinna się znajdować punktacja dla obu paletek, która początkowo wynosi oczywiście 0:0.
Piłka odbija się zgodnie z prostymi zasadami fizyki od górnej i dolnej krawędzi widoku gry oraz od paletek.
Paletka to prostokąt gdzie dłuższy bok jest równoległy do bocznych krawędzi.
W grze chodzi o to aby gracz nie pozwolił piłce dotrzeć do jego krawędzi poprzez odbijanie jej paletką.
Jeżeli piłka dotknie którejś z bocznych krawędzi widoku gry, to zaliczamy punkt dla paletki po przeciwległej stronie
dotkniętej krawędzi i resetujemy położenie paletek i piłki tak jakby gra się rozpoczęła od nowa, jednak nie resetujemy
punktacji.
Gra toczy się do zdobycia ustalonej ilości pkt przez jedną paletkę.
Po zakończeniu gry, gra ma się zrestartować i rozpocząć na nowo.
Piłka musi mieć prędkość większą od paletek aby gra miała sens.
Komputer gra w ten sposób, że stara się nadążyć za piłką przesuwając palętke zawsze w 
taki sposób aby środek jego paletki byl na środku piłki.


Wymagania nie są perfekcyjne. Można powiedzieć, że spisalem je na kolanie, ale w prawdziwych projektach, specyfikacje
często wyglądają jeszcze gorzej, albo nie ma ich wcale.

# Technologia

Skoro już wiemy jaką piszemy aplikację i mamy jej wymagania, to możemy dobrać technologię.
Nie będę sie tutaj specjalnie rozwodził. Potrzebujemy czegoś co w łatwy sposób pozwoli nam stworzyć interfejs graficzny,
oraz języka który wspiera zarówno paradygmat obiektowy jak i funkcyjny. Ponadto chcę mieć typy statyczne.
Mój wybór to TypeScript. Sprawdzi się w obu paradygmatach, UI możemy w prosty sposób zrobić w przeglądarce a dodatkowo
mamy statyczną analizę typów.

Jeżeli ktoś nie zna TS, a zna JS, to bez problemu powinien się odnaleźć, ponieważ TS dodaje do składni JS jedynie
informacje o typach oraz modyfikatory dostępu dla zmiennych klas (czy o czymś zapomniałem?).

# Plan działania.

Określmy release plan. Wypuszczenie wersji bez wszystkich funkcji opisanych w wymaganiach nie ma sensu, ponieważ
gra nie bedzie spełniała swojego zadania. Dlatego na pierwszy release musimy mieć zaimplementowane od razu wszystko.

Określmy zatem kolejność funkcjonalności które bedziemy realizować.

1. Jako pierwszą funkcjonalność możemy zrealizować rozpoczęcie samej gry. Wiemy, że po rozpoczęciu gry gracz ma zobaczyć
widok gry o ustalonej wysokosci i szerokosci, a w nim dwie paletki po lewej i prawej stronie ustawione na środku
wysokosci widoku, piłkę która jest w centrum oraz punktację która wynosi 0:0. Zauważcie, że w tym momencie musimy się
już zająć renderowaniem gry.
2. Poruszanie się paletkami za pomocą klawiszy W i S. Oraz zatrzymywanie paletek gdy
dotkną krawędzi widoku gry.
3. Nadanie losowego kierunku piłce (niepionowego) i jej poruszanie się, oraz odbijanie od ścian (górnej i dolnej).
4. Naliczanie punktów i resetowanie pozycji paletek i piłki gdy ta dotknie bocznej krawędzi.
5. Odbijanie piłki od paletek.
6. Zaimplementowanie AI paletki komputera.
7. Zakończenie gry gdy którą paletka zdobędzie określoną ilość punktów.


Myślę, że wszystko już wiemy. Dodam jeszcze tylko, że po każdym zakończonym ficzerze będę wypychał branch do
repo gita aby każdy mógł sobie podejrzeć kod na wszystkich etapach tworzenia aplikacji.


# Feature: Rozpoczęcie gry

Rozpocznę implementację w paradygmacie funkcyjnym, jednakże, co ficzer będę zmieniał kolejność, także 
kolejny ficzer zacznę od implementacji w OOP. Myślę, że tak będzie fair.

## FP

Rozpoczniemy od napisania testu akceptacyjnego.


```gherkin
Feature: Game start

  Scenario: game is started
    When game is started
    Then two paddles are located at both sides (left and right) of game view, in the middle of view height
      And there is a ball located in the center of game view
      And game score is 0:0
```
 
Zacznijmy od wyjaśnienia dlaczego zaczynam od testu akceptacyjnego i dlaczego wygląda on tak jak wygląda.

Zadaniem testów akceptacyjnych jest sprawdzenie czy dana funkcjonalność spełnia wymagania klienta. Problemem
takich testów jest fakt, że w wymaganiach często są stwierdzenia typu "użytkownika klika w przycisk i dodaje produkt
do koszyka", lub "użytkownik powinien zobaczyć napis 'Sukces'". Tego typu wymagania są mocno zkaplowane z interfejsem
graficznym użytkownika, a ten jest ciężki do testowania. Testy na poziomie UI to najczęściej testy end2end, które są
wolne i ciężkie w utrzymaniu. Wynika to z różnych kwestii o których nie będę tutaj pisał. W kazdym razie aby móc
otestować wszystkie wymagania akceptacyjne dochodzimy do momentu w którym mamy tyle testów e2e, że praca z tymi testami
staje się niemożliwie uciążliwa, np. buildy na jenkinsie wysypują się w sposób losowy, lub drobna zmiana w UI
wprowadzona przez UX designera psuje 30% testów i wszystkie musimy naprawić a PM krzyczy, że zmiany trzeba wypchnąć na
już bo mamy critical buga. Przerabiałem to nie raz, nie tędy droga.
Ok, nie możemy mieć testów akceptacyjnych na poziomie e2e, lecz nikt nam nie zabrania pisać ich poziom niżej. Po prostu
omijamy UI i testujemy tylko naszą logikę, ale traktujemy ją jako black box (czyli w testach używamy tylko ustalonego
interfejsu publicznego). To o czym piszę, jest bardzo dobrze opisane w książce pod tytułem "Specification by example" by
Gojko Adzic. 
W ten sposób mamy testy akceptacyjne na wszystkie wymagania, testy są bardzo szybkie, bronią nas przed regresją w
domenie, bo wciąż są na wysokim poziomie (scope) i jeżeli są dobrze napisane to stanowią "living documentation" dla
wszystkich developerów (jest to szczególnie istotne dla developerów 
którzy dopiero dołączają do teamu, lub tych którzy grzebią w funkcjonalnościach o których wcześniej nie mieli pojęcia).

Dobra, ale ktoś może powiedzieć "W ten sposób nie wiemy gdzy aplikacja rzeczywiście działa w całośći". 
Tak i nie. Musimy do tego podejść zdrowo rozsądkowo. 



