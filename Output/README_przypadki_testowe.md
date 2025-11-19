# Przypadki Testowe - Normalizacja Adresów (WASKO API)

## Format pliku

Przypadki testowe znajdują się w pliku `przypadki_testowe_tabelaryczne.csv` w formacie CSV z separatorem `;`.

## Struktura kolumn

### Kolumny identyfikacyjne
- **TC_ID** - Unikalny identyfikator przypadku testowego
- **KATEGORIA** - Kategoria testu (Street Format Check, Normalizacja City, Wieloznaczność, itp.)
- **PRIORYTET** - Highest, High, Medium
- **OPIS** - Krótki opis przypadku testowego

### REQUEST (dane wejściowe do API)
- **REQUEST_streetName** - Nazwa ulicy
- **REQUEST_buildingNumber** - Numer budynku
- **REQUEST_city** - Nazwa miasta
- **REQUEST_postalCode** - Kod pocztowy

### EXPECTED (oczekiwane wartości w odpowiedzi API)
- **EXPECTED_streetName** - Znormalizowana nazwa ulicy
- **EXPECTED_buildingNumber** - Numer budynku
- **EXPECTED_city** - Znormalizowana nazwa miasta
- **EXPECTED_postalCode** - Znormalizowany kod pocztowy
- **EXPECTED_province** - Województwo
- **EXPECTED_district** - Powiat
- **EXPECTED_commune** - Gmina
- **EXPECTED_streetProbability** - Prawdopodobieństwo dopasowania ulicy (wartość liczbowa lub zakres, np. >0.9)
- **EXPECTED_cityProbability** - Prawdopodobieństwo dopasowania miasta
- **EXPECTED_postalCodeProbability** - Prawdopodobieństwo dopasowania kodu pocztowego
- **EXPECTED_combinedProbability** - Łączne prawdopodobieństwo
- **EXPECTED_postOfficeLocation** - Lokalizacja poczty
- **EXPECTED_isMultiFamily** - Czy budynek wielorodzinny (true/false)
- **EXPECTED_VerificationStatus** - Status weryfikacji (Verified, NotVerified, InVerification, PartiallyVerified)

### Inne
- **UWAGI** - Dodatkowe informacje o przypadku testowym

## Interpretacja wartości EXPECTED

### Wartości null
- `null` oznacza, że pole powinno być puste/null w odpowiedzi API
- Przykład: `EXPECTED_streetName;null` - ulica nie została znaleziona

### Wartości liczbowe z zakresami
- `>0.9` - prawdopodobieństwo powinno być większe niż 0.9
- `<0.5` - prawdopodobieństwo powinno być mniejsze niż 0.5
- `0` - prawdopodobieństwo powinno być równe 0 (brak dopasowania)

### Statusy weryfikacji
- **Verified** - Adres został pomyślnie zweryfikowany i znormalizowany
- **NotVerified** - Adres nie został zweryfikowany (brak dopasowania w WASKO)
- **InVerification** - Wymaga ręcznej weryfikacji (wieloznaczność, niepewność)
- **PartiallyVerified** - Częściowa weryfikacja (np. tylko miasto, bez ulicy)

## Kategorie przypadków testowych

### 1. Street Format Check
Testy rozdzielania formatu "City, Street" na osobne pola.

### 2. Normalizacja City
Testy normalizacji nazw miast (wielkość liter, błędy ortograficzne, itp.).

### 3. Wieloznaczność City
Testy sytuacji, gdy ta sama nazwa miasta występuje w wielu miejscach (np. Dąbrowa w 2 województwach).

### 4. Wieloznaczność Street
Testy sytuacji, gdy ta sama nazwa ulicy występuje z różnymi cechami (ul. vs pl., ul. vs rondo, itp.).

### 5. Normalizacja Street
Testy normalizacji nazw ulic.

### 6. Normalizacja PostalCode
Testy normalizacji kodów pocztowych (formaty, podobieństwo).

### 7. Enrich Address
Testy wzbogacania adresu o pola administracyjne (województwo, powiat, gmina).

### 8. Building Number
Testy walidacji numerów budynków (zakres, poza zakresem).

### 9. Edge Case
Testy przypadków brzegowych (puste pola, polskie znaki, długie nazwy).

### 10. API Test
Testy bezpośrednio związane z API (sukces, brak dopasowania).

### 11. Workflow
Testy pełnych procesów (rozdzielenie, normalizacja, wzbogacenie).

### 12. Rzeczywiste dane
Testy oparte na rzeczywistych danych z pliku `Ulic_Adresowy.csv`.

## Przykłady użycia

### Przykład 1: Sukces API
```
TC-035: Lublin, ul. Jana Pawła II, 17, kod 20-538
- Request: streetName="Jana Pawła II", buildingNumber="17", city="Lublin", postalCode="20-538"
- Expected: Wszystkie pola wypełnione, probabilities >0.9, status=Verified
```

### Przykład 2: Brak dopasowania (z screenshotu)
```
TC-036: Lublin, ul. Jana Pawła II, 17, kod 20538 (błędny)
- Request: streetName="Jana Pawła II", buildingNumber="17", city="Lublin", postalCode="20538"
- Expected: Wszystkie pola null, probabilities=0, status=NotVerified
```

### Przykład 3: Wieloznaczność
```
TC-008: Chełm, Żwirki i Wigury
- Request: streetName="Żwirki i Wigury", city="Chełm"
- Expected: InVerification (wieloznaczność: ul. vs pl.)
```

## Źródła danych

Wszystkie przypadki testowe oparte są na rzeczywistych danych z:
- `Ulic_Adresowy.csv` - główny plik z adresami
- `powtorzenia_nazw_w_miejscowosci.csv` - przypadki wieloznaczności

## Uwagi implementacyjne

1. **Prawdopodobieństwa** - Wartości w kolumnach `*_Probability` są orientacyjne. Rzeczywiste wartości mogą się różnić w zależności od implementacji algorytmu WASKO.

2. **Kody pocztowe** - W rzeczywistych danych z ULIC nie ma kodów pocztowych. Wartości w testach są przykładowe i powinny być zweryfikowane z rzeczywistymi danymi WASKO.

3. **PostOfficeLocation** - Wartość powinna być pobrana z danych WASKO (wymaga weryfikacji, który plik użyć).

4. **isMultiFamily** - Wymaga informacji z WASKO o typie budynku.

5. **Building Number Validation** - Wymaga informacji o zakresach numerów budynków w WASKO dla każdej ulicy.

## Priorytetyzacja

- **Highest** - Krytyczne przypadki, które muszą działać poprawnie
- **High** - Ważne przypadki, wpływające na jakość normalizacji
- **Medium** - Przypadki pomocnicze, edge cases

## Następne kroki

1. Zweryfikować kody pocztowe w testach z rzeczywistymi danymi WASKO
2. Dodać testy dla `postOfficeLocation` po ustaleniu źródła danych
3. Dodać testy dla `isMultiFamily` po uzyskaniu informacji z WASKO
4. Rozszerzyć testy o przypadki z różnymi wariantami zapisu (skróty, odmiany)

