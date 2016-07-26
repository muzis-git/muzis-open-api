# Открытое API
API Muzis предоставляет следующие возможности:
  - Поиск по трекам и исполнителям по названию трека или исполнителя, тексту трека, значению.
  - Создание релевантного списка треков по другому треку или исполнителю
  - Создание релевантного списка треков по списку слов (по тексту песен)
  - Создание релевантного списка треков по значению или значениям.
  - Получение списка похожих исполнителей.

  
# Общие правила
  - Все POST запросы выполняются отностильно домена muzis.ru
  - Все файлы(mp4,jpg) относительно f.muzis.ru

# Обьекты
###Song
=======================
| id           | id трека                           |
|--------------|------------------------------------|
| type         | тип обьекта  (для song всегда 2)   |
| track_name   | название трека                     |
| performer    | название исполнителя               |
| poster       | постер                             |
| timestudy    | длина трека в мс                   |
| performer_id | id исполнителя                     |
| performers   | ids исполнителей входящих в состав |
| lyrics       | текст трека                        |
| file_mp4     | аудио файл в формате mp4           |

###Performer
=======================
| id           | id трека                           |
|--------------|------------------------------------|
| type         | тип обьекта  (для performer всегда 3)   |
| title    | название исполнителя               |
| poster       | постер                             |


# Поиск трека/исполнителя
Возможен поиск по названию трека,  исполнителю, тексту трека, значению
###Запрос:
=======================
| =Путь =  	| /api/search.api 	|
|----------	|---------------------	|
| =Метод = 	| POST                	|

###Параметры запроса:
=======================
| Поле        | Обязательность  | Тип    | Пример  | Описание                               | Требования                                                                                                  |
|-------------|-----------------|--------|---------|----------------------------------------|-------------------------------------------------------------------------------------------------------------|
| q_track     | Нет             | string | abc     | поиск по названию трека/исполнителя    | мин. 2 символа                                                                                               |
| q_performer | Нет             | string | abc     | поиск по названию исполнителя          | мин. 2 символа                                                                                               |
| q_lyrics    | Нет             | string | abc,cba | поиск по тексту в словах через запятую | мин. 2 символа                                                                                               |
| q_value     | Нет             | string | 10,12   | поиск по значению через запятую        | мин. 2 символа                                                                                               |
| size        | Нет             | Int    | 100     |                                        | 0..200                                                                                                      |
| offset      | Нет             | Int    | 50      |                                        |                                                                                                             |
| sort        | Нет             | string | id      |                                        | из 'timestudy', 'poster', 'id','performers','track_name', 'performer', 'performer_id', 'file_mp4', 'lyrics' |

Возвращаемые данные:
```
{
    'songs':[<Song>,...],
    'performers':[<Performer>,...]
}
```

# Релевантный список треков по обьекту (треку/исполнителю)
Получение списка трэков на основе исполнителя или трека.
Запрос строится на id трэка или исполнителя и типе.
Если необходимо получить релевантный список по трэку отправляем type:2,id:id_трека
Если необходимо получить релевантный список по исполнителю отправляем type:3,id:id_исполнителя


###Запрос:
=======================
| =Путь =  	| /api/stream_from_obj.api 	|
|----------	|---------------------	|
| =Метод = 	| POST                	|

###Параметры запроса:
=======================
| Поле | Обязательность | Тип | Пример | Описаине                 | Требования |
|------|----------------|-----|--------|--------------------------|------------|
| type | Да             | int | 2      | тип обьекта              | 2,3        |
| id   | Да             | int | 33301  | id обьекта               |            |
| size | Нет            | int | 100    | количество треков        | 0...200    |

Возвращаемые данные:
```
{
    'songs':[<Song>,...]
}
```

# Релевантный список треков по тексту
Получение релевантного списка трэков на основе слов в треках
###Запрос:
=======================
| =Путь =  	| /api/stream_from_lyrics.api 	|
|----------	|---------------------	|
| =Метод = 	| POST                	|

###Параметры запроса:
=======================
| Поле     | Обязательность | Тип    | Пример              | Описаине                                  | Требования        |
|----------|----------------|--------|---------------------|-------------------------------------------|-------------------|
| lyrics   | Да             | string | любовь:80,да:30,дом | искомые слова через запятую с приоритетом | слово:вес(0..100) |
| size     | Нет            | int    | 100                 | количество треков                         | 0...200           |
| operator | Нет            | string | OR                  | OR поиск или, AND поиск и                 | OR,AND            |

Возвращаемые данные:
```
{
    'songs':[<Song>,...]
}
```

# Релевантный список треков по значению
Получение релевантного списка трэков на основе значений (жанра или др.)

###Запрос:
=======================
| =Путь =  	| /api/stream_from_values.api 	|
|----------	|---------------------	|
| =Метод = 	| POST                	|

###Параметры запроса:
=======================
| Поле     | Обязательность | Тип    | Пример      | Описаине                                  | Требования              |
|----------|----------------|--------|-------------|-------------------------------------------|-------------------------|
| values   | Да             | string | 10:80,12:30 | искомые слова через запятую с приоритетом | id_значения:вес(0..100) |
| size     | Нет            | int    | 100         | количество треков                         | 0...200                 |
| operator | Нет            | string | OR          | OR поиск или, AND поиск и                 | OR,AND                  |

Возвращаемые данные:
```
{
    'songs':[<Song>,...]
}
```

# Получение списка похожих исполнителей

###Запрос:
=======================
| =Путь =  	| /api/similar_performers.api 	|
|----------	|---------------------	|
| =Метод = 	| POST                	|

###Параметры запроса:
=======================
| Поле     | Обязательность | Тип    | Пример      | Описаине                                  | Требования              |
|----------|----------------|--------|-------------|-------------------------------------------|-------------------------|
| performer_id   | Да             | int | 1001 | id исполнителя |  |
| size     | Нет            | int    | 100         |                           | 0...200                 |


Возвращаемые данные:
```
{
    'performers':[<Performer>,...]
}
```


# Возвращаемые ошибки

- HTTP 403 - Доступ запрещен 

- HTTP 404 - Не найден обьект. Попытка получения списка трэков на несуществующий обьект.

Ответ содержит поле error:
```
{
    'error':{'id':402},
    'head':...,
    'postdata':...,
}
```
неверно указано одно из полей в POST запросе, неверные поля указаны в error




# Музыкальные зачения
 Поиск и генерация возможны на основе музыкальных значений, список значений перечислен ниже.
 
###Значения жанра
=======================
| Значение(value) | Описание                     |
|-----------------|------------------------------|
| 32496           | Эстрада                      |
| 32450           | Нью-эйдж                     |
| 32213           | Swedish synthpop             |
| 27482           | New-рop                      |
| 32456           | Фанк                         |
| 32448           | Соул                         |
| 32218           | Техно                        |
| 31959           | Dance-Pop                    |
| 10              | Поп-музыка                   |
| 269             | Поп-фолк                     |
| 31937           | КиноРемикс                   |
| 32510           | Ню-джаз                      |
| 32509           | New-wave                     |
| 14              | Поп-рок                      |
| 32508           | Инди-поп                     |
| 23877           | Роко-попс                    |
| 32453           | Эстрадный рок                |
| 31435           | Фолк-Рок                     |
| 245             | Тяжелый рок                  |
| 5258            | Рок                          |
| 32208           | Глэм-рок                     |
| 3377            | Джаз-рок                     |
| 32253           | Психоделический рок          |
| 32211           | Софт-рок                     |
| 24157           | Панк рок                     |
| 202             | Русский рок                  |
| 23560           | Альтернативный рок           |
| 32506           | gothic rock                  |
| 32507           | Блюз-рок                     |
| 32511           | Арт-рок                      |
| 32504           | Металл                       |
| 32228           | Электро-Рок                  |
| 32240           | Инди-рок                     |
| 32475           | Death/metal core             |
| 32276           | Гранж                        |
| 32244           | Дрим-рок                     |
| 32503           | Прогрессив рок               |
| 32471           | Британский рок               |
| 32495           | Симфорок                     |
| 32457           | РШ                           |
| 23663           | Блатная Электронная          |
| 27305           | Альтернативный -Шансон       |
| 5               | Блатная                      |
| 12344           | Поп шансон                   |
| 27854           | Сиротские песни              |
| 7               | Классика шансона             |
| 1663            | Дворовые песни               |
| 2               | Шансон                       |
| 12224           | Рок Шансон                   |
| 182             | Жестокий романс              |
| 8               | Романс                       |
| 32462           | Миллениум                    |
| 3156            | Ретро                        |
| 745             | Недалекое ретро/Сов. эстрада |
| 1328            | Фортепианная пьесса          |
| 1342            | Инструментальная музыка      |
| 5590            | Попурри                      |
| 31942           | Минус -1                     |
| 905             | Мюзикл                       |
| 31662           | Оперная Ария                 |
| 904             | Классическая                 |
| 32412           | Григорианика                 |
| 32429           | Саундтрек                    |
| 23116           | Духовная                     |
| 32340           | Trap                         |
| 27457           | R&B                          |
| 32481           | Биг-бит                      |
| 24152           | Хип-хоп                      |
| 31675           | Рэпкор                       |
| 11              | Рэп                          |
| 32426           | Трип-хоп                     |
| 32424           | Хоррор -рэп                  |
| 32423           | Рэп-подземка                 |
| 32417           | Гоп-рэп                      |
| 32418           | Рэп-лирика                   |
| 3378            | Регги                        |
| 4362            | Блюз                         |
| 4247            | Диксиленд                    |
| 84              | Рок-н-ролл                   |
| 31724           | Баркаролла                   |
| 31961           | Джаз                         |
| 5609            | Твист                        |
| 523             | Самба                        |
| 3490            | Ча-ча-ча                     |
| 542             | Диско                        |
| 31879           | Одежда                       |
| 31669           | Авторский рассказ            |
| 31758           | Мелодекламация               |
| 22812           | Стих                         |
| 927             | Бардовская                   |
| 12              | Фолк                         |
| 3443            | Кантри                       |
| 26086           | Русская народная песня       |
| 177             | Частушки                     |
| 32271           | Этно-электроника             |
| 12546           | Детские песни                |
| 32469           | Гимн                         |
| 11526           | Рождественские песни         |
| 4               | Армейские песни              |
| 21174           | Коллаж                       |
| 32389           | Dubstep                      |
| 32388           | Club Dubstep                 |
| 32390           | Liquid Dubstep               |
| 32365           | Techno                       |
| 32337           | Electro House                |
| 32326           | House                        |
| 32338           | Big Room House               |
| 32317           | Detroit House & Techno       |
| 32336           | Bass & Jackin' House         |
| 32369           | Deep House                   |
| 32376           | Oldschool House              |
| 32357           | Epic Trance                  |
| 32383           | Dark PsyTrance               |
| 32318           | Trance                       |
| 32387           | Classic Vocal Trance         |
| 32386           | Classic Trance               |
| 32319           | Vocal Trance                 |
| 32309           | Melodic Progressive          |
| 32364           | Drumstep                     |
| 32380           | Liquid DnB                   |
| 32381           | Dark DnB                     |
| 32334           | Jungle                       |
| 32351           | Drum and Bass                |
| 32328           | Minimal                      |
| 32333           | Tech House                   |
| 32316           | IDM                          |
| 32395           | Latin House                  |
| 32327           | Mainstage                    |
| 32329           | Hard Dance                   |
| 32330           | EuroDance                    |
| 32378           | Hardstyle                    |
| 32315           | Liquid Trap |
| 32356           | Oldschool Techno & Trance    |
| 32321           | Chillout                     |
| 32392           | Disco House                  |
| 32385           | Club Sounds                  |
| 32394           | Future Synthpop              |
| 32332           | UMF Radio                    |
| 32335           | Future Garage                |
| 32339           | Nightcore                    |
| 32341           | PsyChill                     |
| 32342           | Goa-Psy Trance               |
| 32343           | Progressive Psy              |
| 32344           | Bassline                     |
| 32396           | Oldschool Acid               |
| 32370           | Deep Tech |
| 32331           | Vocal Lounge                 |
| 32397           | Chill & Tropical House       |
| 32393           | Classic EuroDisco            |
| 32391           | Electropop                   |
| 32345           | Hardcore                     |
| 32322           | Vocal Chillout               |
| 32346           | Downtempo Lounge             |
| 32347           | DJ Mixes                     |
| 32348           | Russian Club Hits            |
| 32349           | Ambient                      |
| 32350           | Psybient                     |
| 32352           | Nu Disco                     |
| 32353           | Big Beat                     |
| 32354           | Dub                          |
| 32355           | EcLectronica                 |
| 32358           | 00's Club Hits               |
| 32359           | Breaks                       |
| 32360           | Gabber                       |
| 32361           | Electronics                  |
| 32362           | EBM                          |
| 32363           | Hard Techno                  |
| 32366           | Electro Swing                |
| 32367           | Electronic Pioneers          |
| 32368           | Soulful House                |
| 32372           | Funky House                  |
| 32373           | Deep Nu-Disco                |
| 32374           | Underground Techno           |
| 32375           | Oldschool Rave               |
| 32379           | Chillout Dreams              |
| 32382           | Classic EuroDance            |
| 32310           | Dub Techno                   |
| 32311           | Glitch Hop                   |
| 32312           | Future Beats                 |
| 32371           | Tribal House                 |
| 32377           | Space Dreams                 |
| 32313           | Indie Dance                  |
| 32384           | Hands Up                     |
| 32314           | Jazz House                   |
| 32320           | Lounge                       |
| 32323           | ChillHop                     |
| 32324           | Chillstep                    |
| 32325           | Progressive                  |

###Тематические значения
=======================
| Значение(value) | Описание            |
|-----------------|---------------------|
| 32500           | Фэнтези             |
| 32501           | Сказка              |
| 23508           | Любовь ПЛЮС         |
| 23509           | Любовь МИНУС        |
| 23515           | Просто Любовь       |
| 20226           | За жизнь            |
| 20229           | Сатира              |
| 20753           | Зарисовка           |
| 20230           | Бытовой             |
| 20255           | Застольный          |
| 31723           | Метафора            |
| 21954           | Сценка              |
| 20259           | Воспоминания        |
| 20256           | Образные-Ассоциации |
| 23556           | Сезоны              |
| 32492           | День                |
| 32491           | Ночь                |
| 32487           | Лето                |
| 32486           | Весна               |
| 32493           | Утро                |
| 23481           | Похмелье            |
| 20225           | Дорожный            |
| 32494           | Вечер               |
| 31670           | Рассуждения         |
| 32489           | Зима                |
| 32488           | Осень               |
| 32490           | Пляжная             |
| 12782           | Эротичная           |
| 23561           | Свобода Отношений   |
| 25544           | Секс                |
| 23471           | Девушки             |
| 20968           | Документальный      |
| 22464           | Память              |
| 32454           | Ужасы               |
| 22453           | Россия              |
| 32430           | Бунтарь             |
| 23472           | Политика            |
| 20993           | Эмигрантский        |
| 25126           | Православие         |
| 25739           | Романтика           |
| 25543           | Свобода             |
| 25201           | Раставание          |
| 25203           | Свидание            |
| 24794           | Возвращение         |
| 25357           | Побег               |
| 26255           | Разлука             |
| 21159           | Медицина            |
| 22575           | Спорт               |
| 25938           | Здоровье            |


###Значения времени создания
=======================
| Значение(value) | Описание |
|-----------------|----------|
| 32438           | 18 век   |
| 22              | 60-е     |
| 19              | 30-е     |
| 21              | 50-е     |
| 20              | 40-е     |
| 145             | 80-е     |
| 32441           | 17 век   |
| 32428           | 20 век   |
| 31658           | 2015-г   |
| 32186           | 2016-г   |
| 32427           | 19 век   |
| 25              | 2010-е   |
| 24              | 2000-е   |
| 23              | 90-е     |
| 3908            | 2014-е   |
| 144             | 70-е     |
| 5013            | 2005-е   |

###Значения темпа
=======================
| Значение(value) | Описание         |
|-----------------|------------------|
| 16              | Средний          |
| 15              | Быстрый          |
| 32445           | Средне-Быстрый   |
| 32444           | Средне-медленный |
| 17              | Медленный        |
| 32512           | Очень быстрый    |

###Значения языка
=======================
| Значение(value) | Описание                    |
|-----------------|-----------------------------|
| 1104            | Русский                     |
| 125             | Английский                  |

