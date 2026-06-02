# YouTube Trending Videos — System Analityczny

Projekt zaliczeniowy z Big Data. System analizuje trendujące filmy YouTube z 5 krajów (PL, US, GB, DE, FR).

## Architektura

```
Warstwa 1: Ingestion   → YouTube API → JSON + CSV + Parquet
Warstwa 2: Data Lake   → Czyszczenie + Feature Engineering → Parquet
Warstwa 3: Hurtownia   → DuckDB (schemat gwiazdy) + kwerendy + Excel
Warstwa 4: ML (bonus)  → Random Forest + XGBoost (predykcja popularności)
Warstwa 5: Wizualizacje→ Plotly (interaktywne wykresy)
```

## Szybki start

### 1. Wymagania wstępne
- **Python 3.10+** — pobierz z [python.org/downloads](https://python.org/downloads)
  - ⚠️ Przy instalacji zaznacz ✅ **"Add Python to PATH"**
- **Visual Studio Code** z rozszerzeniem **Jupyter**

### 2. Instalacja środowiska
Otwórz terminal w VS Code (`Ctrl+`` ` ``) i wykonaj po kolei:

```powershell
# Przejdź do folderu projektu (dostosuj ścieżkę)
cd "C:\ścieżka\do\big data"

# Utwórz wirtualne środowisko
python -m venv .venv

# Zainstaluj wszystkie zależności
.venv\Scripts\pip install -r requirements.txt

# Zarejestruj kernel dla Jupyter
.venv\Scripts\python.exe -m ipykernel install --user --name "big-data-youtube" --display-name "Big Data YouTube"
```

### 3. Wybór kernela w VS Code
1. Otwórz dowolny plik `.ipynb`
2. Kliknij nazwę kernela w prawym górnym rogu
3. Wybierz → **Big Data YouTube** (`.venv\Scripts\python.exe`)

### 4. Klucz YouTube API
- Wejdź na https://console.cloud.google.com
- APIs & Services → Enable APIs → YouTube Data API v3
- Credentials → Create Credentials → API Key
- Utwórz plik `.env` w folderze projektu:
  ```
  YOUTUBE_API_KEY=twój_klucz_tutaj
  ```

### 5. Uruchomienie (kolejność)
Uruchom notebooki po kolei:
1. `01_ingestion.ipynb`      — pobiera dane z YouTube API
2. `02_transformation.ipynb` — czyści dane i tworzy cechy
3. `03_warehouse.ipynb`      — buduje hurtownię DuckDB
4. `04_ml.ipynb`             — trenuje modele ML
5. `05_visualization.ipynb`  — generuje wykresy

## Struktura katalogów

```
big-data-youtube/
├── notebooks/
│   ├── 01_ingestion.ipynb
│   ├── 02_transformation.ipynb
│   ├── 03_warehouse.ipynb
│   ├── 04_ml.ipynb
│   └── 05_visualization.ipynb
├── data/
│   ├── raw/          ← JSON + CSV z YouTube API
│   ├── parquet/      ← Data Lake (Parquet)
│   └── warehouse/    ← DuckDB + Excel
├── requirements.txt
└── README.md
```

## Rozwiązywanie problemów

### ❌ "requires the ipykernel package" lub Python -1.-1.-1
Folder `.venv` jest nieprzenosny między komputerami. Na nowym PC wykonaj:
```powershell
cd "C:\ścieżka\do\big data"
Remove-Item -Recurse -Force .venv
python -m venv .venv
.venv\Scripts\pip install -r requirements.txt
.venv\Scripts\python.exe -m ipykernel install --user --name "big-data-youtube" --display-name "Big Data YouTube"
```

### ❌ "python is not recognized"
Python nie jest zainstalowany lub nie jest w PATH.  
Pobierz z [python.org/downloads](https://python.org/downloads) → przy instalacji zaznacz ✅ **"Add Python to PATH"** → zrestartuj VS Code.

### ❌ "pip is not recognized"
Użyj pełnej ścieżki do pip z venv:
```powershell
.venv\Scripts\pip install -r requirements.txt
```

### ❌ "FileNotFoundError: trending_all.csv"
Nie uruchomiono najpierw `01_ingestion.ipynb`. Uruchom notebooki w kolejności od 01 do 05.

### ❌ Kernel zawiesza się przy ML (notebook 04)
XGBoost próbuje wykryć GPU. Upewnij się że w notebooku 04 parametry XGBoost zawierają:
```python
tree_method="hist", device="cpu"
```

## Dane wyjściowe

| Plik | Opis |
|------|------|
| `data/raw/trending_*.json` | Surowe dane per region |
| `data/raw/trending_all.csv` | Wszystkie dane w CSV |
| `data/parquet/trending_raw.parquet` | Surowe dane kolumnowo |
| `data/parquet/trending_clean.parquet` | Dane po transformacji |
| `data/warehouse/youtube_dw.duckdb` | Hurtownia danych |
| `data/warehouse/wyniki.xlsx` | Wyniki kwerend w Excelu |


program developer 
Azenko Oleksandr 
Artem Boiko