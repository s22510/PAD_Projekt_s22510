# PAD Projekt

**Autor:** Piotr Trzos, s22510

---

## Wstęp

Projekt przewiduje temperaturę oraz prędkość wiatru na podstawie dnia i wilgotności powietrza. Dane zostały pozyskane z serwisu meteorologicznego obejmującego okres 4 lat.

---

## Pliki

`PAD_Piotr_Trzos_s22510.ipynb` jest jedynym plikiem projektu. Wszystkie etapy, takie jak pozyskiwanie danych, ich obróbka, nauka modelu oraz wizualizacja, są realizowane w jednym Jupyter Notebook.

---

## Pozyskanie danych

Funkcja `get_weather_data` służy do web scrapingu danych pogodowych z [en.tutiempo.net](https://en.tutiempo.net) dla określonego zakresu dat.

### Parametry wejściowe
- **start_date**: początkowa data zakresu pobierania danych.
- **end_date**: końcowa data zakresu pobierania danych.

### Proces działania
Funkcja iteruje przez dni w zadanym zakresie dat, generując dynamiczne URL. Dane są pobierane przy użyciu `requests`, a następnie parsowane przez `BeautifulSoup`.

### Zwracane dane
- **weather_data**: zebrane dane pogodowe w postaci słowników.

---

## Czyszczenie danych

### Usuwanie jednostek i konwersja typów
- **Kolumna `Temperature`**: Usunięcie symbolu `°`, konwersja na `int`.
- **Kolumna `Humidity`**: Usunięcie `%`, konwersja na `int`.
- **Kolumna `Pressure`**: Usunięcie `hPa`, konwersja na `int`.

### Ekstrakcja prędkości wiatru
Funkcja `extract_wind_speed` używa regex do wyciągania wartości prędkości wiatru (w km/h). Brakujące dane są oznaczane jako `None` i usuwane.

### Wyodrębnianie minut i tworzenie `Datetime`
- Godziny i minuty są wyodrębniane przy pomocy regex.
- Kolumna `Datetime` łączy rok, miesiąc, dzień, godzinę, minuty i sekundy.

### Usuwanie odstających wartości
Ekstremalne wartości temperatury poza granicami percentyli (0.1% - 99.9%) są usuwane.

---

## Analiza danych

### Wizualizacje
Wewnątrz pliku znajdują się wykresy do analizy zależności pomiędzy zmiennymi:

- **Temperatura**: Rozkład zbliżony do normalnego (zakres: -15°C do 30°C).
- **Wilgotność**: Skupienie w zakresie 75%-100%.
- **Prędkość wiatru**: Większość wartości w zakresie 0-20 km/h, z nielicznymi ekstremami >30 km/h.
- **Wykres korelacji**: Przedstawia zależności pomiędzy temperaturą, wilgotnością powietrza oraz prędkością wiatru. W szczególności pozwalają one wyraźnie dostrzec związek pomiędzy temperaturą a wilgotnością powietrza, co czyni wilgotność cenną zmienną wejściową do modelu.
- **Wykres dzienny**: Przedstawia zmiany temperatury na przestrzeni godzin w ciągu jednego dnia (wybrano dzień 2024-06-05). Pozwala to zauważyć codzienne wzorce temperaturowe.
- **Wykres miesięczny**: Pokazuje, jak temperatura zmienia się w zależności od pory roku (dni i miesięcy). Dzięki temu możemy stwierdzić, że model może uwzględniać zmienność wynikającą z dnia oraz godziny dla lepszych rezultatów.

### Wnioski z analizy
- Wilgotność powietrza może być używana do nauki modelu.
- Modelowanie uzależniono od dnia i godziny dla lepszych wyników.

---

## Modelowanie

### Przygotowanie danych
- **X (wejście)**: `Hour`, `Minute`, `Month`, `Day`, `Humidity`.
- **Y (wyjście)**: `Temperature`, `Wind`.
- Dane podzielono na zestawy treningowe (80%) i testowe (20%).

### Wybór modelu
Model regresyjny z 100 estymatorami został wybrany do przewidywania temperatury i prędkości wiatru.

### Wyniki
- **Temperatura**:
  - Średni kwadratowy błąd (MSE): 2.50
  - R-kwadrat: 0.97
- **Prędkość wiatru**:
  - Średni kwadratowy błąd (MSE): 20.27
  - R-kwadrat: 0.69

---

## Instrukcja uruchomienia

### Wymagania
- **Google Colab**
- **Python**: z bibliotekami:
  - `numpy`
  - `calendar`
  - `BeautifulSoup`
  - `re`
  - `datetime`
  - `pandas`
  - `matplotlib`
  - `seaborn`
  - `scikit-learn`

### Kroki uruchomienia
1. Otwórz plik notebooka w Google Colab.
2. Uruchamiaj komórki po kolei, aby przejść przez etapy projektu.

---

## Źródła danych
Projekt korzysta z danych pochodzących z:
- [en.tutiempo.net](https://en.tutiempo.net)
