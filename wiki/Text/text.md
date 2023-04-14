### Настройка бегущей строки:
#### Общие правила
- Поддерживается 36 строк, каждая строка имеет номер '0'..'9','A'..'Z'
- "Макрос" - последовательность управляющих конструкций, каждая из которых заключена в фигурные скобки, например '{Exx}'
- Строка с индексом '0' является управляющей строкой, содержащей индексы строк, определяющие в какой последовательности они должны отображаться на матрице.
- Если строка с индексом '0' начинается с '#' - строка содержит последовательность строк к отображению, например '#123ZYX'  
  Эта последовательность задает порядок отображения строк '1','2','3','Z','Y','X' и далее по циклу
- Если строка с индексом '0' содержит только значение '##' - строки отображаются в случайном порядке
- Если строка с индексом '0' пустая или содержит обычный текст - это просто строка. Перебор строк будет выполняться последовательно по циклу.
- Если строка пустая, начинается с '-' или содержит '{-}' - строка отключена и не отображается

#### Макросы 
- **{-}**   в любом месте строки означает, что строка временно отключена, аналогично '-' в начале строки
- **{#n}**  в любом месте строки означает, что после отображения этой строки будет немедленно отображена строка с номером n, где n - 1..35 или '1'..'9','A'..'Z'
- **{An+}** при наличии MP3 DFPlayer воспроизводить звук n из папки SD://03; Если указан необязательный суффикс '+' - звук зацикливается пока отображается строка.
- **{Exx}** строка отображается на фоне эффекта XX, где XX - номер эффекта. Эффект не меняется, пока строка не будет полностью отображена
- **{Exx-n}** строка отображается на фоне эффекта XX вариант n, где XX - номер эффекта, n - номер варианта.
- **{Ts}**  отображать строку указанное количество (s) секунд
- **{Nx}**  отображать строку указанное количество (x) раз подряд
- **{Cc}**  отображать строку указанным цветом. Цвет 'c' задается в web-формате #RRGGBB  
            Если в строке только один макрос цвета текста, расположенный **в самом начале** или в **самом конце** строки - вся строка выводится указанным цветом  
            Если в строке содержится несколько макросов определения цвета или макрос задан посередине строки, строка выводится указанными цветами:  
  - от начала строки до первого макроса цвета - глобальным значением цвета, который задается в Web-приложении в настройках бегущей строки
  - от позиции макроса цвета до следующего макроса цвета или до концв строки - цветом, определенным в макросе
            Специальные значения цветов:  
  -  #000001 - радужные буквы вдоль текста  
  -  #000002 - каждая буква отдельным цветом
- **{Bc}**  отображать строку на однотонном фоне. Цвет фона 'c' - задается в формате #RRGGBB, например для черного цвета макрос имеет вид ```{B#000000}```  
- **{WS}**  вставить в строку текущее состояние погоды (ясно, пасмурно, дождь и т.п.)
- **{WT}**  вставить в строку текущую температуру
- **{D}**   отображать часы вида ЧЧ:MM на месте макроса
- **{D:f}** отображать часы в указанном формате f, гдее f может содержать в себе:  
  -    **d** или **D**       - день месяца в диапазоне от 1 до 31  
  -    **dd** или **DD**     - день месяца в диапазоне от 01 до 31  
  -    **ddd** или **DDD**   - сокращенное название дня недели (пон - вск)  
  -    **dddd** или **DDDD** - полное название дня недели (понедельник - воскресенье)  
  -    **М**                 - месяц от 1 до 12  
  -    **ММ**                - месяц от 01 до 12  
  -    **МММ**               - месяц прописью (янв - дек)  
  -    **MMMM**              - месяц прописью (января - декабря)  
  -    **y** или **Y**       - год от 0 до 99  
  -    **yy** или **YY**     - год от 00 до 99  
  -    **yyyy** или **YYYY** - год в виде четырехзначного числа  
  -    **h**                 - час в 12-часовом формате от 1 до 12  
  -    **hh**                - час в 12-часовом формате от 01 до 12  
  -    **H**                 - час в 24-часовом формате от 0 до 23  
  -    **HH**                - час в 24-часовом формате от 00 до 23  
  -    **m**                 - минуты от 0 до 59  
  -    **mm**                - минуты от 00 до 59  
  -    **s** или **S**       - секунды от 0 до 59  
  -    **ss** или **SS**     - секунды от 00 до 59  
  -    **t** или **T**       - первый символ указателя am/pm или AM/PM  
  -    **tt** или **TT**     - указатель am/pm или AM/PM  
- **{Rdate time#N}** оставшееся до события время, где  

    **date** - дата события в формате ДД.ММ.ГГГГ  
               В дате допускается использовать символы-заменители:  
               - День  `'**'`   - означает любой день  
               - Месяц `'**'`   - означает любой месяц   
               - Год   `'****'` - означает текущий год  
               - Год   `'***+'` - означает следующий год  

    **time** - время события в формате ЧЧ:ММ:CC  
               Время может быть опущено, тогда принимается 00:00:00
    **N**    - индекс строки (1..9,A..Z или 1..35) отображаемой ПОСЛЕ наступления события.  
               Если N не указана - после наступления события, строка отображаться не будет.  

  При использовании в части заменителя года значения '\*\*\*\*' или '\*\*\*+' при смене года текст "До Нового года осталось {R01.01.\*\*\*+}\#N"  
  строка '#N' никогда не будет показана, поскольку при наступлении даты события год сменится и будет выводиться текст 
  "До Нового года осталось 365 дней".  

  Чтобы избежать такого развития событий, рекомендуется макрос {R} использовать совместно с макросом {S}, ограничивающим период вывода строки:
```  
  "До Нового года осталось {R01.01.***+}{S01.12.****#31.12.**** 23:59:59}"
  "До Нового года осталось {R31.12.**** 23:59:59}{S01.12.****#31.12.**** 23:59:59}"
```
  
  Время, оставшееся до наступления события, показывается в соответствии с правилами:  
  -   осталось более 7 дней - *'XX дней'*  
  -   осталось 7 дней и менее - *'XX дней XX часов'*  
  -   осталось менее 1 дня - *'XX часов XX минут'*  
  -   осталось менее 1 часа - *'XX минут'*  
  -   осталось менее 1 минуты - *'XX секунд'*  

  Если событие уже прошло, вместо строки, содержащей макрос показывается строка-заместитель с номером N, где N - индекс 1..35 или '1'..'9','A'..'Z'.  
  Для того, чтобы строка-заместитель не показывалась как обычная строка, не связанная с событием - она должна быть "отключена" - то есть - начинаться с '-' или иметь внутри макрос отключения '{-}'.  
  Если вы забудете отключить строку-заместитель, она будет отключена автоматически при воспроизведении родительской строки-события.  
  Если строка-заместитель не указана - строка с наступившим событием не показывается.  
  
- **{Pdate time#N#B#A#W}** оставшееся до события время, где  
    **date** - дата события в формате ДД.ММ.ГГГГ  
               - дата может быть опущена, указано только время, что означает "каждый день в указанное время"  
               - любой компонент даты может быть заменен звездочкой ```'*'```, что означает "любой"  
               ```**.01.2020``` - "весь январь 2020 года"  
               ```**.01.****``` - "весь январь каждого года"  
               ```01.**.2020``` - "каждое 1-е число месяца 2020-го года"  
               ```01.09.****``` - "каждое 1-е сентября"  
               ```**.**.2020``` - "весь 2020-год, каждый день"  
    **time** - время события в формате ЧЧ:ММ  
               - если время не указано - означает 0:00  
    **N**    - индекс строки (1..9,A..Z или 1..35) отображаемой ПОСЛЕ наступления события.  
    **B**    - количество секунд ДО наступления события, когда начинает отображаться строка  
    **A**    - количество секунд ПОСЛЕ наступления события, когда отображается строка-заместитель  
    **W**    - дни недели 1-пн..7-вс когда отображается бегущая строка  

  Дата или время должны быть указаны обязательно, остальные элементы могут последовательно опускаться, тогда они будут иметь значения по умолчанию:
    - нет строки заместителя, если опущены элементы #N#B#A#W
    - количество секунд ДО события - 60, если опущены элементы #B#A#W
    - количество секунд ПОСЛЕ события - 60, если опущены элементы #A#W
    - ограничения по дням недели - '1234567', если опущен элемент #W

  Строка, содержащая макрос {P} не отображается регулярно как строки с прочими макросами.  
  Эта строка отображается только за #B секунд до наступления события, затем #A секунд ПОСЛЕ наступления события отображается строка-заместитель, 
  если в макросе присутствует элемент #N и указываемая им строка не пустая.

  Если указана конкретная дата и день недели, но указанная дата не является этим днем недели - строка отображаться не будет.  

  Для того, чтобы строка-заместитель не показывалась как обычная строка, не связанная с событием - она должна быть "отключена" - то есть - начинаться с '-' или иметь внутри макрос отключения '{-}'.  

  Пример:  
    Строка D: ```Подъем через {P7:00#E#60#60#12345}```  
    Строка E: ```-Доброе утро!```  

  Каждый будний день в 6:59 до 7:00 будет отображаться обратный отсчет до пробуждения, затем с 7:00 до 7:01 отображается надпись "Доброе утро!"

  Пример:  
    Строка D: ```Трансляция начнется через {P01.11.2020 19:30#E#180#60}```  
    Строка E: ```Все на футбол!{-}```  

  1 ноября 2020 года за три минуты до начала трансляции (в 19:27 и до 19:30) начнется обратный отсчет оставшегося до трансляции времени. 
  В 19:30 и до 19:31 будет отображаться бегущая строка с текстом "Все на футбол!"

  Пример:  
    Строка D: ```До Нового года осталось {P01.01.****#E#60#60}```  
    Строка E: ```-С Новым годом!!!!```  

  За минуту до наступления каждого Нового года начнет отображаться бегущая строка с обратным отсчетом. 
  С наступлением Нового года, пока бьют куранты 1 минуту показывается бегущая строка "С Новым годом!!!"

- **{Sdate1 time1#date2 time2}** период отображения строки, где  
    **date1** - дата начала периода в формате ДД.ММ.ГГГГ  
    **time1** - время начала периода в формате ЧЧ:ММ:CC  
    **date2** - дата окончания периода в формате ДД.ММ.ГГГГ  
    **time2** - время окончания периода в формате ЧЧ:ММ:CC  

     - дата может быть опущена, указано только время, что означает "каждый день в указанное время"  
     - любой компонент даты может быть заменен звездочкой ```'*'```, что означает "любой"  
       ```**.01.2020``` - "весь январь 2020 года"  
       ```**.01.****``` - "весь январь каждого года"  
       ```01.**.2020``` - "каждое 1-е число месяца 2020-го года"  
       ```01.09.****``` - "каждое 1-е сентября"  
       ```**.**.2020``` - "весь 2020-год, каждый день"  

     - если указана только дата начала периода - датой окончания будет считаться конец указанного дня - та же дата, время 23:59:59
     - если указана только дата окончания периода - датой начала будет считаться начала указанного дня - та же дата, время 00:00:00
     - если время начала периода не указано - означает 00:00:00  
     - если время окончания периода не указано - означает 23:59:59  
     - допускается указывать несколько макроcов {S} в строке для определения нескольких разрешенных диапазонов

  Примеры:
    ``` "С Рождеством!{S07.01.2020}"```  - один день только 7 января 2021 года (весь день с 0:00 до 23:59)
    ``` "С днем Победы!{S09.05.**** 09:00#09.05.**** 21:00}"``` - каждый год 9 мая с 9:00 до 21:00
    ``` "Ура, каникулы!!!{S01.01.****#12.01.****}{S01.04.****#08.04.****}"``` - каждый год в период с 1 по 12 января и с 1 по 8 апреля

### Номера эффектов для макроса {Exx}
- **1** - "Лампа"
- **2** - "Снегопад"
- **3** - "Кубик"
- **4** - "Радуга"
- **5** - "Пейнтбол"
- **6** - "Огонь"
- **7** - "The Matrix"
- **8** - "Шарики"
- **9** - "Звездопад"
- **10** - "Конфетти"
- **11** - "Цветной шум"
- **12** - "Облака"
- **13** - "Лава"
- **14** - "Плазма"
- **15** - "Радужные переливы"
- **16** - "Полосатые переливы"
- **17** - "Зебра"
- **18** - "Шумящий лес"
- **19** - "Морской прибой"
- **20** - "Смена цвета"
- **21** - "Светлячки"
- **22** - "Водоворот"
- **23** - "Циклон"
- **24** - "Мерцание"
- **25** - "Северное сияние"
- **26** - "Тени"
- **27** - "Лабиринт"
- **28** - "Змейка"
- **29** - "Тетрис"
- **30** - "Арканоид"
- **31** - "Палитра"
- **32** - "Спектрум"
- **33** - "Синусы"
- **34** - "Вышиванка"
- **35** - "Дождь"
- **36** - "Камин"
- **37** - "Стрелки"
- **38** - "Анимация"
- **39** - "Погода"
- **40** - "Жизнь"
- **41** - "Узоры"
- **42** - "Кубик Рубика"
- **43** - "Звезды"
- **44** - "Штора"
- **45** - "Трафик"
- **46** - "Слайды"
- **47** - "Рассвет"
- **48** - Эффекты с SD карты