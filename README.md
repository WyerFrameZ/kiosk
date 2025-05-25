# Rozszeżenie kiosk do EduMotiv

System do obsługi kart RFID, umożliwiający automatyczną rejestrację użytkowników i odczyt danych z głownej bazy dnaych.
## Tech stack
<p align="center">
  <img src="img/html.svg" width="100" style="display:inline-block" />
  <img src="img/css.svg" width="100" style="display:inline-block" />
  <img src="img/nodejs.svg" width="100" style="display:inline-block" />
  <img src="img/python.svg" width="100" style="display:inline-block" />
  <img src="img/cpp.svg" width="100" style="display:inline-block" />
</p>

## Opis systemu

System składa się z trzech głównych komponentów:
- **Interfejs webowy** (index.html) - Panel użytkownika wyświetlający dane karty
- **Serwer API** (api_server.py) - Obsługuje żądania HTTP i komunikuje się z bazą danych
- **Skrypt RFID** (rfid_simple.py) - Komunikuje się z czytnikiem Arduino i odczytuje dane kart

## Funkcje

- Odczyt kart RFID
- Wyświetlanie danych użytkownika karty
- Automatyczna rejestracja nowych kart
- Zliczanie dni użycia karty
- Intuicyjny interfejs użytkownika

## Wymagania

- Python 3.7 lub nowszy
- Jakikolwiek mikrokontroloer AVR z czytnikiem RFID
- Przeglądarka internetowa
- Serwer MariaDB/MySQL
- Biblioteki Python: flask, flask-cors, pymysql, serial

## Instalacja

### 1. Instalacja zależności Python

```bash
pip install flask flask-cors pymysql pyserial
```

### 2. Konfiguracja bazy danych

Utwórz bazę danych MySQL/MariaDB z następującą tabelą:

```sql
CREATE TABLE uczen (
  id INT(11) NOT NULL AUTO_INCREMENT,
  imie VARCHAR(50) DEFAULT NULL,
  nazwisko VARCHAR(50) DEFAULT NULL,
  username VARCHAR(30) NOT NULL,
  legitymacja VARCHAR(140) DEFAULT NULL,
  klasa VARCHAR(10) DEFAULT NULL,
  dni INT(11) DEFAULT NULL,
  PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

### 3. Konfiguracja pliku dot.env

Utwórz w katalogu projektu plik `dot.env` z następującą zawartością:

```
IP: adres_IP_bazy_danych
MariaDB Port: 3306
User: nazwa_użytkownika_bazy
Password: hasło_do_bazy
Database: nazwa_bazy_danych
```

## Uruchomienie systemu

### Sposób 1: Automatyczne uruchomienie

Uruchom plik `start.bat`, który automatycznie uruchomi wszystkie komponenty systemu.

### Sposób 2: Ręczne uruchomienie

1. Uruchom serwer API:
```bash
python api_server.py
```

2. W nowym oknie terminala uruchom skrypt RFID:
```bash
python rfid_simple.py
```

3. Otwórz przeglądarkę pod adresem `http://localhost:5000`

## Obsługa systemu

1. **Główny ekran**: Wyświetla informacje o ostatnio zeskanowanej karcie
2. **Rejestracja nowej karty**: Po zeskanowaniu nieznanej karty system automatycznie wyświetli formularz rejestracji
3. **Rejestracja użytkownika**: Wypełnij pola formularza i kliknij "Zarejestruj"

## Rozwiązywanie problemów

### Nie mogę połączyć się z mikrokontrolerem
- Sprawdź połączenie USB
- Upewnij sie, że masz sterownik do układu ch340 usb to ttl

### Błąd połączenia z bazą danych
- Sprawdź poprawność danych w pliku dot.env
- Upewnij się, że serwer bazy danych jest uruchomiony
- Sprawdź, czy firewall nie blokuje połączenia 

## Informacje techniczne

- Odświeżanie danych: co 5 sekund
- Port API: 5000 (web)
- API jest dostępne pod adresem http://[adres-ip]:5000/api/last-card (adres jest bran yz głownej karty sieciowej)
