# Przypadki Testowe - Normalizacja Adresów (WASKO)

## 1. Street Format Check - Rozdzielanie "City, Street"

### TC-001: Poprawne rozdzielenie formatu "City, Street"
**Input:**
- Street: "Warszawa, Aleje Jerozolimskie"
- City: (puste)

**Oczekiwany rezultat:**
- City: "Warszawa"
- Street: "Aleje Jerozolimskie"
- VerificationStatus: Verified

---

### TC-002: Format "City, Street" z dodatkowymi spacjami
**Input:**
- Street: "Kraków ,  ul. Floriańska"
- City: (puste)

**Oczekiwany rezultat:**
- City: "Kraków"
- Street: "ul. Floriańska"
- VerificationStatus: Verified

---

### TC-003: Format bez przecinka (nie powinien być rozdzielany)
**Input:**
- Street: "Warszawa Aleje Jerozolimskie"
- City: "Warszawa"

**Oczekiwany rezultat:**
- City: "Warszawa"
- Street: "Warszawa Aleje Jerozolimskie" (bez zmian)
- VerificationStatus: Verified

---

### TC-004: Wiele przecinków w formacie
**Input:**
- Street: "Gdańsk, ul. Długa, Stare Miasto"
- City: (puste)

**Oczekiwany rezultat:**
- City: "Gdańsk"
- Street: "ul. Długa, Stare Miasto"
- VerificationStatus: Verified

---

## 2. Normalizacja City - Pierwszy krok

### TC-005: Jednoznaczne dopasowanie miasta
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- PostalCode: "00-001"

**Oczekiwany rezultat:**
- City: "Warszawa" (znormalizowane)
- VerificationStatus: Verified
- Województwo: "MAZOWIECKIE"
- Powiat: "Warszawa"
- Gmina: "Warszawa"

---

### TC-006: Miasto z małymi literami
**Input:**
- City: "warszawa"
- Street: "Marszałkowska"

**Oczekiwany rezultat:**
- City: "Warszawa" (znormalizowane)
- VerificationStatus: Verified

---

### TC-007: Miasto z błędami ortograficznymi (podobne)
**Input:**
- City: "Warszwa" (brak 'a')
- Street: "Marszałkowska"
- PostalCode: "00-001"

**Oczekiwany rezultat:**
- City: "Warszawa" (znormalizowane na podstawie podobieństwa)
- VerificationStatus: Verified
- BrokenRules: [] (lub info o korekcie)

---

### TC-008: Miasto nieistniejące
**Input:**
- City: "Nieistniejące Miasto"
- Street: "Jakaś ulica"

**Oczekiwany rezultat:**
- VerificationStatus: NotVerified
- BrokenRules: ["City not found in WASKO database"]

---

## 3. Wieloznaczność City - Wiele miast o tej samej nazwie

### TC-009: Wieloznaczność City - Dąbrowa (2 różne województwa)
**Input:**
- City: "Dąbrowa"
- Street: "Lipowa"
- PostalCode: (puste)

**Oczekiwany rezultat:**
- VerificationStatus: InVerification
- BrokenRules: [
    {
      "severity": "Info",
      "message": "Multiple cities found: Dąbrowa in OPOLSKIE and KUJAWSKO-POMORSKIE",
      "matches": [
        {"City": "Dąbrowa", "Województwo": "OPOLSKIE", "Street": "Lipowa", "CECHA": "ul."},
        {"City": "Dąbrowa", "Województwo": "KUJAWSKO-POMORSKIE", "Street": "Lipowa", "CECHA": "al."}
      ]
    }
  ]

---

### TC-010: Wieloznaczność City rozwiązana przez Street
**Input:**
- City: "Dąbrowa"
- Street: "Lipowa"
- PostalCode: (puste)
- **Dodatkowy kontekst:** Użytkownik wybiera "OPOLSKIE"

**Oczekiwany rezultat:**
- City: "Dąbrowa"
- Street: "Lipowa"
- CECHA: "ul."
- Województwo: "OPOLSKIE"
- VerificationStatus: Verified

---

### TC-011: Wieloznaczność City rozwiązana przez PostalCode
**Input:**
- City: "Zakrzewo"
- Street: "Kujawska"
- PostalCode: "87-700" (dla woj. kujawsko-pomorskie)

**Oczekiwany rezultat:**
- City: "Zakrzewo"
- Województwo: "KUJAWSKO-POMORSKIE"
- VerificationStatus: Verified

---

## 4. Normalizacja Street - Drugi krok

### TC-012: Jednoznaczne dopasowanie ulicy
**Input:**
- City: "Lublin" (już znormalizowane)
- Street: "Krakowskie Przedmieście"
- PostalCode: "20-001"

**Oczekiwany rezultat:**
- Street: "Krakowskie Przedmieście"
- VerificationStatus: Verified
- SYM_UL: (odpowiedni kod)

---

### TC-013: Ulica z różnymi cechami - ul. vs pl.
**Input:**
- City: "Chełm"
- Street: "Żwirki i Wigury"
- PostalCode: (puste)

**Oczekiwany rezultat:**
- VerificationStatus: InVerification
- BrokenRules: [
    {
      "severity": "Info",
      "message": "Multiple street types found: ul. Żwirki i Wigury and pl. Żwirki i Wigury",
      "matches": [
        {"Street": "Żwirki i Wigury", "CECHA": "ul.", "SYM_UL": "26608"},
        {"Street": "Żwirki i Wigury", "CECHA": "pl.", "SYM_UL": "26607"}
      ]
    }
  ]

---

### TC-014: Ulica z różnymi cechami - ul. vs rondo
**Input:**
- City: "Lublin"
- Street: "Dmowskiego"
- PostalCode: (puste)

**Oczekiwany rezultat:**
- VerificationStatus: InVerification
- BrokenRules: [
    {
      "severity": "Info",
      "message": "Multiple street types found: ul. Dmowskiego and rondo Dmowskiego",
      "matches": [
        {"Street": "Dmowskiego", "CECHA": "ul.", "SYM_UL": "03868"},
        {"Street": "Dmowskiego", "CECHA": "rondo", "SYM_UL": "29979"}
      ]
    }
  ]

---

### TC-015: Ulica z różnymi cechami - ul. vs os.
**Input:**
- City: "Tomaszów Mazowiecki"
- Street: "Górna"
- PostalCode: (puste)

**Oczekiwany rezultat:**
- VerificationStatus: InVerification
- BrokenRules: [
    {
      "severity": "Info",
      "message": "Multiple street types found: ul. Górna and os. Górna",
      "matches": [
        {"Street": "Górna", "CECHA": "ul.", "SYM_UL": "05948"},
        {"Street": "Górna", "CECHA": "os.", "SYM_UL": "05947"}
      ]
    }
  ]

---

### TC-016: Ulica z różnymi cechami - ul. vs park
**Input:**
- City: "Tomaszów Mazowiecki"
- Street: "Rodego"
- PostalCode: (puste)

**Oczekiwany rezultat:**
- VerificationStatus: InVerification
- BrokenRules: [
    {
      "severity": "Info",
      "message": "Multiple street types found: ul. Rodego and park Rodego",
      "matches": [
        {"Street": "Rodego", "CECHA": "ul.", "NAZWA_2": "Doktora Jana Serafina", "SYM_UL": "48560"},
        {"Street": "Rodego", "CECHA": "park", "NAZWA_2": "im.", "SYM_UL": "18712"}
      ]
    }
  ]

---

### TC-017: Ulica z NAZWA_2 - różne warianty
**Input:**
- City: "Tomaszów Mazowiecki"
- Street: "Hubala"
- NAZWA_2: "mjr" (użytkownik wpisał skrót)

**Oczekiwany rezultat:**
- VerificationStatus: InVerification (lub Verified jeśli algorytm rozpozna "mjr" = "mjra")
- BrokenRules: [
    {
      "severity": "Info",
      "message": "Multiple variants found: os. Hubala mjr. and ul. Hubala mjra",
      "matches": [
        {"Street": "Hubala", "CECHA": "os.", "NAZWA_2": "mjr.", "SYM_UL": "06770"},
        {"Street": "Hubala", "CECHA": "ul.", "NAZWA_2": "mjra", "SYM_UL": "48565"}
      ]
    }
  ]

---

### TC-018: Ulica nieistniejąca w danym mieście
**Input:**
- City: "Warszawa" (znormalizowane)
- Street: "Nieistniejąca Ulica"
- PostalCode: "00-001"

**Oczekiwany rezultat:**
- VerificationStatus: NotVerified
- BrokenRules: ["Street 'Nieistniejąca Ulica' not found in Warszawa"]

---

## 5. Normalizacja PostalCode - Trzeci krok

### TC-019: Poprawny kod pocztowy
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- PostalCode: "00-001"

**Oczekiwany rezultat:**
- PostalCode: "00-001" (znormalizowany)
- VerificationStatus: Verified

---

### TC-020: Kod pocztowy bez myślnika
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- PostalCode: "00001"

**Oczekiwany rezultat:**
- PostalCode: "00-001" (znormalizowany z myślnikiem)
- VerificationStatus: Verified

---

### TC-021: Kod pocztowy ze spacją zamiast myślnika
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- PostalCode: "00 001"

**Oczekiwany rezultat:**
- PostalCode: "00-001" (znormalizowany)
- VerificationStatus: Verified

---

### TC-022: Kod pocztowy niepasujący do ulicy
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- PostalCode: "11-222" (niepasujący kod)

**Oczekiwany rezultat:**
- VerificationStatus: InVerification
- BrokenRules: [
    {
      "severity": "Info",
      "message": "Postal code 11-222 does not match street Marszałkowska in Warszawa"
    }
  ]

---

### TC-023: Wieloznaczność - przykład z wymagań
**Input:**
- City: (nieznane)
- Street: "4-go maja"
- PostalCode: "11-222"

**WASKO zawiera:**
- "1-go maja" z kodem "11 222"
- "3-go maja" z kodem "11 223"

**Oczekiwany rezultat:**
- VerificationStatus: InVerification
- BrokenRules: [
    {
      "severity": "Info",
      "message": "Multiple matches found for similar street name",
      "matches": [
        {"Street": "1-go maja", "PostalCode": "11-222", "similarity": 0.85},
        {"Street": "3-go maja", "PostalCode": "11-223", "similarity": 0.85}
      ]
    }
  ]

---

## 6. Enrich Address - Wzbogacanie pól administracyjnych

### TC-024: Poprawne wzbogacenie wszystkich pól
**Input:**
- City: "Brodnica"
- Street: "Długa"
- PostalCode: "87-300"

**Oczekiwany rezultat:**
- Province (Województwo): "KUJAWSKO-POMORSKIE"
- District (Powiat): "brodnicki"
- Commune (Gmina): "Brodnica"
- Gmina Rodzaj: "gmina miejska"
- PostLocation (Poczta): (z WASKO)
- GeoLocation: (z WASKO)
- VerificationStatus: Verified

---

### TC-025: Wzbogacenie dla gminy wiejskiej
**Input:**
- City: "Zakrzewo"
- Street: "Kujawska"
- PostalCode: "87-700"

**Oczekiwany rezultat:**
- Province: "KUJAWSKO-POMORSKIE"
- District: "aleksandrowski"
- Commune: "Zakrzewo"
- Gmina Rodzaj: "gmina wiejska"
- VerificationStatus: Verified

---

## 7. Building Number Validation

### TC-026: Numer budynku w zakresie
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- BuildingNumber: "15"

**WASKO zawiera:** numery 1-100 dla tej ulicy

**Oczekiwany rezultat:**
- BuildingNumber: "15"
- VerificationStatus: Verified

---

### TC-027: Numer budynku poza zakresem
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- BuildingNumber: "150"

**WASKO zawiera:** numery 1-100 dla tej ulicy

**Oczekiwany rezultat:**
- VerificationStatus: InVerification
- BrokenRules: [
    {
      "severity": "Warning",
      "message": "Building number 150 exceeds range 1-100 for street Marszałkowska"
    }
  ]

---

### TC-028: Numer budynku dla importu (poza zakresem)
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- BuildingNumber: "150"
- **Kontekst:** Import danych (batch)

**Oczekiwany rezultat:**
- VerificationStatus: NotVerified
- BuildingNumber: "150" (zachowany)
- BrokenRules: [
    {
      "severity": "Info",
      "message": "Building number 150 exceeds range 1-100 for street Marszałkowska"
    }
  ]
- **Uwaga:** Brak warning dla importu (zgodnie z wymaganiami)

---

### TC-029: Numer budynku dla ręcznego dodawania (poza zakresem)
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- BuildingNumber: "150"
- **Kontekst:** Ręczne dodawanie (manual entry)

**Oczekiwany rezultat:**
- VerificationStatus: (nie zmieniamy - pozostaje jak było)
- BrokenRules: [
    {
      "severity": "Warning",
      "message": "Building number 150 exceeds range 1-100 for street Marszałkowska"
    }
  ]
- **Uwaga:** Zwracamy warning, ale nie zmieniamy statusu

---

## 8. Edge Cases i przypadki specjalne

### TC-030: Puste wszystkie pola
**Input:**
- City: ""
- Street: ""
- PostalCode: ""

**Oczekiwany rezultat:**
- VerificationStatus: NotVerified
- BrokenRules: ["City is required", "Street is required"]

---

### TC-031: Tylko City (bez Street)
**Input:**
- City: "Warszawa"
- Street: ""
- PostalCode: ""

**Oczekiwany rezultat:**
- City: "Warszawa" (znormalizowane)
- VerificationStatus: PartiallyVerified (lub odpowiedni status)
- BrokenRules: ["Street is required for full verification"]

---

### TC-032: Tylko Street (bez City)
**Input:**
- City: ""
- Street: "Marszałkowska"
- PostalCode: ""

**Oczekiwany rezultat:**
- VerificationStatus: NotVerified
- BrokenRules: ["City is required"]

---

### TC-033: Miasto z polskimi znakami diakrytycznymi
**Input:**
- City: "Łódź"
- Street: "Piotrkowska"

**Oczekiwany rezultat:**
- City: "Łódź" (znormalizowane, zachowane znaki)
- VerificationStatus: Verified

---

### TC-034: Ulica z polskimi znakami diakrytycznymi
**Input:**
- City: "Warszawa"
- Street: "Żurawia"

**Oczekiwany rezultat:**
- Street: "Żurawia" (znormalizowane, zachowane znaki)
- VerificationStatus: Verified

---

### TC-035: Miasto z wieloma słowami
**Input:**
- City: "Biała Podlaska"
- Street: "Dmowskiego"

**Oczekiwany rezultat:**
- City: "Biała Podlaska" (znormalizowane)
- VerificationStatus: Verified

---

### TC-036: Ulica z wieloma słowami
**Input:**
- City: "Warszawa"
- Street: "Krakowskie Przedmieście"

**Oczekiwany rezultat:**
- Street: "Krakowskie Przedmieście" (znormalizowane)
- VerificationStatus: Verified

---

### TC-037: Ulica z numerem w nazwie
**Input:**
- City: "Brodnica"
- Street: "700-lecia"

**Oczekiwany rezultat:**
- Street: "700-lecia" (znormalizowane)
- VerificationStatus: Verified

---

### TC-038: Ulica z skrótem w NAZWA_2
**Input:**
- City: "Brodnica"
- Street: "Janaszka"
- NAZWA_2: "Jana"

**Oczekiwany rezultat:**
- Street: "Janaszka"
- NAZWA_2: "Jana" (znormalizowane)
- VerificationStatus: Verified

---

### TC-039: Ulica z pełnym imieniem w NAZWA_2
**Input:**
- City: "Brodnica"
- Street: "Moniuszki"
- NAZWA_2: "Stanisława"

**Oczekiwany rezultat:**
- Street: "Moniuszki"
- NAZWA_2: "Stanisława" (znormalizowane)
- VerificationStatus: Verified

---

### TC-040: Różne warianty zapisu tej samej ulicy
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska" (użytkownik wpisał)
- **WASKO zawiera:** "ul. Marszałkowska"

**Oczekiwany rezultat:**
- Street: "Marszałkowska" (znormalizowane, bez "ul.")
- CECHA: "ul."
- VerificationStatus: Verified

---

## 9. Testy integracyjne - Workflow

### TC-041: Pełny workflow - ręczne dodawanie
**Input:**
- City: "warszawa" (małe litery)
- Street: "Warszawa, Marszałkowska" (format do rozdzielenia)
- PostalCode: "00001" (bez myślnika)
- BuildingNumber: "15"

**Oczekiwany rezultat:**
- City: "Warszawa" (znormalizowane)
- Street: "Marszałkowska" (rozdzielone i znormalizowane)
- PostalCode: "00-001" (znormalizowane)
- BuildingNumber: "15"
- Province: "MAZOWIECKIE"
- District: "Warszawa"
- Commune: "Warszawa"
- VerificationStatus: Verified
- BrokenRules: []

---

### TC-042: Pełny workflow - import danych
**Input:**
- City: "lublin"
- Street: "Krakowskie Przedmieście"
- PostalCode: "20 001"
- BuildingNumber: "25"

**Oczekiwany rezultat:**
- City: "Lublin" (znormalizowane)
- Street: "Krakowskie Przedmieście" (znormalizowane)
- PostalCode: "20-001" (znormalizowane)
- BuildingNumber: "25"
- Province: "LUBELSKIE"
- District: "Lublin"
- Commune: "Lublin"
- VerificationStatus: Verified (lub NotVerified jeśli import)
- BrokenRules: []

---

### TC-043: Workflow z wieloznacznością - wymaga interwencji użytkownika
**Input:**
- City: "Dąbrowa"
- Street: "Lipowa"
- PostalCode: (puste)

**Oczekiwany rezultat:**
- VerificationStatus: InVerification
- BrokenRules: [wieloznaczność]
- **UI:** Wyświetl sugestie do wyboru przez użytkownika
- **Po wyborze użytkownika:**
  - VerificationStatus: Verified
  - Wszystkie pola wzbogacone

---

## 10. Testy wydajnościowe i graniczne

### TC-044: Duża liczba dopasowań
**Input:**
- City: "Warszawa"
- Street: "Główna" (bardzo popularna nazwa)

**Oczekiwany rezultat:**
- VerificationStatus: InVerification
- BrokenRules: [lista wszystkich dopasowań]
- **Uwaga:** System powinien obsłużyć wiele dopasowań bez spadku wydajności

---

### TC-045: Bardzo długie nazwy
**Input:**
- City: "Warszawa"
- Street: "Powstańców Wielkopolskich" (długa nazwa)

**Oczekiwany rezultat:**
- Street: "Powstańców Wielkopolskich" (znormalizowane)
- VerificationStatus: Verified

---

### TC-046: Specjalne znaki w nazwach
**Input:**
- City: "Warszawa"
- Street: "PCK" (skrót)

**Oczekiwany rezultat:**
- Street: "PCK" (znormalizowane)
- VerificationStatus: Verified

---

## 11. Testy zgodności z istniejącymi regułami walidacji

### TC-047: Zgodność z istniejącymi regułami walidacji
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- PostalCode: "00-001"
- **Kontekst:** Istniejące reguły walidacji wymagają kodu pocztowego

**Oczekiwany rezultat:**
- VerificationStatus: Verified
- **Uwaga:** Normalizacja nie powinna kolidować z istniejącymi regułami

---

### TC-048: Konflikt z istniejącymi regułami
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- PostalCode: "00-001"
- **Kontekst:** Istniejące reguły walidacji wymagają dodatkowego pola "Region"

**Oczekiwany rezultat:**
- VerificationStatus: Verified (normalizacja)
- BrokenRules: ["Region field is required"] (z istniejących reguł)
- **Uwaga:** Oba systemy powinny działać niezależnie

---

## 12. Testy API i komunikacji z WASKO

### TC-049: Sukces API WASKO
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"

**Oczekiwany rezultat:**
- API Response: 200 OK
- VerificationStatus: Verified
- Wszystkie pola wzbogacone

---

### TC-050: Błąd API WASKO - timeout
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- **Kontekst:** API WASKO nie odpowiada

**Oczekiwany rezultat:**
- VerificationStatus: NotVerified
- BrokenRules: ["WASKO API timeout - address not verified"]
- **Uwaga:** System powinien obsłużyć błąd gracefully

---

### TC-051: Błąd API WASKO - 500 Internal Server Error
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- **Kontekst:** API WASKO zwraca 500

**Oczekiwany rezultat:**
- VerificationStatus: NotVerified
- BrokenRules: ["WASKO API error - address not verified"]
- **Uwaga:** System powinien logować błąd i informować użytkownika

---

### TC-052: Błąd API WASKO - 401 Unauthorized
**Input:**
- City: "Warszawa"
- Street: "Marszałkowska"
- **Kontekst:** Błędne dane uwierzytelniające

**Oczekiwany rezultat:**
- VerificationStatus: NotVerified
- BrokenRules: ["WASKO API authentication error"]
- **Uwaga:** System powinien logować błąd bezpieczeństwa

---

## Podsumowanie

**Łączna liczba przypadków testowych:** 52

**Kategorie:**
- Street Format Check: 4 przypadki
- Normalizacja City: 4 przypadki
- Wieloznaczność City: 3 przypadki
- Normalizacja Street: 7 przypadków
- Normalizacja PostalCode: 5 przypadków
- Wzbogacanie pól: 2 przypadki
- Walidacja numeru budynku: 4 przypadki
- Edge Cases: 11 przypadków
- Testy integracyjne: 3 przypadki
- Testy wydajnościowe: 3 przypadki
- Zgodność z regułami: 2 przypadki
- Testy API: 4 przypadki

**Priorytety:**
- **Highest:** TC-001, TC-005, TC-009, TC-012, TC-019, TC-024, TC-041, TC-049
- **High:** TC-013, TC-014, TC-015, TC-023, TC-026, TC-043
- **Medium:** Pozostałe przypadki

