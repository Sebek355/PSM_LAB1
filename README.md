# Porównanie implementacji: Jetpack Compose (Kotlin) vs Views (Java + XML)

Projekt ma na celu porównanie dwóch różnych podejść do tworzenia interfejsu użytkownika w systemie Android na przykładzie prostej aplikacji, która pozwala na pokazywanie i ukrywanie danych osobowych ("Sebastian Światłowski") za pomocą przycisku.

Aplikacja została zaimplementowana w dwóch wersjach:
1. **Jetpack Compose (Kotlin)** – nowoczesne, deklaratywne podejście.
2. **Views (Java + XML)** – tradycyjne podejście imperatywne z wykorzystaniem plików XML oraz biblioteki View Binding.

---

## 📊 Porównanie podejść

Poniższa tabela przedstawia kluczowe różnice zaobserwowane podczas realizacji obu wersji:

| Zagadnienie               | Jetpack Compose (Kotlin)                                          | Views (Java + XML)                                             |
|:--------------------------|:------------------------------------------------------------------|:---------------------------------------------------------------|
| **Opis interfejsu**           | Kod Kotlina, funkcje `@Composable`                                  | Plik XML + kod aktywności                                      |
| **Liczba edytowanych plików** | 3                                                                 | 4                                                              |
| **Czas kompilacji**           | 634 ms                                                            | 436 ms                                                         |
| **Czytelność kodu**           | Wyższa (deklaratywny kod, brak podziału na pliki widoku i logiki) | Niższa (rozbicie na plik XML z wyglądem i klasę Java z logiką) |
| **Dostęp do kontrolek**       | Niepotrzebny – stan steruje widokiem                              | View Binding (dawniej `findViewById`)                            |
| **Ukrywanie elementu**        | Element nie powstaje (`if`)                                         | Atrybut `visibility: invisible / gone`                           |
| **Podgląd interfejsu**        | `@Preview` w IDE                                                    | Edytor układu (Design / Split)                                 |
| **Reakcja na zmianę danych**  | Automatyczna rekompozycja                                         | Ręczna aktualizacja kontrolek                                  |

---

## 📱 Działanie Aplikacji & UI

Obie aplikacje wizualnie zachowują się identycznie. Interfejs składa się z wyśrodkowanego przycisku oraz tekstu, który pojawia się pod nim po kliknięciu.

### Stan początkowy (Dane ukryte)
Na ekranie widoczny jest wyłącznie przycisk z napisem "Show data".

| Jetpack Compose | Views (Java + XML) |
|:---:|:---:|
| <img src="screeny/Zrzut ekranu 2026-10-12 234000.png" width="280" alt="Compose ukryte"> | <img src="screeny/Zrzut ekranu 2026-10-12 234857.png" width="280" alt="Views ukryte"> |
| *Status: oczekiwanie na kliknięcie* | *Status: oczekiwanie na kliknięcie* |

### Stan aktywny (Dane widoczne)
Po kliknięciu przycisku jego tekst zmienia się na "Hide data", a poniżej wyświetla się imię i nazwisko: **Sebastian Światłowski**.

| Jetpack Compose | Views (Java + XML) |
|:---:|:---:|
| <img src="screeny/Zrzut ekranu 2026-10-12 234018.png" width="280" alt="Compose widoczne"> | <img src="screeny/Zrzut ekranu 2026-10-12 234919.png" width="280" alt="Views widoczne"> |
| *Status: dane wyświetlone* | *Status: dane wyświetlone* |

---

## 💻 Omówienie Implementacji

### 1. Wersja Jetpack Compose (Kotlin)
Interfejs użytkownika jest opisany w całości w języku Kotlin przy użyciu paradygmatu deklaratywnego. 

* **Zarządzanie stanem**: Zastosowano `var isDataVisible by remember { mutableStateOf(false) }`. Kiedy stan ulega zmianie, Compose automatycznie odświeża (rekomponuje) tylko te elementy, które od niego zależą.
* **Struktura**: Zastosowano kontener `Column` z wyśrodkowaniem. Przycisk dynamicznie pobiera zasoby tekstowe w zależności od stanu `isDataVisible`, a sam tekst z nazwiskiem jest renderowany warunkowo przy użyciu zwykłej instrukcji `if (isDataVisible)`.

```kotlin
// Fragment kodu z MainActivity.kt (Compose)
@Composable
fun BusinessCardScreen(modifier: Modifier = Modifier) {
    var isDataVisible by remember { mutableStateOf(false) }

    Column(
        modifier = modifier.fillMaxSize().padding(all = 24.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Button(onClick = { isDataVisible = !isDataVisible }) {
            Text(
                text = stringResource(
                    id = if (isDataVisible) R.string.hide_data else R.string.show_data
                )
            )
        }

        if (isDataVisible) {
            Text(
                text = stringResource(id = R.string.full_name),
                style = MaterialTheme.typography.headlineMedium,
                modifier = Modifier.padding(top = 16.dp)
            )
        }
    }
}
