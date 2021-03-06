--- 
title       : Ujarzmienie natury
subtitle    : czyli o tworzeniu pakietów w języku R
author      : Piotr Sobczyk
job         : PWr
logo        : pwr.jpg
biglogo     : znak_pwr.png
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : zenburn      # 
widgets     : [mathjax, bootstrap]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides

--- .big-background-slide

## Kilka uwag 

* Prezentacja na podstawie gigantycznej liczby materiałów internetowych
* Zbiór uwag i porad - nie przepis na pakiet
* Niektóre fragmenty są wręcz tłumaczeniem - referencje na końcu
* Dużo wiedzy pochodzi od Julie Josse i Michała Burdukiewicza

--- .big-background-slide

## Plan prezentacji

> * Po co tworzyć pakiety, jak to zrobić?
> * Tworzenie dobrego pakietu - podstawowe cechy
> * Tricks&Tips


--- .big-background-slide

## O czym (nie) jest ta prezentacja?

* Nie opisuję procesu tworzenia pakietu
* Pełny opis tego procesu można znaleźć w [tutorialu Dirka Eddelbuettela](http://dirk.eddelbuettel.com/papers/r_package_development_nov2014.pdf)
* Powiem o tym dlaczego warto pisać pakiety i jak sprawić, żeby ten pakiet był napisany dobrze
* Jak zachęcić użytkownika do skorzystania z naszego pakietu?

---  .big-background-slide

## Dlaczego tworzyć pakiety

> * Pakiet sprawia, że Twoja praca jest wpływowa - ma szansę być rzeczywiście używana
> * Pakiety, stworzony software to wizytówka podobnie jak prace naukowe czy prezentacje
> * Trzymanie funkcji w pakietach na własny użytek znacznie ułatwia pracę nad kodem
> * Pakiety wersjonujemy. Przeglądając nasze stare kody i wyniki możemy zorientować się
jak dokładnie wyglądały funkcje, z których korzystaliśmy

---  .background-slide

> * Pytanie: Tworzę kod na własny użytek jaką korzyść mam z wrzucenia moich kilku funkcji do pakietu?
> * Odpowiedź: Jeśli kod będzie wykorzystywany w przyszłości to dobrze, żeby tworzył pewną całość.
> * [P]: Czy można po prostu utrzymywać porządek w folderach? 
> * [U]: Tak, ale co gdy daną funkcję chcemy wykorzystać kilka razy?
> * [P]: Będę ładować funkcję z innego folderu albo ją skopiuję. 

---  .big-background-slide

> * [U]: Wtedy kod w folderach nie będzie stanowił całości i jedności, albo będziemy mieć kod funkcji w kilku miejscach. 
Co jeśli dokonamy zmiany i zapomnimy zmienić jednego z tych plików? Zrobi nam się bałagan i potencjalnie kod nie będzie
działał tak, jak tego oczekujemy.
> * [P]: Czyli zamiast tworzenia pakietów można utrzymywać porządek w kodzie?
> * [U]: Pakiety tworzy się właśnie w tym celu! Chodzi o to, żeby samemu nie zgubić się w swoim kodzie
oraz żeby inni mogli z naszego kodu skorzystać.

> * Generalna zasada jest taka, że aby nadać naszemy kodowi strukturę musimy poświęcić sporo trochę czasu. 
Ten czas oszczędzamy później gdy korzystamy z wcześniej stworzonych funkcji, kiedy łatwiej znaleźć nam błędy, lub
gdy musimy komuś wytłumaczyć jak nasz kod działa.
I wreszcie duże znaczenie ma to, że wiemy, że nasz kod robi **dokładnie** to czego oczekujemy.


--- .big-background-slide

## Jak się za to zabrać?

> * Pakiet najwygodniej tworzyć w RStudio, instalujemy też pakiet devtools
> * Nazwa pakietu powinna być prosta, bez dziwnych znaków, unikalna, googlowalna
> * Od razu wybieramy opcję na wersjonowanie pakietu

--- .big-background-slide

## DESCRIPTION


```r
Package: mojpakiet
Type: Package
Title: Jedno zdanie - co robi pakiet
Version: 0.0.1
Date: 2014-11-28
Author: Ja
Maintainer: Ja <adres@mailowy.pl>
Description: Krótki opis pakietu
License: GPL-3
Depends:
    R (>= 3.0.2)
Imports:
    RcppEigen
Suggests:
    knitr,
    rmarkdown,
    testthat
Repository: CRAN
```

--- .big-background-slide

## Co dalej?

```r
devtools::build()
devtools::document()
devtools::check()
devtools::test()
devtools::install_github()
R CMD install nazwapakietu_0.0.1.tar.gz
```
> * Nie powinniśmy dostawać żadnych Errors, Warnings, Notes
> * To jest warunek do przyjęcia do CRANu (repozytorium pakietów)

--- .big-background-slide

## Dokumentacja za pomocą pakietu roxygen2


```r
#' Short description
#'
#' Detailed descprition.
#'
#' @param param1, list of vectors, some vectors in a list
#' @param param2 an integer, some number
#' @param param3 a boolean, if TRUE (value set by default) then sth
#' @export
#' @return An object of class klasa consisting of
#' \item{item1}{a vector containing sth}
#' \item{item2}{numeric, value of sth}
#' @examples
#' \donttest{
#' example1
#' }
```

--- .big-background-slide

## Co jeszcze warto wiedzieć o dokumentacji


```r
  donttest{ } #kod nie będzie sprawdzany przy tworzeniu pakietu
  dontrrun{ } #kod nie będzie się wykonywałem po uruchomieniu
  example(nazwa_funkcji)
```
> * @export - funkcja będzie dostępna po załadowaniu
> * @internal - dokumentacja nie pokazuje się w index (w pomocy)
> * Nie opisuj czym jest parametr, ale jak go wykorzystać
> * Rób taką dokumentacją, z jakiej sam chciałbyś skorzystać (zasada Kanta)

--- .big-background-slide

## Styl - [źródło](http://stat405.had.co.nz/r-style.html)

> * Nazwy funkcji i zmiennych powinny być zgodne z wybraną, jedną, konwencją.
    Np. camelCase, lowercase, z separatorem: moja_funkcja
> * nazwy zmiennych - rzeczowniki, nazwy funkcji - czasowniki
> * Kod powinien być przejrzysty, linie mają mieć mniej niż 80 znaków
> * Należy pamiętać o wcięciach (m.in. przy if, for, definicjach funkcji)
> * Używaj <- zamiast = jako przypisania.

--- .big-background-slide

## Kontrola wersji

> * Kod przestaje działać po dużej zmianie
> * Dawno temu zrobiłeś/aś symulacje, ale program
  się od tego czasu nieco zmienił
> * Piszesz kod wspólnie z kimś
> * Rozwiązanie to system kontroli wersji
> * Konwencja wersjonowania
> * Dlaczego [używać git(hub)](http://kbroman.org/github_tutorial/)?

--- .big-background-slide

## Testy - czyli jak uniknąć najgorszego?

> * Najgorszą rzeczą jest późne odkrycie buga
> * Pisanie testów pozwoli Ci szybko znaleźć błędy
> * Każda funkcja powinna wykonywać jedną, niedużą pracę
> * Testy można/powinno się pisać **przed** rozpoczęciem pisania funkcji
> * They make you slow down and think about what you are doing (Jeff Leek)

--- .big-background-slide
## Testy - aspekty techniczne
> * Polecany pakiet to testthat
> * testy znajdują się w folderze tests/testthat

```r
test_that("tests for kwantyle",{
  set.seed(12345)
  dane <- rnorm(100)
  expect_less_than(kwantyle(dane)[1], 0)
  expect_warning(kwantyle(NULL))
  expect_equal(kwantyle(c(1,2,3,4,5))[1], 1.9999, tolerance = 1e-3, scale = 1)
})
```


--- .big-background-slide
## Vignette czyli prosty tutorial

> * Nie możemy nikogo zmusić do używania naszego pakietu
> * Dokumentacja to tylko doraźna pomoc
> * Musimy sprawić, żeby użytkownik wiedział od czego zacząć

<iframe id="player" style="position: relative; height: 250px; width: 450px" src="http://www.youtube.com/embed/Qx8RlqCG2mg?rel=0&amp;enablejsapi=1"></iframe>

--- .big-background-slide
## Vignette czyli tworzenie tutorialu

> * Krótki opis tego co robi pakiet i na jakich danych operuje
> * Przykładowa analiza 
> * Opis podstawowych funkcji wraz przykładem użycia
> * Plik vignette.Rmd w folderze vignettes/
> * Używamy [rmarkdown](http://rmarkdown.rstudio.com/)
> * W stosowanej statystyce vignette może ewoluować do pracy naukowej

--- .big-background-slide
## Shiny czyli jak się pokazać

> * Na CRAN-ie jest 6000 pakietów - musimy się wyróżnić z tłumu
> * Potencjalnemu użytkownikowi niekoniecznie chce się czytać nasz tutorial
> * Rozwiązaniem jest napisanie aplikacji webowej
> * Pewien schemat, z możliwościami rozbudowy, daje pakiet shiny
> * [Świetna dokumentacja](http://shiny.rstudio.com/)
> * Przyzwoita aplikacja do napisania w parę godzin

--- .big-background-slide
## Podsumowanie

> * Tworzenie pakietów pozwala na wywieraniu wpływu
> * Pakiet tworzony na własny użytek pozwala lepiej panować nad kodem
> * Warto dbać o dokumentację i jakość estetyczną kodu
> * System kontroli wersji strukturyzuje pracę (zwłaszcza w grupie)
> * Pisanie testów może spowalniać pracę, ale zwiększa jej jakość

--- .big-background-slide

## Materiały internetowe

> * Strona [Hadleya Wickhama](http://adv-r.had.co.nz/)
> * Blog [simplystatistics](www.simplystatistics.org) 
> * Blog [Przemysława Biecka](http://smarterpoland.pl/)
> * Tutorial [Dirka Eddelbuettela](http://dirk.eddelbuettel.com/papers/r_package_development_nov2014.pdf)
> * Tutorial [Jeffa Leeka](https://github.com/jtleek/rpackages/)

--- {
 tpl: thankyou, 
 social: [{title: Blog, href: "http://szychtawdanych.wordpress.com/"},
  {title: Github, href: "https://github.com/psobczyk"}, 
  {title: IM, href: "http://im.pwr.edu.pl/~sobczyk"}]
}

## Pytania?

Slajdy są dostępne na mojej stronie internetowej

