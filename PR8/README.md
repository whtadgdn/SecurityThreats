# Использование Yandex Query для анализа данных сетевой активности
Zid4a84@yandex.ru

## Исходные данные

1.  Программное обеспечение ОС Windows 11 Pro

2.  RStudio

3.  Интерпретатор языка R 4.5.1

4.  Доступ к экосистеме Yandex Query


## Шаги:

``` r
sessionInfo()
```

    R version 4.5.1 (2025-06-13 ucrt)
    Platform: x86_64-w64-mingw32/x64
    Running under: Windows 11 x64 (build 26200)

    Matrix products: default
    LAPACK version 3.12.1

    locale:
    [1] LC_COLLATE=Russian_Russia.utf8  LC_CTYPE=Russian_Russia.utf8    LC_MONETARY=Russian_Russia.utf8
    [4] LC_NUMERIC=C                    LC_TIME=Russian_Russia.utf8    
    
    time zone: Europe/Moscow
    tzcode source: internal
    
    attached base packages:
    [1] stats     graphics  grDevices utils     datasets  methods   base      

    loaded via a namespace (and not attached):
     [1] compiler_4.5.1    fastmap_1.2.0     cli_3.6.5         tools_4.5.1      
     [5] htmltools_0.5.8.1 rstudioapi_0.17.1 yaml_2.3.10       rmarkdown_2.30   
     [9] knitr_1.50        jsonlite_2.0.0    xfun_0.53         digest_0.6.37    
    [13] rlang_1.1.6       evaluate_1.0.5   

### 1.Используя сервис Yandex DataLens настроить доступ к Yandex Query,который Вы использовали в ходе ранее выполненных практических работ,и визуально представить результаты анализа данных.

![](img/1.png)

### 2.Представить в виде круговой диаграммы соотношение внешнего и внутреннего сетевого трафика.

Для создания крговой диаграммы необходимо создать новую метрику, которая
будет принимать значениние принадлежности трафика к внутреннему или
внешнему. По условию задания внутренняя сетевая адресация начианется с
12./13./14., зная это создаем новое поле измерения подчиняющееся
следующему правилу:

<img width="1275" height="450" alt="image" src="https://github.com/user-attachments/assets/f57af7aa-8d8c-4290-bfbc-ab06c537bbd1" />

Для данного поля создаем счетчик

<img width="1200" height="446" alt="image" src="https://github.com/user-attachments/assets/a2868c72-ba25-40ac-bf14-389359c637d6" />


На основании полученного поля строим круговую диаграмму:

<img width="3439" height="1308" alt="image" src="https://github.com/user-attachments/assets/bd0298bb-6e39-4369-8b01-e8b7b8962c7d" />


### 3.Представить в виде столбчатой диаграммы соотношение входящего и исходящего трафика из внутреннего сетвого сегмента.

Для создания столбчатой диаграммы необходимо создать новую метрику,
которая будет принимать значениние принадлежности трафика к внутреннему
или внешнему. По условию задания внутренняя сетевая адресация начианется
с 12./13./14., зная это создаем новое поле измерения подчиняющееся
следующему правилу:

<img width="1199" height="427" alt="image" src="https://github.com/user-attachments/assets/83827f12-edb6-4ad9-8055-97cdaf731d47" />


Так же на основе этого правила создадим два счетчика:

<img width="1231" height="504" alt="image" src="https://github.com/user-attachments/assets/6c98a958-11eb-40c6-87d8-1bc33bc09428" />


<img width="1253" height="490" alt="image" src="https://github.com/user-attachments/assets/6b5f66a9-ac9c-415c-a290-4c52558c6339" />

Итоговая диаграмма имеет следующий вид: <img width="3439" height="1316" alt="image" src="https://github.com/user-attachments/assets/b785fb78-2874-4317-b2ba-e2ab56fdea74" />

<img width="3439" height="1299" alt="image" src="https://github.com/user-attachments/assets/403fe5a3-1031-4248-b2ac-4da4fe2aa1c2" />


### 4.Построить график активности (линейная диаграмма) объема трафика во времени.

Для создания линейной диаграммы необходимо создать несколько метрик.
Первая из них будет приводть число формата timstamp в дробное число
обозначающее время в микросекундах с момента начала отсчета. Формула
данной метрики будет иметь слудующий вид:

<img width="1219" height="596" alt="image" src="https://github.com/user-attachments/assets/48aabb43-8437-45f0-8e17-352629d8a32c" />

На основе данного измерения выведем счетчик значений в каждый момент
времени. Счетчик имеет следующий вид:

<img width="1220" height="740" alt="image" src="https://github.com/user-attachments/assets/11dd5d80-1337-46b8-bd9d-a1934be95476" />

Получаем итоговую линейную диаграмму

<img width="3439" height="1307" alt="image" src="https://github.com/user-attachments/assets/d010be70-1fc1-4955-ab52-8e6426798aff" />

Из полученных чартов формируем дашборд:

<img width="3439" height="1362" alt="image" src="https://github.com/user-attachments/assets/b87277d2-df63-43ed-8f7b-7d145f3388e6" />



Настроив параметры публичного доступа получим [ссылку на
дашборд](https://datalens.ru/8s7irf8hhvsss)

## Оценка результата

В рамках практческой работы были изучены возможности Yandex Datalens для
визуализации данных.

## Вывод

В практической работе были изучены возможности технологии Yandex
Datalens для анализа структурированных наборов данных и получены навыки
создания визуализаций в данной системе.
