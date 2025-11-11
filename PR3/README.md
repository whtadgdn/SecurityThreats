# Основы обработки данных с помощью R и Dplyr

Zid4a84@yandex.ru

## Цель работы

1.  Развить практические навыки использования языка программирования R для обработки данных
2.  Закрепить знания базовых типов данных языка R
3.  Развить практические навыки использования функций обработки данных пакета dplyr – функции select(), filter(), mutate(), arrange(), group_by()


---

## Исходные данные

1. Программное обеспечение: **OC Windows 11 Pro**  
2. Среда разработки: **RStudio**  
3. Интерпретатор языка: **R 4.5.1**  

---

## План

Научиться работать с языком R.


## Шаги

### Установка nycflights13
> install.packages('nycflights13')
WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

https://cran.rstudio.com/bin/windows/Rtools/
Устанавливаю пакет в ‘C:/Users/Zid4a/AppData/Local/R/win-library/4.5’
(потому что ‘lib’ не определено)
пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/nycflights13_1.0.2.zip'
Content type 'application/zip' length 4511548 bytes (4.3 MB)
downloaded 4.3 MB

пакет ‘nycflights13’ успешно распакован, MD5-суммы проверены

Скачанные бинарные пакеты находятся в
	C:\Users\Zid4a\AppData\Local\Temp\RtmpOU8nnE\downloaded_packages


### Подключение пакетов
```{r}
library(nycflights13)
library(dplyr)
```	

### Сколько встроенных в пакет nycflights13 датафреймов?
```{r}
length(ls("package:nycflights13"))
```


### Сколько строк в каждом датафрейме?
```{r}
sapply(ls("package:nycflights13"), function(df) nrow(get(df, "package:nycflights13")))
```
      
### Сколько столбцов в каждом датафрейме?
```{r}
sapply(ls("package:nycflights13"), function(df) ncol(get(df, "package:nycflights13")))
```


### Как просмотреть примерный вид датафрейма?
```{r}
glimpse(airlines)
```


### Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных (представлено в наборах данных)?
```{r}
length(unique(nycflights13::flights$carrier))
```


### Сколько рейсов принял аэропорт John F Kennedy Intl в мае?
```{r}
nycflights13::flights %>%
      filter(dest == "JFK", month == 5) %>%
      summarise(flights_count = n())
```


### Какой самый северный аэропорт?
```{r}
nycflights13::airports %>%
      filter(lat == max(lat, na.rm = TRUE))
```


### Какой аэропорт самый высокогорный (находится выше всех над уровнем моря)?
```{r}
airports %>%
  filter(alt == max(alt, na.rm = TRUE))
```


### Какие бортовые номера у самых старых самолетов?
```{r}
planes %>%
  filter(!is.na(year)) %>%
  arrange(year) %>%
  select(tailnum, year) %>%
  head(10)
```


### Какая средняя температура воздуха была в сентябре в аэропорту John F Kennedy Intl (в градусах Цельсия).
```{r}
weather %>%
  filter(origin == "JFK", month == 9) %>%
  summarise(mean_temp_c = (mean(temp, na.rm = TRUE) - 32)* 5/9) %>%
  pull(mean_temp_c)
```


### Самолеты какой авиакомпании совершили больше всего вылетов в июне?
```{r}
june_flights <- flights %>%
  filter(month == 6)

# 2) посчитать вылеты по каждому перевозчику
carrier_counts <- june_flights %>%
  group_by(carrier) %>%
  summarise(departures = n()) %>%
  arrange(desc(departures))

# 3) вывести лидера(-ей)
top_carriers <- head(carrier_counts, n = 1)

top_carriers
```


### Самолеты какой авиакомпании задерживались чаще других в 2013 году?
```{r}
library(nycflights13)
library(dplyr)

flights %>%
  filter(year == 2013, dep_delay > 0) %>%          # задержки в 2013, dep_delay > 0 минут
  group_by(carrier) %>%
  summarise(mean_delays = mean(dep_delay, na.rm = TRUE),
            count_delays = sum(dep_delay > 0)) %>%
  arrange(desc(count_delays)) %>%
  head(1)
```

## Оценка результата

В результате лабораторной работы мы развили практические навыки использования языка программирования R для обработки данных и закрепили знания базовых типов данных языка R

## Вывод

Таким образом, мы научились использовать функции обработки данных пакета dplyr – функции select(), filter(), mutate(), arrange(), group_by()
