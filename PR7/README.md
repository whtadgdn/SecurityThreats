# Использование технологии Yandex Query для анализа данных сетевой активности
Zid4a84@yandex.ru

## Цель работы

1.  Изучить возможности технологии Yandex Query для анализа
    структурированных наборов данных
2.  Получить навыки построения аналитического пайплайна для анализа
    данных с помощью сервисов Yandex Cloud
3.  Закрепить практические навыки использования SQL для анализа данных
    сетевой активности в сегментированной корпоративной сети

## Исходные данные

1.  Программное обеспечение ОС Windows 11 Pro
2.  RStudio
3.  Интерпретатор языка R 4.5.2

## План

1.  Используя сервис Yandex Query настроить доступ к данным, хранящимся
    в сервисе хранения данных Yandex Object Storage.
2.  Проверьте доступность данных (файл yaqry_dataset.pqt) в бакете
    arrow-datasets S3 хранилища Yandex Object Storage.
3.  Известно, что IP адреса внутренней сети начинаются с октетов,
    принадлежащих интервалу \[12-14\]. Определите количество хостов
    внутренней сети, представленных в датасете.
4.  Определите суммарный объем исходящего трафика.
5.  Определите суммарный объем входящего трафика.

## Шаги:

### Загрузка библиотек

``` r
options(repos = c(CRAN = "https://mirror.truenetwork.ru/CRAN/"))
install.packages("arrow")
```

WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

https://cran.rstudio.com/bin/windows/Rtools/
Устанавливаю пакет в ‘C:/Users/Zid4a/AppData/Local/R/win-library/4.5’
(потому что ‘lib’ не определено)
пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/arrow_22.0.0.zip'
Content type 'application/zip' length 19858512 bytes (18.9 MB)
downloaded 18.9 MB

пакет ‘arrow’ успешно распакован, MD5-суммы проверены

Скачанные бинарные пакеты находятся в
	C:\Users\Zid4a\AppData\Local\Temp\RtmpUduRjn\downloaded_packages
	
``` r
install.packages("dplyr")
```

WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

https://cran.rstudio.com/bin/windows/Rtools/
Устанавливаю пакет в ‘C:/Users/Zid4a/AppData/Local/R/win-library/4.5’
(потому что ‘lib’ не определено)
пробую URL 'https://mirror.truenetwork.ru/CRAN/bin/windows/contrib/4.5/dplyr_1.1.4.zip'
Content type 'application/zip' length 1593482 bytes (1.5 MB)
downloaded 1.5 MB

пакет ‘dplyr’ успешно распакован, MD5-суммы проверены

Скачанные бинарные пакеты находятся в
	C:\Users\Zid4a\AppData\Local\Temp\RtmpUduRjn\downloaded_packages

``` r
library(arrow)
```

Присоединяю пакет: ‘arrow’

Следующий объект скрыт от ‘package:utils’:

    timestamp

Предупреждение:
пакет ‘arrow’ был собран под R версии 4.5.2 

``` r
library(dplyr)
```

Присоединяю пакет: ‘dplyr’

Следующие объекты скрыты от ‘package:stats’:

    filter, lag

Следующие объекты скрыты от ‘package:base’:

    intersect, setdiff, setequal, union

Предупреждение:
пакет ‘dplyr’ был собран под R версии 4.5.2 

### Импортируйте данные в R.

``` r
dataset_url <- "https://storage.yandexcloud.net/arrow-datasets/yaqry_dataset.pqt"
local_filename <- "yaqry_dataset.pqt"
if (!file.exists(local_filename)) {
  download.file(url = dataset_url, destfile = local_filename, mode = "wb")
}
network_data <- open_dataset(local_filename, format = "parquet")
```

### Определите количество хостов внутренней сети, представленных в датасете.

``` r
unique_internal_src <- network_data %>%
  filter(grepl("^(12|13|14)", src)) %>%
  distinct(src) %>%
  select(host = src)
unique_internal_dst <- network_data %>%
  filter(grepl("^(12|13|14)", dst)) %>%
  distinct(dst) %>%
  select(host = dst)
all_unique_internal_hosts <- union(unique_internal_src, unique_internal_dst)
num_internal_hosts <- all_unique_internal_hosts %>%
  count() %>%
  pull(n)

``` r
num_internal_hosts
```

    [1] 1000

<img width="3437" height="1359" alt="image" src="https://github.com/user-attachments/assets/5e988532-0f9d-4c22-9fcd-c05540a8725f" />


### Определите суммарный объем исходящего трафика.

``` r
total_outgoing_bytes <- network_data %>%
  filter(grepl("^(12|13|14)", src)) %>%
  summarise(total = sum(bytes, na.rm = TRUE)) %>%
  pull(total)
total_outgoing_bytes
```

    integer64
    [1] 10007506588

<img width="3439" height="1323" alt="image" src="https://github.com/user-attachments/assets/3ea4b093-866c-4abc-b11a-d4d0e5177eea" />



### Определите суммарный объем входящего трафика.

``` r
total_ingoing_bytes <- network_data %>%
  filter(grepl("^(12|13|14)", dst)) %>%
  summarise(total = sum(bytes, na.rm = TRUE)) %>%
  pull(total)
total_ingoing_bytes
```

    integer64
    [1] 15740490053

<img width="3438" height="1318" alt="image" src="https://github.com/user-attachments/assets/ca9d8da3-cba3-4740-a694-0fd393e9e603" />

