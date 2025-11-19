# Przypadki Testowe - Normalizacja Adresów (WASKO API)

## Przegląd

Zestaw przypadków testowych do weryfikacji działania API normalizacji adresów WASKO. Zawiera **591 podstawowych przypadków testowych** oraz **684 przypadków w pliku automatyzacji** (w tym 185 rozszerzonych wariantów wieloznaczności).

## Pliki

### 1. `przypadki_testowe_kompletne.csv`
**Przeznaczenie:** Dokumentacja, analiza, pełna wersja z wszystkimi danymi

- **Liczba przypadków:** 591
- **Liczba kolumn:** 25
- **Zawiera:** Wszystkie kolumny włącznie z `EXPECTED_VerificationStatus`
- **Format:** CSV z separatorem `;`, kodowanie UTF-8

**Użycie:**
- Dokumentacja przypadków testowych
- Analiza pokrycia testowego
- Pełna wersja referencyjna

### 2. `przypadki_testowe_automation.csv`
**Przeznaczenie:** Automatyzacja testów, integracja z systemami CI/CD

- **Liczba przypadków:** 684
- **Liczba kolumn:** 21
- **Zawiera:** Tylko niezbędne kolumny dla automatyzacji (bez `EXPECTED_VerificationStatus`)
- **Format:** CSV z separatorem `;`, kodowanie UTF-8
- **Rozszerzone warianty:** 185 wariantów wieloznaczności

**Użycie:**
- Automatyczne testy API
- Integracja z systemami testowymi
- Egzekucja testów w CI/CD

## Struktura kolumn

### Kolumny identyfikacyjne

| Kolumna | Opis | Przykład |
|---------|------|----------|
| `TC_ID` | Unikalny identyfikator przypadku testowego | `TC-001`, `TC-439-1`, `TC-439-2` |
| `KATEGORIA` | Kategoria testu | `API Test`, `Wieloznaczność`, `Błędna kombinacja` |
| `PRIORYTET` | Priorytet testu | `Highest`, `High`, `Medium`, `Low` |
| `OPIS` | Krótki opis przypadku testowego | `Sukces API - Lublin` |

### REQUEST (dane wejściowe do API)

| Kolumna | Opis | Przykład |
|---------|------|----------|
| `REQUEST_streetName` | Nazwa ulicy | `Jana Pawła II` |
| `REQUEST_buildingNumber` | Numer budynku | `17` |
| `REQUEST_city` | Nazwa miasta | `Lublin` |
| `REQUEST_postalCode` | Kod pocztowy | `20-538` |

### EXPECTED (oczekiwane wartości w odpowiedzi API)

| Kolumna | Opis | Przykład |
|---------|------|----------|
| `EXPECTED_streetName` | Znormalizowana nazwa ulicy | `Jana Pawła II` |
| `EXPECTED_buildingNumber` | Numer budynku | `17` |
| `EXPECTED_city` | Znormalizowana nazwa miasta | `Lublin` |
| `EXPECTED_postalCode` | Znormalizowany kod pocztowy | `20-538` |
| `EXPECTED_province` | Województwo | `LUBELSKIE` |
| `EXPECTED_district` | Powiat | `Lublin` |
| `EXPECTED_commune` | Gmina | `Lublin` |
| `EXPECTED_streetProbability` | Prawdopodobieństwo dopasowania ulicy | `>0.9` |
| `EXPECTED_cityProbability` | Prawdopodobieństwo dopasowania miasta | `>0.95` |
| `EXPECTED_postalCodeProbability` | Prawdopodobieństwo dopasowania kodu | `>0.9` |
| `EXPECTED_combinedProbability` | Łączne prawdopodobieństwo | `>0.9` |

**Uwaga:** W pliku `przypadki_testowe_automation.csv` **nie ma** kolumny `EXPECTED_VerificationStatus` (usunięta dla automatyzacji).

### Inne kolumny

| Kolumna | Opis |
|---------|------|
| `UWAGI` | Dodatkowe informacje o przypadku testowym |
| `RYZYKO_BLEDU` | Informacja o ryzyku błędu dla danego przypadku |

## Numeracja przypadków testowych

### Podstawowa numeracja
- Przypadki są ponumerowane sekwencyjnie: `TC-001`, `TC-002`, ..., `TC-591`
- Numeracja jest zgodna z sortowaniem: **kategoria → podkategoria → priorytet**

### Numeracja wariantów wieloznaczności
- Przypadki wieloznaczności mają format: `TC-XXX-N`
- Przykład: `TC-439-1`, `TC-439-2` dla Dąbrowa w 2 województwach
- Każdy wariant ma osobny wiersz z unikalnym TC_ID
- W pliku automatyzacji: **185 wariantów** (z 92 rozszerzonych przypadków)

## Interpretacja wartości EXPECTED

### Wartości null
- `null` oznacza, że pole powinno być puste/null w odpowiedzi API
- Przykład: `EXPECTED_streetName;null` - ulica nie została znaleziona

### Wartości liczbowe z zakresami
- `>0.9` - prawdopodobieństwo powinno być większe niż 0.9
- `<0.5` - prawdopodobieństwo powinno być mniejsze niż 0.5
- `0` - prawdopodobieństwo powinno być równe 0 (brak dopasowania)

### Statusy weryfikacji (tylko w pliku kompletny)
- **Verified** - Adres został pomyślnie zweryfikowany i znormalizowany
- **NotVerified** - Adres nie został zweryfikowany (brak dopasowania w WASKO)
- **InVerification** - Wymaga ręcznej weryfikacji (wieloznaczność, niepewność)
- **PartiallyVerified** - Częściowa weryfikacja (np. tylko miasto, bez ulicy)

## Kategorie przypadków testowych

### API Test (4 przypadków, 0.7%)
- **Kompletny:** 4 przypadków
- **Automation:** 4 przypadków

### Building Number (13 przypadków, 2.2%)
- **Kompletny:** 13 przypadków
- **Automation:** 13 przypadków

### Błędna kombinacja (180 przypadków, 30.5%)
- **Kompletny:** 180 przypadków
- **Automation:** 180 przypadków

### Błędne dopasowanie (153 przypadków, 25.9%)
- **Kompletny:** 153 przypadków
- **Automation:** 153 przypadków

### Edge Case (7 przypadków, 1.2%)
- **Kompletny:** 7 przypadków
- **Automation:** 7 przypadków

### Enrich Address (2 przypadków, 0.3%)
- **Kompletny:** 2 przypadków
- **Automation:** 2 przypadków

### Normalizacja (65 przypadków, 11.0%)
- **Kompletny:** 65 przypadków
- **Automation:** 65 przypadków

### Rzeczywiste dane (10 przypadków, 1.7%)
- **Kompletny:** 10 przypadków
- **Automation:** 10 przypadków

### Street Format Check (2 przypadków, 0.3%)
- **Kompletny:** 2 przypadków
- **Automation:** 2 przypadków

### Wieloznaczność (153 przypadków, 25.9%)
- **Kompletny:** 153 przypadków
- **Automation:** 246 przypadków

### Workflow (2 przypadków, 0.3%)
- **Kompletny:** 2 przypadków
- **Automation:** 2 przypadków


## Statystyki

### Plik kompletny (`przypadki_testowe_kompletne.csv`)

- **Łącznie przypadków:** 591
- **Kategorie:** 11
- **Priorytety:**
  - Highest: 42 (7.1%)
  - High: 298 (50.4%)
  - Medium: 189 (32.0%)
  - Low: 62 (10.5%)
- **Statusy weryfikacji:**
  - Verified: 157 (26.6%)
  - InVerification: 418 (70.7%)
  - NotVerified: 11 (1.9%)
- **Kody pocztowe:** 520 przypadków (88.0%)

### Plik automatyzacji (`przypadki_testowe_automation.csv`)

- **Łącznie przypadków:** 684
- **Rozszerzonych wariantów:** 185
- **Kolumny:** 21 (bez EXPECTED_VerificationStatus)
- **Kody pocztowe:** 578 przypadków (84.5%)
- **Priorytety:**
  - Highest: 42 (6.1%)
  - High: 339 (49.6%)
  - Medium: 219 (32.0%)
  - Low: 84 (12.3%)

## Przykłady użycia

### Przykład 1: Sukces API
```
TC-001: Lublin, ul. Jana Pawła II, 17, kod 20-538
- Request: streetName="Jana Pawła II", buildingNumber="17", city="Lublin", postalCode="20-538"
- Expected: Wszystkie pola wypełnione, probabilities >0.9
```

### Przykład 2: Brak dopasowania
```
TC-002: Lublin, ul. Jana Pawła II, 17, kod 20538 (błędny)
- Request: streetName="Jana Pawła II", buildingNumber="17", city="Lublin", postalCode="20538"
- Expected: Wszystkie pola null, probabilities=0
```

### Przykład 3: Wieloznaczność miasta (rozszerzona na warianty)
```
TC-439-1: Dąbrowa w OPOLSKIE
- Request: streetName="Lipowa", city="Dąbrowa"
- Expected: city="Dąbrowa", province="OPOLSKIE"

TC-439-2: Dąbrowa w KUJAWSKO-POMORSKIE
- Request: streetName="Lipowa", city="Dąbrowa"
- Expected: city="Dąbrowa", province="KUJAWSKO-POMORSKIE"
```

### Przykład 4: Wieloznaczność cech
```
TC-480-1: Chełm, ul. Żwirki i Wigury
- Request: streetName="Żwirki i Wigury", city="Chełm"
- Expected: streetName="Żwirki i Wigury", province="LUBELSKIE"

TC-480-2: Chełm, pl. Żwirki i Wigury
- Request: streetName="Żwirki i Wigury", city="Chełm"
- Expected: streetName="Żwirki i Wigury", province="LUBELSKIE"
```

## Użycie w automatyzacji

### Format pliku
- **Separator:** `;` (średnik)
- **Kodowanie:** UTF-8 z BOM (utf-8-sig)
- **Format:** CSV standardowy

### Przykład użycia w Python
```python
import csv

def load_test_cases(file_path):
    """Wczytuje przypadki testowe z pliku CSV"""
    test_cases = []
    with open(file_path, 'r', encoding='utf-8-sig') as f:
        reader = csv.DictReader(f, delimiter=';')
        for row in reader:
            test_cases.append(row)
    return test_cases

def execute_test_case(tc, api_client):
    """Wykonuje pojedynczy przypadek testowy"""
    # Przygotuj request
    request = {
        'streetName': tc.get('REQUEST_streetName') or None,
        'buildingNumber': tc.get('REQUEST_buildingNumber') or None,
        'city': tc.get('REQUEST_city') or None,
        'postalCode': tc.get('REQUEST_postalCode') or None
    }
    
    # Usuń None values
    request = {k: v for k, v in request.items() if v is not None and v != 'null'}
    
    # Wywołaj API
    response = api_client.normalize_address(**request)
    
    # Porównaj z EXPECTED
    # ... logika weryfikacji ...
    
    return {
        'tc_id': tc.get('TC_ID'),
        'passed': True/False,
        'details': '...'
    }

# Użycie
test_cases = load_test_cases('przypadki_testowe_automation.csv')
for tc in test_cases:
    result = execute_test_case(tc, api_client)
    print(f"{result['tc_id']}: {'PASS' if result['passed'] else 'FAIL'}")
```

### Przykład użycia w JavaScript/Node.js
```javascript
const fs = require('fs');
const csv = require('csv-parser');

function loadTestCases(filePath) {
    return new Promise((resolve, reject) => {
        const testCases = [];
        fs.createReadStream(filePath)
            .pipe(csv({ separator: ';' }))
            .on('data', (row) => testCases.push(row))
            .on('end', () => resolve(testCases))
            .on('error', reject);
    });
}

async function executeTestCases() {
    const testCases = await loadTestCases('przypadki_testowe_automation.csv');
    for (const tc of testCases) {
        const request = {
            streetName: tc.REQUEST_streetName || null,
            buildingNumber: tc.REQUEST_buildingNumber || null,
            city: tc.REQUEST_city || null,
            postalCode: tc.REQUEST_postalCode || null
        };
        
        // Usuń null values
        Object.keys(request).forEach(key => {
            if (request[key] === 'null' || !request[key]) {
                delete request[key];
            }
        });
        
        // Wywołaj API i zweryfikuj
        // ...
    }
}
```

## Rozszerzone przypadki wieloznaczności

W pliku `przypadki_testowe_automation.csv` przypadki wieloznaczności są rozszerzone na osobne wiersze dla każdego możliwego wariantu:

### Wieloznaczność miasta
- **Przykład:** Dąbrowa występuje w 2 województwach (OPOLSKIE, KUJAWSKO-POMORSKIE)
- **Rozszerzenie:** `TC-439-1` (OPOLSKIE), `TC-439-2` (KUJAWSKO-POMORSKIE)
- **Użycie:** Każdy wariant powinien być testowany osobno

### Wieloznaczność cech
- **Przykład:** Chełm - ul. vs pl. "Żwirki i Wigury"
- **Rozszerzenie:** `TC-480-1` (ul.), `TC-480-2` (pl.)
- **Użycie:** Każdy wariant powinien być testowany osobno

### Wieloznaczność z NAZWA_2
- **Przykład:** Różne warianty NAZWA_2 dla tej samej ulicy
- **Rozszerzenie:** Osobne wiersze dla każdego wariantu
- **Użycie:** Weryfikacja poprawności dopasowania dla każdego wariantu

## Źródła danych

Wszystkie przypadki testowe oparte są na rzeczywistych danych z:
- **`Ulic_Adresowy.csv`** - główny plik z adresami (304,879 rekordów)
- **`powtorzenia_nazw_w_miejscowosci.csv`** - przypadki wieloznaczności (532 rekordy)
- **`niezgodnosci_cecha_w_nazwie.csv`** - niezgodności cech w nazwach (5,459 rekordów)

## Uwagi implementacyjne

1. **Prawdopodobieństwa** - Wartości w kolumnach `*_Probability` są orientacyjne. Rzeczywiste wartości mogą się różnić w zależności od implementacji algorytmu WASKO.

2. **Kody pocztowe** - Kody pocztowe zostały dodane na podstawie dostępnych danych. Wartości są przykładowe i powinny być zweryfikowane z rzeczywistymi danymi WASKO.

3. **Wieloznaczność** - Przypadki wieloznaczności w pliku automatyzacji są rozszerzone na osobne wiersze dla każdego możliwego wariantu. Każdy wariant powinien być testowany osobno.

4. **Priorytetyzacja** - Przypadki są priorytetyzowane zgodnie z ważnością:
   - **Highest** - Krytyczne przypadki (np. "Wszystkie parametry błędne")
   - **High** - Ważne przypadki (wieloznaczność, błędne dopasowania)
   - **Medium** - Przypadki pomocnicze
   - **Low** - Przypadki niskiego priorytetu

5. **Wartości null** - W pliku CSV wartość `null` oznacza, że pole powinno być puste/null w odpowiedzi API. W kodzie należy to obsłużyć odpowiednio (usunąć z request lub ustawić na None/null).

## Pokrycie testowe

### Mocne strony
- ✅ Duża liczba przypadków testowych (591 podstawowych, 684 w automatyzacji)
- ✅ Dobra różnorodność scenariuszy błędów
- ✅ Pokrycie wieloznaczności (185 wariantów)
- ✅ Pokrycie błędnych kombinacji
- ✅ Pokrycie błędnych dopasowań
- ✅ Kody pocztowe w 88.0% przypadków

### Obszary do rozważenia
- ⚠ Edge cases: można rozszerzyć
- ⚠ Rzeczywiste dane: można dodać więcej
- ⚠ Workflow: można rozszerzyć
- ⚠ Enrich Address: można rozszerzyć

## Różnice między plikami

| Aspekt | Kompletny | Automation |
|--------|-----------|------------|
| Liczba przypadków | 591 | 684 |
| Liczba kolumn | 25 | 21 |
| EXPECTED_VerificationStatus | ✅ Tak | ❌ Nie |
| Rozszerzone warianty | ❌ Nie | ✅ Tak (185) |
| Przeznaczenie | Dokumentacja | Automatyzacja |

## Aktualizacje

- **2025-11-18:** Utworzono plik automatyzacji z rozszerzonymi przypadkami wieloznaczności
- **2025-11-18:** Dodano kody pocztowe dla 88.0% przypadków
- **2025-11-18:** Poprawiono priorytety i sortowanie
- **2025-11-18:** Rozszerzono przypadki wieloznaczności na osobne wiersze dla każdego wariantu
- **2025-11-18:** Usunięto EXPECTED_VerificationStatus z pliku automatyzacji

## Wsparcie

W przypadku pytań lub problemów z przypadkami testowymi, proszę skontaktować się z zespołem odpowiedzialnym za testy.
