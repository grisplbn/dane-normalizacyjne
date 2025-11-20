# Address Normalizer - Test Execution

## Overview
Run test suite for address normalization API. Check if address matching, ambiguity handling, and error handling work correctly.

## Test Files
- **Main file**: `Output/przypadki_testowe_automation.csv` (1,167 tests)
- **Simple version** (no probabilities): `Output/przypadki_testowe_automation_simple.csv` (1,167 tests)
- **Full docs**: `Output/przypadki_testowe_kompletne.csv` (649 tests with extra info)

## Test Coverage

### Test Types
- **Ambiguity** (637 tests - 54.6%): Tests for ambiguous addresses (street vs square, same city name in different places)
- **Wrong matches** (149 tests - 12.8%): Tests when system picks wrong address
- **Bad combinations** (180 tests - 15.4%): Tests with wrong input combinations
- **Street prefixes** (97 tests - 8.3%): Tests for ul., al., pl., rondo prefixes in REQUEST_streetName
- **Formatting** (65 tests - 5.6%): Tests for data format and normalization
- **Building numbers** (13 tests - 1.1%): Tests for building number validation
- **Real data** (10 tests - 0.9%): Tests based on real data from source files
- **Other** (16 tests - 1.4%): API tests, edge cases, workflow, format checks

### Priority
- **High**: 671 tests (57.5%)
- **Medium**: 308 tests (26.4%)
- **Low**: 146 tests (12.5%)
- **Highest**: 42 tests (3.6%)

## Test Structure
- **Base tests**: 453 (TC-001 to TC-649)
- **Variants**: 714 (TC-XXX-1, TC-XXX-2, etc.) - extra tests for ambiguous cases
- **Total**: 1,167 tests

## Main Test Areas

### 1. Ambiguity
- Same city name in different places (e.g., "Dąbrowa" in 2 voivodeships)
- Same street name with different types (ul. vs pl. vs rondo)
- Tests with and without postal codes
- Tests with and without street prefixes in REQUEST_streetName

### 2. Street Prefixes
- Street name with prefix (ul. Marszałkowska)
- Street name without prefix (Marszałkowska)
- Wrong prefix (ul. when only pl. exists)
- System should remove prefix in EXPECTED_streetName

### 3. Error Cases
- Similar city names (Bolesławiec vs Bolesławice)
- Popular street names in many cities (Leśna in 3,393 cities)
- NAZWA_2 variations (different name endings)
- Empty or incomplete input

### 4. Input Types
- Street + City + Postal Code: 710 tests (60.8%)
- Street + City: 349 tests (29.9%)
- Street + City + Postal Code + Building Number: 100 tests (8.6%)
- Street only: 7 tests (0.6%)

## Test File Format

### Input Fields (REQUEST_*)
- `REQUEST_streetName`: Street name (may have prefix like ul., al., pl.)
- `REQUEST_buildingNumber`: Building number
- `REQUEST_city`: City name
- `REQUEST_postalCode`: Postal code

### Expected Output Fields (EXPECTED_*)
- `EXPECTED_streetName`: Street name without prefix
- `EXPECTED_buildingNumber`: Building number
- `EXPECTED_city`: City name
- `EXPECTED_postalCode`: Postal code
- `EXPECTED_province`: Voivodeship
- `EXPECTED_district`: District (powiat)
- `EXPECTED_commune`: Commune (gmina)
- `EXPECTED_streetProbability`: Street match probability (only in full version)
- `EXPECTED_cityProbability`: City match probability (only in full version)
- `EXPECTED_postalCodeProbability`: Postal code match probability (only in full version)
- `EXPECTED_combinedProbability`: Combined match probability (only in full version)

## What to Check
- All tests run successfully
- API returns correct values in EXPECTED_* fields
- Ambiguity cases handled correctly or flagged
- Street prefixes removed in output
- Errors handled properly
- Probability values correct (where used)

## Notes
- Tests sorted by: Category → Priority → TC_ID
- Variants (TC-XXX-N) are extra tests for ambiguous cases
- Each test has unique input (one input → one output)
- Tests based on real Polish address data (ULIC, TERC, SIMC)

