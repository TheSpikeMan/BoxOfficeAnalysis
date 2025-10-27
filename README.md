# Pipeline ETL i Analiza Box Office Filmów

## Opis Projektu

Projekt realizuje kompletny proces ETL (Extract, Transform, Load). Dane dotyczące dziennych przychodów filmów są pobierane z pliku `revenues_per_day.csv`, następnie wzbogacane o szczegółowe informacje z zewnętrznego OMDb API. Oczyszczone i ustrukturyzowane dane są modelowane zgodnie z technikami modelowania wymiarowego (schemat gwiazdy), a na końcu prezentowane w formie interaktywnego dashboardu w Power BI.

### Kluczowe cechy
* Zbudowanie w pełni funkcjonalnego pipeline'u danych w Pythonie.
* Integracja z zewnętrznym REST API (OMDb API) z obsługą kluczy.
* Zaawansowane czyszczenie i transformacja danych przy użyciu biblioteki `pandas`.
* Implementacja prostego modelu wymiarowego (tabela faktów i dwie tabele wymiarów).
* Stworzenie dashboardu analitycznego w Power BI.

## Zastosowane Technologie
* **Język:** Python 3.10+
* **Biblioteki:** `pandas`, `requests`, `sqlalchemy`
* **Baza Danych:** SQLite (używana w skrypcie)
* **Wizualizacja:** Power BI

## Podjęte Decyzje Projektowe

* **Ograniczenie Zakresu Danych**: Z uwagi na limit 1000 zapytań API dziennie, pipeline został zaprojektowany tak, aby analizować **Top 300 filmów** o najwyższym łącznym przychodzie. Gwarantuje to, że analizie poddawany jest najbardziej znaczący biznesowo podzbiór danych, jednocześnie szanując limit API.
* **Model Danych**: Zastosowano prosty **schemat gwiazdy** składający się z:
    * `fact_table` (tabela faktów): Zawiera klucze obce i metrykę `revenue`.
    * `fact_genre` (tabela faktów): Zawiera mapowania filmów do gatunków filmów.
    * `dim_movies` (wymiar filmu): Zawiera szczegółowe, wzbogacone atrybuty filmów z API.
    * `dim_date` (wymiar daty): Zawiera atrybuty daty ułatwiające analizę czasową.
    * `dim_genres` (wymiar gatunku filmu): Zawiera gatunki filmów
* **Format Danych Wyjściowych**: Zgodnie z założeniami zadania, jako lekki silnik hurtowni danych wybrano SQLite. Finalny model (wymiary i tabela faktów) został załadowany do pliku .db, tworząc w pełni funkcjonalny i przenośny model danych. Połączenie z Power BI zostało zrealizowane za pomocą sterownika ODBC.
* **Czyszczenie Danych**: Zaimplementowano szereg kroków czyszczących, m.in. standaryzację nazw filmów (usunięcie spacji, ujednolicenie wielkości liter), konwersję kolumn liczbowych (np. `BoxOffice`) poprzez usunięcie znaków nienumerycznych oraz obsługę brakujących wartości (`N/A`).