# Przypadki Testowe - Błędne Dopasowania Normalizatora

## Opis

Ten plik zawiera **50 dodatkowych przypadków testowych** (TC-051 do TC-100) skupionych na sytuacjach, gdzie normalizator mógłby popełnić błąd i dokonać złego przypisania adresu.

## Główne kategorie błędów

### 1. Popularne nazwy ulic (TC-051 do TC-053)
**Problem:** Niektóre nazwy ulic (np. "Lipowa", "Długa", "Polna") występują w wielu miejscowościach.

**Ryzyko:** Normalizator może przypisać adres do innego miasta z tą samą nazwą ulicy.

**Przykłady:**
- "Lipowa" występuje w 19+ miejscowościach
- "Długa" występuje w 19+ miejscowościach  
- "Polna" występuje w 19+ miejscowościach

### 2. Podobne nazwy miast (TC-054 do TC-055)
**Problem:** Miasta o podobnych nazwach mogą być źle dopasowane.

**Ryzyko:** Normalizator może przypisać do miasta o podobnej nazwie.

**Przykłady:**
- Bolesławiec vs Bolesławice
- Zakrzewo vs Zakrzew

### 3. Błędy w pisowni (TC-056 do TC-058)
**Problem:** Błędy ortograficzne mogą prowadzić do dopasowania do innego miasta/ulicy.

**Ryzyko:** Algorytm podobieństwa może dopasować do złego adresu.

**Przykłady:**
- "Brodnic" (brak 'a') może dopasować do innego miasta
- "Lipow" (brak 'a') może dopasować do innej ulicy
- "Marszałkowsk" (brak 'a') może dopasować do innej ulicy

### 4. Kody pocztowe (TC-059 do TC-060)
**Problem:** Podobne lub błędne kody pocztowe mogą prowadzić do przypisania do innego miasta.

**Ryzyko:** Normalizator może przypisać do miasta z podobnym kodem.

**Przykłady:**
- Kod 87-301 vs 87-300 (Brodnica)
- Kod 20-001 (Warszawa) vs 87-300 (Brodnica)

### 5. Wieloznaczność - automatyczne rozstrzygnięcie (TC-061 do TC-062, TC-086 do TC-087)
**Problem:** Automatyczne rozstrzygnięcie wieloznaczności może wybrać zły wariant.

**Ryzyko:** System może automatycznie wybrać ul. zamiast pl., lub odwrotnie.

**Przykłady:**
- Chełm: ul. vs pl. "Żwirki i Wigury"
- Lublin: ul. vs rondo "Dmowskiego"
- Dąbrowa: OPOLSKIE vs KUJAWSKO-POMORSKIE

### 6. Skróty (TC-063 do TC-064)
**Problem:** Skróty mogą być źle rozszyfrowane.

**Ryzyko:** Może dopasować do innej ulicy zawierającej skrót w innej formie.

**Przykłady:**
- "PCK" może być źle rozszyfrowane
- "ul." w nazwie ulicy może być źle zinterpretowane

### 7. Nazwy z liczbami (TC-065 do TC-066)
**Problem:** Nazwy z liczbami mogą być źle zinterpretowane.

**Ryzyko:** Może dopasować do ulicy z numerem lub datą osobno.

**Przykłady:**
- "700-lecia" może być zinterpretowane jako "700" lub "lecia"
- "3 Maja" vs "Konstytucji 3 Maja"

### 8. Wielosłowność (TC-067 do TC-068)
**Problem:** Długie nazwy mogą być dopasowane tylko częściowo.

**Ryzyko:** Może dopasować tylko do części nazwy.

**Przykłady:**
- "Powstańców Wielkopolskich" może być dopasowane tylko do "Powstańców"
- "Biała Podlaska" może być dopasowane tylko do "Biała"

### 9. Polskie znaki diakrytyczne (TC-069 do TC-070)
**Problem:** Polskie znaki mogą być źle zinterpretowane.

**Ryzyko:** Może dopasować do wersji bez polskich znaków.

**Przykłady:**
- "Łąkowa" może być dopasowane do "Lakowa"
- "Łódź" może być dopasowane do "Lodz"

### 10. NAZWA_2 (TC-071 do TC-072)
**Problem:** Różne warianty NAZWA_2 mogą być źle dopasowane.

**Ryzyko:** Może dopasować do innego wariantu z innym imieniem.

**Przykłady:**
- "Moniuszki" bez imienia vs "Moniuszki Stanisława"
- "Hubala mjr" vs "Hubala mjra"

### 11. Podobne nazwy ulic (TC-073 do TC-075)
**Problem:** Podobne nazwy ulic mogą być źle dopasowane.

**Ryzyko:** Może dopasować do ulicy o bardzo podobnej nazwie.

**Przykłady:**
- "Marszałkowska" vs "Marszałkowskiego"
- "Warszawska" występuje w wielu miastach
- "Krakowska" występuje w wielu miastach

### 12. Częściowe dopasowanie (TC-076 do TC-078, TC-097 do TC-099)
**Problem:** Brak niektórych danych może prowadzić do błędnego dopasowania.

**Ryzyko:** Może przypisać do innego miasta/ulicy jeśli brak danych weryfikujących.

**Przykłady:**
- Tylko ulica i miasto (bez kodu pocztowego)
- Tylko miasto (bez ulicy)
- Tylko ulica (bez miasta)

### 13. Kolejność normalizacji (TC-079 do TC-080)
**Problem:** Zła kolejność normalizacji może prowadzić do błędnego dopasowania.

**Ryzyko:** Jeśli normalizuje ulicę przed miastem (lub odwrotnie), może przypisać do złego adresu.

### 14. Algorytm podobieństwa (TC-081 do TC-083)
**Problem:** Algorytm podobieństwa może dopasować do złego adresu.

**Ryzyko:** Zbyt wysokie lub zbyt niskie podobieństwo może prowadzić do błędów.

### 15. Threshold (TC-084 do TC-085)
**Problem:** Zbyt niski lub zbyt wysoki threshold może prowadzić do błędów.

**Ryzyko:**
- Zbyt niski: może zaakceptować błędne dopasowanie
- Zbyt wysoki: może odrzucić poprawne dopasowanie

### 16. Case sensitivity (TC-088 do TC-089)
**Problem:** Wielkość liter może prowadzić do błędnego dopasowania.

**Ryzyko:** Może nie dopasować lub dopasować do złego jeśli case-sensitive.

**Przykłady:**
- "LIPOWA" vs "Lipowa"
- "LiPoWa" vs "Lipowa"

### 17. Białe znaki (TC-090 do TC-091)
**Problem:** Dodatkowe białe znaki mogą prowadzić do błędnego dopasowania.

**Ryzyko:** Może nie dopasować lub dopasować do złego jeśli białe znaki nie są obsłużone.

**Przykłady:**
- "  Lipowa  " (dodatkowe spacje)
- "Lipowa    15" (wielokrotne spacje)

### 18. Format kodu pocztowego (TC-092 do TC-093)
**Problem:** Różne formaty kodu mogą prowadzić do błędnego dopasowania.

**Ryzyko:** Może dopasować do innego miasta jeśli format nie jest znormalizowany.

**Przykłady:**
- "87300" (bez myślnika)
- "87 300" (ze spacją)

### 19. Numer budynku (TC-094 do TC-095)
**Problem:** Numer budynku może prowadzić do błędnego dopasowania.

**Ryzyko:** Może dopasować do innej ulicy jeśli numer jest poza zakresem.

### 20. Kolejność pól (TC-096)
**Problem:** Zła kolejność pól w request może prowadzić do błędnego dopasowania.

**Ryzyko:** Może przypisać miasto do ulicy i odwrotnie.

### 21. Edge cases (TC-100)
**Problem:** Puste wszystkie pola może prowadzić do błędnego dopasowania.

**Ryzyko:** Może przypisać do domyślnego lub pierwszego znalezionego adresu.

## Kolumna RYZYKO_BLEDU

Każdy przypadek testowy zawiera kolumnę `RYZYKO_BLEDU`, która opisuje konkretne ryzyko błędnego dopasowania dla danego przypadku.

## Rekomendacje

1. **Weryfikacja wieloznaczności:** Zawsze zwracać `InVerification` gdy występuje wieloznaczność, nie rozstrzygać automatycznie.

2. **Weryfikacja kodów pocztowych:** Używać kodów pocztowych jako dodatkowego kryterium weryfikacji.

3. **Threshold:** Ustawić odpowiedni threshold dla algorytmu podobieństwa (zalecane: 0.85-0.9).

4. **Normalizacja białych znaków:** Zawsze usuwać dodatkowe białe znaki przed normalizacją.

5. **Case-insensitive:** Algorytm powinien być case-insensitive.

6. **Kolejność normalizacji:** Najpierw normalizować miasto, potem ulicę, potem kod pocztowy.

7. **Weryfikacja numerów budynków:** Sprawdzać zakres numerów budynków dla każdej ulicy.

8. **Logowanie:** Logować wszystkie przypadki wieloznaczności i niskich prawdopodobieństw.

## Statystyki

- **Łączna liczba przypadków:** 50 (TC-051 do TC-100)
- **Priorytet Highest:** 15 przypadków
- **Priorytet High:** 30 przypadków
- **Priorytet Medium:** 5 przypadków

## Użycie

Te przypadki testowe powinny być używane do:
1. Testowania algorytmów normalizacji
2. Weryfikacji poprawności dopasowań
3. Identyfikacji potencjalnych błędów w systemie
4. Optymalizacji threshold i algorytmów podobieństwa

