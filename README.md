| Zagadnienie               | Jetpack Compose (Kotlin)                                          | Views (Java + XML)                                             |
|:--------------------------|:------------------------------------------------------------------|:---------------------------------------------------------------|
| Opis interfejsu           | kod Kotlina, funkcje @Composable                                  | plik XML + kod aktywności                                      |
| Liczba edytowanych plików | 3                                                                 | 4                                                              |
| Czas kompilacji           | 634 ms                                                            | 436 ms                                                         |
| Czytelność kodu           | Wyższa (deklaratywny kod, brak podziału na pliki widoku i logiki) | Niższa (rozbicie na plik XML z wyglądem i klasę Java z logiką) |
| Dostęp do kontrolek       | niepotrzebny - stan steruje widokiem                              | View Binding (dawniej findViewById)                            |
| Ukrywanie elementu        | element nie powstaje (if)                                         | atrybut visibility: invisible / gone                           |
| Podgląd interfejsu        | @Preview w IDE                                                    | edytor układu (Design / Split)                                 |
| Reakcja na zmianę danych  | automatyczna rekompozycja                                         | ręczna aktualizacja kontrolek                                  |




