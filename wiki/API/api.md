## Общие понятия

Управление устройством выполняется путем отправки ему команды через один из доступных каналов - UDP или MQTT.  
Не имеет значения по какому каналу устройство получило команду - обработка выполняется одинаково.  
Команда представляет собой управляющую последовательность, начинающуюся с символа `'$'`, далее идет код операции и параметры.  
Завершается команда символом `';'`. Код команды и параметры разделены символом `' '`(пробел), состав параметров - зависит от конкретной команды.  

```
Команда: $<код> <параметры>; 
```

## Команды управления

    1 - Включение / выключение устройства
      - $1 0; - выключить
      - $1 1; - включить
      - $1 2; - переключить

    2 - Настройка типа и размера матрицы, угла ее подключения, количество сегментов
       - $2 M0 M1 M2 M3 M4 M5 M6 M7 M8 M9 M11
             M0  - ширина сегмента матрицы 1..127
             M1  - высота сегмента матрицы 1..127
             M2  - тип сегмента матрицы - 0 - зигзаг; 1 _ параллельная
             M3  - угол подключения диодов в сегменте: 0 - левый нижний, 1 - левый верхний, 2 - правый верхний, 3
             M4  - направление ленты из угла сегмента: 0 - вправо, 1 - вверх, 2 - влево, 3 - вниз
             M5  - количество сегментов в ширину составной матрицы
             M6  - количество сегментов в высоту составной матрицы
             M7  - соединение сегментов составной матрицы: 0 - зигзаг, 1 - параллельная
             M8  - угол 1-го сегмента мета-матрицы: 0 - левый нижний, 1 - левый верхний, 2 - правый верхний, 3 - правый нижний
             M9  - направление следующих сегментов мета-матрицы из угла: 0 - вправо, 1 - вверх, 2 - влево, 3 - вниз
             M11 - Индекс текущей используемой карты индексов

    3 - управление играми из Web-интерфейса
      - $3 0;    - включить на устройстве демо-режим
      - $3 1;    - включить игру "Лабиринт" в режиме ожидания начала игры
      - $3 2;    - включить игру "Змейка" в режиме ожидания начала игры
      - $3 3;    - включить игру "Тетрис" в режиме ожидания начала игры
      - $3 4;    - включить игру "Арканоид" в режиме ожидания начала игры
      - $3 10    - кнопка вверх
      - $3 11    - кнопка вправо
      - $3 12    - кнопка вниз
      - $3 13    - кнопка влево
      - $3 14    - центральная кнопка джойстика (ОК)
      - $3 15 N; - Скорость повтора нажатия кнопки * 10 (10 .. 100) => 100..1000 мс 

    4 - общая яркость
     - $4 0 D; установить текущий уровень общей яркости  
            D - яркость в диапазоне 1..255

    5 - рисование
      - $5 0 RRGGBB; - установить активный цвет рисования в формате RRGGBB
      - $5 1;        - очистить матрицу (заливка черным)
      - $5 2;        - заливка матрицы активным активным цветом рисования 
      - $5 3 X Y;    - рисовать точку активным цветом рисования в позицию X Y
      - $5 4 T N;    - запрос текущей картинки с матрицы в телефон: T - 0 - запрос строки с номером N; T - 1- запрос колонки с номером N
      - $5 5 X;      - запрос списка файлов картинок X: 0 - с FS, 1 - SD

    6 - передача текстовых параметров в устройство
     - $6 N|text     где N - назначение текста:
     - $6 0|текст  - текст бегущей строки "$6 0|X|text" - X - 0..9,A..Z - индекс строки, text - сохраняемый текст
     - $6 1|текст  - имя сервера NTP
     - $6 2|текст  - SSID сети подключения
     - $6 3|текст  - пароль для подключения к сети 
     - $6 4|текст  - имя точки доступа
     - $6 5|текст  - пароль к точке доступа
     - $6 6|текст  - Настройки будильника в формате "$6 6|DD EF WD AD HH1 MM1 HH2 MM2 HH3 MM3 HH4 MM4 HH5 MM5 HH6 MM6 HH7 MM7"
                       DD    - установка продолжительности рассвета (рассвет начинается за DD минут до установленного времени будильника)
                       EF    - установка эффекта, который будет использован в качестве рассвета
                       WD    - установка дней пн-вс как битовая маска
                       AD    - продолжительность "звонка" сработавшего будильника
                       HHx   - часы дня недели x (1-пн..7-вс)
                       MMx   - минуты дня недели x (1-пн..7-вс)
     - $6 7|текст  - строка запрашиваемых текущих значений параметров, 
                     например - "$6 7|CE|CC|CO|CK|NC|SC|C1|DC|DD|DI|NP|NT|NZ|NS|DW|OF" или
                                "$6 7|CE CC CO CK NC SC C1 DC DD DI NP NT NZ NS DW OF"
     - $6 8|текст  - имя сервера MQTT
     - $6 9|текст  - имя пользователя MQTT
     - $6 10|текст - пароль к MQTT-серверу
     - $6 11|Y colorHEX,colorHEX,...,colorHEX   
                   - Отправка изображения построчно. X в строке изменяется от 0 до WIDTH-1, Y изменяется от 0 до HEIGHT-1   
                     В ответ ожидается строка json '{"L":"Y"}', обычно последовательно увеличивающийся номер ответа.   
                     При получении ответа, что строка разобрана, Y увеличивается на 1 и в устройство отправляется следующая строка  
                     Отправка строк изображения повторяется пока Y < HEIGHT  
                     Если приложение не получило ответа - через время таймаута осуществляется повторная отправка строки с последним значением Y  
     - $6 12|X colorHEX,colorHEX,...,colorHEX   
                   - Отправка изображения по колонкам. Y в строке изменяется от 0 до HEIGHT-1, Y изменяется от 0 до WIDTH-1   
                     В ответ ожидается строка json '{"C":"X"}', обычно последовательно увеличивающийся номер ответа.   
                     При получении ответа, что строка разобрана, X увеличивается на 1 и в устройство отправляется следующая колонка   
                     Отправка колонок изображения повторяется пока X < WIDTH  
                     Если приложение не получило ответа - через время таймаута осуществляется повторная отправка колонки с последним значением X  
     - $6 13|текст - префикс топика сообщения к MQTT-серверу
     - $6 14|текст - текст бегущей строки для немедленного отображения без сохранения
     - $6 15|ST|файл - Загрузить пользовательскую картинку из файла на матрицу; $6 15|ST|filename; ST - "FS" - файловая система; "SD" - карточка
     - $6 16|ST|файл - Сохранить текущее изображение с матрицы в файл $6 16|ST|filename; ST - "FS" - файловая система; "SD" - карточка
     - $6 17|ST|файл - Удалить файл изображения $6 17|ST|filename; ST - "FS" - файловая система; "SD" - карточка
     - $6 18|текст   - строка избранных эффектов в порядке следования. Каждый символ строки 0..9,A..Z,a..z - кодирует ID эффекта 0..MAX_EFFECT; Позиция в строке - очередность воспроизведения

       *** Примечание: 
           Способ отправки изображения из приложения на смартфоне - по строкам или по колонкам - определяется по меньшей строне изображения:  
           Если Высота изображения меньше ширины - изображение отправляется по колонкам слева направо, иначе изображение отправляется по строкам сверху вниз.

    8 - управление эффектами
      - $8 0 N;     - включить эффект N
      - $8 1 N D;   - D -> параметр #1 для эффекта N;
      - $8 3 N D;   - D -> параметр #2 для эффекта N;
      - $8 4 N X;   - вкл/выкл оверлей текста поверх эффекта; N - номер эффекта, X=0 - выкл X=1 - вкл 
      - $8 5 N X;   - вкл/выкл оверлей часов поверх эффекта; N - номер эффекта, X=0 - выкл X=1 - вкл 
      - $8 6 N D;   - D -> контрастность эффекта N;
      - $8 7 N;     - Запрос текущих параметров эффекта N, возвращается JSON с параметрами. Используется для редактирования эффекта в Web-интерфейсе;
      - $8 8 N D;   - D -> скорость эффекта N;

    11 - Настройки MQTT-канала (см. также $6 для N=8,9,10,13)
      - $11 1 X;    - использовать управление через MQTT сервер; X=0 - не использовать; X=1 - использовать
      - $11 2 D;    - порт MQTT
      - $11 5;      - Разорвать подключение к MQTT серверу, чтобы он мог переподключиться с новыми параметрами

    12 - Настройки погоды
      - $12 3 X;    - использовать цвет для отображения температуры в дневных часах X=0 - выкл X=1 - вкл
      - $12 4 X;    - использовать получение погоды с погодного сервера 0 - выкл; 1 - Yandex; 2 - OpenWeatherMap
      - $12 5 I С C2; - интервал получения погоды с сервера в минутах (I) и код региона C - Yandex и код региона C2 - OpenWeatherMap
      - $12 6 X;    - использовать цвет для отображения температуры в ночных часах X=0 - выкл X=1 - вкл 

    13 - Настройки бегущей cтроки  
      - $13 1 N;    - активация прокручивания строки с номером N 0..35
      - $13 2 N;    - запросить текст бегущей строки с индексом N 0..35 как есть, без обработки макросов - ответ - параметр "TY" (см. таблицу параметров ниже)
      - $13 9 D;    - сохранить настройку D - интервал в секундах отображения бегущей строки
      - $13 11 X;   - режим цвета бегущей строки X: 0,1,2; 0 - цвет из команды $13 15; 1 - "Радуга по ширине матрицы"; 2 - каждая буква своим цветом.
      - $13 13 X;   - скорость прокрутки бегущей строки
      - $13 15 00FFAA; - цвет текстовой строки для режима "монохромный", сохраняемый в globalTextColor
      - $13 18 X;   - сохранить настройку X "Бегущая строка в эффектах" (общий, для всех эффектов), где X: 0 - отключено; 1 - включено

    14 - быстрая установка ручных режимов с пред-настройками
      - $14 0;      - Черный экран (выкл);  
      - $14 1;      - Белый экран (освещение);  
      - $14 2;      - Цветной экран;  
      - $14 3;      - Огонь;  
      - $14 4;      - Конфетти;  
      - $14 5;      - Радуга;  
      - $14 6;      - Матрица;  
      - $14 7;      - Светлячки;  
      - $14 8;      - Часы ночные;
      - $14 9;      - Часы бегущей строкой;
      - $14 10;     - Часы простые;  

    15 - скорость таймеров
      - $15 D N; D - скорость 0..255; 
                  N - таймер, где N
                      0 - таймер эффектов

    16 - Режим смены эффектов: 
      - $16 0;     - ручной режим;  
      - $16 1;     - авторежим; 
      - $16 2;     - предыдущий эффект; 
      - $16 3;     - следующий эффект; 
      - $16 5 X;   - вкл/выкл случайный выбор следующего эффекта, X: 0 - выкл; 1 - вкл. 

    17 - Время автосмены эффектов и бездействия: 
      - $17 N1 N2;
            N1 - время в секундах демонстрации эффекта, затем переход к следующему эффекту
            N2 - время в секундах до автоматического перехода из ручного режима в автоматический; Значение 0 - отключает автопереход в демо-режим

    18 - Запрос текущих параметров программой WiFiPanel / WiFiPlayer: 
      - $18 page; 
         page - страница настройки в программе на смартфоне (1..12) или специальный параметр (91..99)
           1: - Настройки
           2: - Эффекты
           3: - Настройки бегущей строки
           4: - Настройки часов
           5: - Настройки будильника
           6: - Настройки подключения
           7: - Настройки режимов автовключения по времени
          10: - Загрузка картинок
          11: - Рисование
          12: - Игры
          91: - Запрос текста бегущих строк для заполнения списка в программе (как есть, без обработки макроса)
          92: - Запрос текста бегущих строк для заполнения списка в программе (добавлен индекс, выполняется обработка макросов в строке)
          93: - Запрос списка звуков будильника
          94: - Запрос списка звуков рассвета
          95: - Ответ состояния будильника - сообщение по инициативе устройства
          96: - Ответ демо-режима звука - сообщение по инициативе устройства
          99: - Запрос списка эффектов

    19 - работа с настройками часов
      - $19 1 X;    - сохранить настройку X "Часы поверх эффектов"; X=0 - нет, X=1 - да
      - $19 2 X;    - Использовать синхронизацию часов NTP;  X=0 - нет, X=1 - да
      - $19 3 N Z;  - $19 3 N ZH ZM; - Период синхронизации часов NTP (N) и Часовой пояс (ZH) -12..12  и минуты 0 / 15 / 30 / 45 (ZM)
      - $19 4 X;    - Выключать индикатор TM1637 при выключении экрана X=0 - нет, X=1 - да
      - $19 6 X;    - Ориентация часов  X: 0 - горизонтально, 1 - вертикально
      - $19 7 X;    - Размер часов X: 0 - автовыбор в зависимости от размера матрицы (3x5 или 5x7), 1 - малые 3х5, 2 - большие 5x7
      - $19 8 YYYY MM DD HH MM; - Установить текущее время YYYY.MM.DD HH:MM
      - $19 9 X;    - Показывать температуру вместе с малыми часами X=1 - да; X=0 - нет
      - $19 10 X;   - Цвет ночных часов:  0 - R; 1 - G; 2 - B; 3 - C; 3 - M; 5 - Y; 6 - W;
      - $19 11 X;   - Яркость ночных часов:  1..255;
      - $19 12 X;   - скорость прокрутки часов оверлея или 0, если часы остановлены по центру матрицы
      - $19 14 00FFAA; - цвет часов оверлея, сохраняемый в globalClockColor
      - $19 16 X;   - Показывать дату в режиме часов:  X=0 - нет, X=1 - да
      - $19 17 D I; - Продолжительность / интервал отображения даты / температуры (в секундах): D - продолжительность; I - интервал;

    20 - настройки и управление будильниками
      - $20 0;  - отключение будильника после его срабатывания (сброс состояния isAlarming)
      - $20 2 X VV MA MB;
           X    - использовать звук будильника X=0 - нет, X=1 - да 
          VV    - максимальная громкость
          MA    - номер файла звука будильника
          MB    - номер файла звука рассвета
      - $20 3 X NN VV; - воспроизведение примера звука будильника
           X - 1 играть 0 - остановить
          NN - номер файла звука будильника из папки SD:/01
          VV - уровень громкости
      - $20 4 X NN VV; - воспроизведение примера звука рассвета
           X - 1 играть 0 - остановить
          NN - номер файла звука рассвета из папки SD:/02
          VV - уровень громкости
      - $20 5 VV; - установить уровень громкости проигрывания примеров (когда звук уже воспроизводится плеером)
          VV - уровень громкости

    21 - настройки подключения к сети / точке доступа
      - $21 0 X; - использовать точку доступа: X=0 - не использовать X=1 - использовать
      - $21 1 IP1 IP2 IP3 IP4; - установить статический IP адрес подключения к локальной WiFi сети, пример: $21 1 192 168 0 106;
      - $21 2; Выполнить переподключение к сети WiFi

    22 - настройки включения режимов матрицы в указанное время (режимы по времени 1..4, 5 - Рассвет, 6 - Закат) 
       - $22 HH1 MM1 NN1 HH2 MM2 NN2 HH3 MM3 NN3 HH4 MM4 NN4 NN5 NN6;
             HHn - час срабатывания
             MMn - минуты срабатывания
             NNn - эффект: -3 - выключено; -2 - выключить матрицу; -1 - ночные часы; 0 - демо режим: сдучайный и далее по кругу; 1 и далее - список режимов EFFECT_LIST 
             Время рассвета и заката получаются с сервера погоды - время восзхода и заката Солнца на текущий день.

    23 - прочие настройки
       - $23 0 VAL;  - лимит по потребляемому току  
       - $23 1 ST;   - Сохранить EEPROM в файл    ST = 0 - внутр. файл. систему; 1 - на SD-карту  
       - $23 2 ST;   - Загрузить EEPROM из файла  ST = 0 - внутр. файл. системы; 1 - на SD-карты  
       - $23 3 E1 E2 E3;   - Установка режима работы панели и способа трактовки полученных данных синхронизации  
             E1 - режим работы 0 - STANDALONE; 1 - MASTER; 2 - SLAVE  
             E2 - данные в кадре: 0 - физическое расположение цепочки диодов  
                                  1 - логическое расположение - X,Y - 0,0 - левый верхний угол и далее вправо по X, затем вниз по Y  
                                  2 - payload пакета - строка команды  
             E3 - группа синхронизации 0..9 
       - $23 4; - Перезагрузить устройство  

## Получение текущих настроек

Для получения текущих значений параметров устройства следует отправить ему команду
```
    $6 7|<список>
``` 
где <список> - строка запрашиваемых текущих значений параметров   
Пример:
 
    "$6 7|CE|CC|CO|CK|NC|SC|C1|DC|DD|DI|NP|NT|NZ|NS|DW|OF"
 
или  

    "$6 7|CE CC CO CK NC SC C1 DC DD DI NP NT NZ NS DW OF"  

Ключи разделяются символом `'|'` или `' '`(пробел).  


Список ключей и формат возвращаемых значений приведены в таблице  

  | **Ключ** | **Ответ**         | **Значение** 
  | -------- | ----------------- | -------- 
  | **W**    |**W:число**        | ширина матрицы
  | **H**    |**H:число**        | высота матрицы
  | **AA**   |**AA:[текст]**     | пароль точки доступа - MQTT-канал
  | **AB**   |**AB:[текст]**     | пароль точки доступа - Web-канал
  | **AD**   |**AD:число**       | продолжительность рассвета, мин
  | **AE**   |**AE:число**       | эффект, использующийся для будильника
  | **AL**   |**AL:X**           | сработал будильник 0-нет, 1-да
  | **AM1T** |**AM1T:HH MM**     | час 00..23 и минуты 00..59 включения режима 1, разделенные пробелом
  | **AM1A** |**AM1A:NN**        | номер эффекта режима 1:   -3 - не используется; -2 - выключить матрицу; -1 - ночные часы; 0 - включить случайный с автосменой; 1 - номер режима из списка EFFECT_LIST
  | **AM2T** |**AM2T:HH MM**     | час 00..23 и минуты 00..59 включения режима 2, разделенные пробелом
  | **AM2A** |**AM2A:NN**        | номер эффекта режима 2:   -3 - не используется; -2 - выключить матрицу; -1 - ночные часы;  0 - включить случайный с автосменой; 1 - номер режима из списка EFFECT_LIST
  | **AM3T** |**AM3T:HH MM**     | час 00..23 и минуты 00..59 включения режима 3, разделенные пробелом
  | **AM3A** |**AM3A:NN**        | номер эффекта режима 3:   -3 - не используется; -2 - выключить матрицу; -1 - ночные часы; 0 - включить случайный с автосменой; 1 - номер режима из списка EFFECT_LIST
  | **AM4T** |**AM4T:HH MM**     | час 00..23 и минуты 00..59 включения режима 4, разделенные пробелом
  | **AM4A** |**AM4A:NN**        | номер эффекта режима 4:   -3 - не используется; -2 - выключить матрицу; -1 - ночные часы;  0 - включить случайный с автосменой; 1 - номер режима из списка EFFECT_LIST
  | **AM5A** |**AM5A:NN**        | номер эффекта режима по времени "Рассвет": -3 - не используется; -2 - выключить матрицу; -1 - ночные часы;  0 - включить случайный с автосменой; 1 - номер режима из списка EFFECT_LIST
  | **AM6A** |**AM6A:NN**        | номер эффекта режима по времени "Закат":   -3 - не используется; -2 - выключить матрицу; -1 - ночные часы;  0 - включить случайный с автосменой; 1 - номер режима из списка EFFECT_LIST
  | **AN**   |**AN:[текст]**     | имя точки доступа
  | **AT**   |**AT: DW HH MM**   | часы-минуты времени будильника для дня недели DW 1..7 -> например "AT:1 09 15"
  | **AU**   |**AU:X**           | создавать точку доступа 0-нет, 1-да
  | **AW**   |**AW:число**       | битовая маска дней недели будильника b6..b0: b0 - пн .. b7 - вс
  | **BE**   |**BE:число**       | контрастность эффекта
  | **BR**   |**BR:число**       | яркость
  | **BS**   |**BS:число**       | задержка повтора нажатия кнопки в играх 10..100 => 100..1000 vc
  | **C1**   |**C1:цвет**        | цвет режима "монохром" часов оверлея; цвет: 192,96,96 - R,G,B
  | **C2**   |**C2:цвет**        | цвет режима "монохром" бегущей строки; цвет: 192,96,96 - R,G,B
  | **CС**   |**CС:X**           | режим цвета часов оверлея: 0,1,2
  | **CE**   |**CE:X**           | оверлей часов вкл/выкл, где Х = 0 - выкл; 1 - вкл (использовать часы в эффектах)
  | **CH**   |**CH:X**           | доступны горизонтальные часы, где Х = 0 - недоступны; 1 - доступны
  | **CK**   |**CK:X**           | размер горизонтальных часов, где Х = 0 - авто; 1 - малые 3x5; 2 - большие 5x7 
  | **CL**   |**CL:цвет**        | цвет рисования в формате RRGGBB
  | **CO**   |**CO:X**           | ориентация часов: 0 - горизонтально, 1 - вертикально
  | **CT**   |**CT:X**           | режим цвета текстовой строки: 0,1,2
  | **CV**   |**CV:X**           | доступны вертикальные часы, где Х = 0 - недоступны; 1 - доступны
  | **DC**   |**DC:X**           | показывать дату вместе с часами 0-нет, 1-да
  | **DD**   |**DD:число**       | время показа даты при отображении часов (в секундах)
  | **DI**   |**DI:число**       | интервал показа даты при отображении часов (в секундах)
  | **DM**   |**DM:Х**           | демо режим, где Х = 0 - ручное управление; 1 - авторежим
  | **DW**   |**DW:X**           | показывать температуру вместе с малыми часами 0-нет, 1-да
  | **E0**   |**E0:число**       | поддержка протокола E1.31 0-нет, 1-есть
  | **E1**   |**E1:число**       | режим работы 0 - STANDALONE, 1 - MASTER, 2 - SLAVE  
  | **E2**   |**E2:число**       | тип данных работы 0 - PHYSIC, 1 - LOGIC, 2 - COMMAND
  | **E3**   |**E3:число**       | группа синхронизации 0..9
  | **EE**   |**EE:X**           | Наличие сохраненных настроек EEPROM на SD-карте или в файловой системе МК: 0 - нет 1 - есть в FS; 2 - есть на SD; 3 - есть в FS и на SD
  | **EF**   |**EF:число**       | текущий эффект - id
  | **EN**   |**EN:[текст]**     | текущий эффект - название
  | **ER**   |**ER:[текст]**     | отправка клиенту сообщения инфо/ошибки последней операции
  | **FL**   |**FL:[список]**    | список файлов картинок нарисованных пользователем с SD-карты или внутренней памяти, разделенный запятыми, ограничители [] обязательны
  | **FM**   |**FM:X**           | количество оставшейся свободной память в Heap
  | **FS**   |**FS:X**           | доступность внутренней файловой системы микроконтроллера для хранения файлов: 0 - нет, 1 - да
  | **FV**   |**FV:[список]**    | Список избранных эффектов и порядок их воспроизведения. Позиция в строке - очередность воспроизведения. Символ в позиции строки - 0..9,A..Z,a..z - закодированный ID эффекта 0..MAX_EFFECT
  | **IC**   |**IС:N\|colors**   | отправка изображения с матрицы на телефон по колонкам; текст N - номер колонки; colors - RRGGBB - цвета точек разделенные запятой: RRGGBB,RRGGBB,...,RRGGBB; 
  | **IR**   |**IR:N\|colors**   | отправка изображения с матрицы на телефон по строкам;  текст N - номер строки; colors - RRGGBB - цвета точек разделенные запятой: RRGGBB,RRGGBB,...,RRGGBB; 
  | **IP**   |**IP:xx.xx.xx.xx** | текущий IP адрес WiFi соединения в сети
  | **IT**   |**IT:число**       | время бездействия в секундах
  | **LE**   |**LE:[список]**    | список эффектов, разделенный запятыми, ограничители [] обязательны
  | **LF**   |**LF:[список]**    | список файлов эффектов считанных с SD-карты, разделенный запятыми, ограничители [] обязательны
  | **M0**   |**M0:число**       | ширина сегмента матрицы (количество диодов) 1..128
  | **M1**   |**M1:число**       | высота сегмента матрицы (количество диодов) 1..128
  | **M2**   |**M2:число**       | тип сегмента матрицы - 0 - зигзаг; 1 - параллельная
  | **M3**   |**M3:число**       | угол подключения диодов в сегменте: 0 - левый нижний, 1 - левый верхний, 2 - правый верхний, 3 - правый нижний
  | **M4**   |**M4:число**       | направление ленты из угла сегмента: 0 - вправо, 1 - вверх, 2 - влево, 3 - вниз
  | **M5**   |**M5:число**       | количество сегментов в ширину составной матрицы 1..15
  | **M6**   |**M6:число**       | количество сегментов в высоту составной матрицы 1..15
  | **M7**   |**M7:число**       | соединение сегментов составной матрицы: 0 - зигзаг, 1 - параллельная
  | **M8**   |**M8:число**       | угол 1-го сегмента мета-матрицы: 0 - левый нижний, 1 - левый верхний, 2 - правый верхний, 3 - правый нижний
  | **M9**   |**M9:число**       | направление следующих сегментов мета-матрицы из угла: 0 - вправо, 1 - вверх, 2 - влево, 3 - вниз
  | **M10**  |**M10:[список]**   | список файлов карт индексов, разделенный запятыми, ограничители [] обязательны
  | **M11**  |**M11:число**      | Индекс текущей используемой карты индексов
  | **MA**   |**MA:число**       | номер файла звука будильника из папки SD:/01
  | **MB**   |**MB:число**       | номер файла звука рассвета из папки SD:/02
  | **MD**   |**MD:число**       | сколько минут звучит будильник, если его не отключили
  | **MP**   |**MP:папка~файл**  | номер папки и файла звука который проигрывается, номер папки и звука разделены '~'
  | **MU**   |**MU:X**           | использовать звук в будильнике 0-нет, 1-да
  | **MV**   |**MV:число**       | максимальная громкость будильника
  | **MX**   |**MX:X**           | MP3 плеер доступен для использования 0-нет, 1-да
  | **NA**   |**NA:[текст]**     | пароль подключения к сети (MQTT-канал)
  | **NB**   |**NB:Х**           | яркость цвета ночных часов, где Х = 1..255
  | **NC**   |**NС:Х**           | цвет ночных часов, где Х = 0 - R; 1 - G; 2 - B; 3 - C; 4 - M; 5 - Y;
  | **NM**   |**NM:число**       | часовой пояс - минуты 0 / 15 /30 / 45
  | **NP**   |**NP:Х**           | использовать NTP, где Х = 0 - выкл; 1 - вкл
  | **NS**   |**NS:[текст]**     | сервер NTP, ограничители [] обязательны
  | **NT**   |**NT:число**       | период синхронизации NTP в минутах
  | **NW**   |**NW:[текст]**     | SSID сети подключения
  | **NX**   |**NX:[текст]**     | пароль подключения к сети (Web-канал)
  | **NZ**   |**NZ:число**       | часовой пояс -12..+12
  | **OF**   |**OF:X**           | выключать часы вместе с лампой 0-нет, 1-да
  | **PD**   |**PD:число**       | продолжительность режима в секундах
  | **PS**   |**PS:X**           | состояние программного вкл/выкл панели 0-выкл, 1-вкл
  | **PW**   |**PW:число**       | ограничение по току в миллиамперах
  | **QA**   |**QA:X**           | использовать MQTT 0-нет, 1-да
  | **QP**   |**QP:число**       | порт подключения к MQTT серверу
  | **QR**   |**QR:X**           | топик использует префикс в виде имени пользователя для формирования топика
  | **QS**   |**QS:[text]**      | имя MQTT сервера, например QS:[srv2.clusterfly.ru]
  | **QU**   |**QU:[text]**      | имя пользователя MQTT соединения, например QU:[user_af7cd12a]
  | **QW**   |**QW:[text]**      | пароль MQTT соединения, например QW:[pass_eb250bf5]
  | **QZ**   |**QZ:X**           | сборка поддерживает MQTT 0-нет, 1-да
  | **RM**   |**RM:Х**           | смена режимов в случайном порядке, где Х = 0 - выкл; 1 - вкл
  | **S1**   |**S1:[список]**    | список звуков будильника, разделенный запятыми, ограничители [] обязательны
  | **S2**   |**S2:[список]**    | список звуков рассвета, разделенный запятыми, ограничители [] обязательны
  | **S3**   |**S3:[список]**    | список звуков для макроса {A} бегущей строки, ограничители [] обязательны
  | **SC**   |**SC:число**       | скорость смещения часов оверлея
  | **SD**   |**SD:X**           | наличие на SD карте файлов эффектов Jinx: Х = 0 - нет эффектов; 1 - есть эффекты
  | **SE**   |**SE:число**       | скорость эффектов
  | **SM**   |**SM:число**       | текущий "специальный режим" или -1 если работа в обычном режиме
  | **SS**   |**SS:число**       | параметр #1 эффекта
  | **SQ**   |**SQ:спец**        | параметр #2 эффекта; спец - "L>val>itrm1,item2,..itemN" - список, где val - текущее, далее список; "C>x>title" - чекбокс, где x=0 - выкл, x=1 - вкл; title - текст чекбокса
  | **ST**   |**ST:число**       | скорость смещения бегущей строки
  | **SX**   |**SX:X**           | наличие и доступность SD карты в системе: Х = 0 - нат SD карты; 1 - SD карта доступна
  | **T1**   |**T1:[ЧЧ:ММ]**     | время рассвета, полученное с погодного сервера
  | **T2**   |**T2:[ЧЧ:ММ]**     | время заката, полученное с погодного сервера
  | **TE**   |**TE:X**           | оверлей текста бегущей строки вкл/выкл, где Х = 0 - выкл; 1 - вкл (использовать бегущую строку в эффектах)
  | **TI**   |**TI:число**       | интервал отображения текста бегущей строки
  | **TM**   |**TM:X**           | в системе присутствует индикатор TM1637, где Х = 0 - нет; 1 - есть
  | **TS**   |**TS:строка**      | строка состояния кнопок выбора текста из массива строк: 36 символов 0..5, где<br>- 0 - серый - пустая<br>- 1 - черный - отключена<br>- 2 - зеленый - активна - просто текст, без макросов<br>- 3 - голубой - активна, содержит макросы кроме даты <br>- 4 - синий - активная, содержит макрос даты<br>- 5 - красный - для строки 0 - это управляющая строка  
  | **TY**   |**TY:[I:Z > текст]**| текст для строки, с указанным индексом I 0..35, Z 0..9,A..Z. Ограничители [] обязательны; текст ответа в формате: 'I:Z > текст'; 
  | **UC**   |**UC:X**           | использовать часы поверх эффекта 0-нет, 1-да
  | **UP**   |**UP:число**       | время работы системы с последнего перезапуска в секундах
  | **UT**   |**UT:X**           | использовать бегущую строку поверх эффекта 0-нет, 1-да
  | **W1**   |**W1**             | текущая погода ('ясно','пасмурно','дождь'и т.д.)
  | **W2**   |**W2**             | текущая температура
  | **WC**   |**WC:X**           | Использовать цвет для отображения температуры в дневных часах  X: 0 - выключено; 1 - включено
  | **WN**   |**WN:X**           | Использовать цвет для отображения температуры в ночных часах  X: 0 - выключено; 1 - включено
  | **WR**   |**WR:число**       | Регион погоды Yandex
  | **WS**   |**WS:число**       | Регион погоды OpenWeatherMap
  | **WT**   |**WT:число**       | Период запроса сведений о погоде в минутах
  | **WU**   |**WU:X**           | Использовать получение погоды с сервера: 0 - выключено; 1 - Yandex; 2 - OpenWeatherMap
  | **WZ**   |**WZ:X**           | Прошивка поддерживает погоду USE_WEATHER == 1 - 0 - выключено; 1 - включено
  
<br><br><br><br><br>