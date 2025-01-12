# Пошаговая инструкция подготовки среды для проекта

## Шаг 1

Перейдите на сайт разработчиков [Arduino](https://www.arduino.cc/en/Main/Software) и скачайте среду разработки.

Текущая версия среды разработки - Arduino IDE 2.0.4 еще очень сырая, содержит множество ошибок, иногда портит код 
исходных файлов и не всегда правильно компилирует проект. **Категорически не рекомендуется к использованию!!!**  

![Среда разработки Arduino IDE](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_01.png)

Прокрутите страницу вниз, до появления плашки Legacy IDE (1.8.X) и выберите для загрузки версию **1.8.19**  
Нажмите на ссылку **Windows Win 7 and newer**. Вы же работаете под Windows?  
Если нет - загружайте версию IDE для вашей ОС.

![Среда разработки Arduino IDE](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_01_0.png)

Можете заплатить донат разработчикам Arduino IDE. Но если не хотите - просто нажмите кнопку **JUST DOWNLOAD**
для загрузки установочного файла. После завершения скачивания запустите его.

![Среда разработки Arduino IDE](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_01_1.png)

Установите среду разработки на ваш компьютер. После завершения установки, запустите Arduino IDE.

## Шаг 2

В Arduino IDE в меню "Файл" выберите пункт "Настройки".

![Настройки среды разработки ArduinoIDE](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_02_1.png)

В открывшемся окне нажмите на кнопку справа от поля "Дополнительные ссылки для менеджера плат"

![Дополнительные ссылки для менеджера плат](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_02_2.png)

В открывшемся окне добавьте в поле ввода следующие строки:
`https://raw.githubusercontent.com/esp8266/esp8266.github.io/master/stable/package_esp8266com_index.json` - для поддержки микроконтроллеров ESP8266 (NodeMCU, Wemos d1 mini)
`https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json` - для поддержки микроконтроллера ESP32.

![Ссылка на плату ESP8266](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_02_3.png)

Примените изменения

## Шаг 3

В Arduino IDE в меню "Инструменты" выберите пункт "Менеджер плат".

![Менеджер плат](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_03_1.png)

В строке фильтра введите "ESP8266", найдите блок esp8266 by ESP8266 Commumity  
1. Выберите версию ядра 2.7.4
2. Нажмите кнопку "Установка", дождитесь завершения установки.
3. Должна появиться надпись, подтверждающая что вы установили правильную (2.7.4!!!) версию ядра системы

![Менеджер плат - установка](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_03_2.png)

В строке фильтра введите "ESP32", найдите блок ESP32 by Espressif System
1. Выберите версию ядра 2.0.6
2. Нажмите кнопку "Установка", дождитесь завершения установки.
3. Должна появиться надпись, подтверждающая что вы установили правильную (2.0.6!!!) версию ядра системы

![Менеджер плат - установка](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_03_3.png)

Закройте Arduino IDE

## Шаг 4

Перейдите в [репозиторий](https://github.com/vvip-68/LedPanelWiFi) проекта
Нажмите зеленую кнопку "Code" выберите "Download ZIP"

![Загрузка проекта из репозитория](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_04_1.png)

Сохраните архив на диск, распакуйте его в отдельную папку.

Для тех, кто пользуется утилитами работы с хранилищем GitHub (например, [SmartGit](https://www.syntevo.com/smartgit/download/)) - зарегистрируйте в нем ссылку на репозиторий
https://github.com/vvip-68/LedPanelWiFi

## Шаг 5
Скопируйте содержимое папки "libraries" из папки проекта, в папку "libraries" установленной среды разработки Arduino
C:\Program Files (x86)\Arduino\libraries

![Копирование библиотек](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_05_1.png)

**Внимание!!!**  
Выполнение этого шага **ОЧЕНЬ** важно, поскольку часть библиотек исправлена и дополнена для соответствия требованиям проекта.
Библиотеки установленные из менеджера библиотек не будут иметь этих изменений или просто будут несовместимой версии,
что приведет к тому, что проект не будет компилироваться или соберется, но может работать неправильно.  

## Шаг 6

Запустите Arduino IDE
В меню "Инструменты" выберите пункт "Управлять библиотеками"

![Менеджер библиотек](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_06_1.png)

A строке поиска наберите "FastLED"
Выберите блок "FastLED by Daniel Garcia"
1. Выберите версию библиотеки 3.5.0
2. Нажмите кнопку "Установка", дождитесь завершения установки.
3. Должна появиться надпись, подтверждающая что вы установили правильную (3.5.0!!!) версию библиотеки


![Менеджер библиотек](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_06_2.png)

Дождитесь завершения установки. Закройте Arduino IDE

## Шаг 7

В проводнике откройте папку с проектом, перейдите в папку "firmware" и далее в папку "LedPanelWiFi_v1.14"

![Проводник - папка проекта](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_07_1.png)

Дважды щелкните на файле "LedPanelWiFi_v1.14.ino"
Откроется Arduino IDE с загруженным проектом. Файлы проекта располагаются в разных вкладках. Их несколько.

![Редактор кода](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_07_2.png)

В меню "Инструменты" в пункте "Плата" в выпадающем списке выберите плату, соответствующую вашему микроконтроллеру.
В данном проекте используется плата микроконтроллера NodeMCU v1.0 или Wemos D1 pro mini.
В обоих случаях рекомендуется установить настройки как показано на рисунке.

![Параметры сборки](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_07_3.png)

Подключите плату микроконтроллера кабелем micro-USB к компьютеру.
Установите драйверы, соответствующие вашей плате (CH340G или CP2101) если они не установились автоматически при подключении контроллера.

Откройте менеджер устройств, найдите в группе "Диспетчер устройств" ветку дерева "Порты COM и LPT"

![Управление компьютером](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_07_4.png)

<a id="com-port"></a>
Найдите COM-порт, соответствующей вашей подключенной плате.

![Менеджер устройств](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_07_5.png)

Укажите данный порт в настройках - в меню "Инструменты", пункт "Порт"

## Шаг 8

Измените в скетче [параметры](https://github.com/vvip-68/LedPanelWiFi/wiki/Настройка-скетча-для-вашего-устройства),
соответствующие вашему проекту - высоту, ширину матрицы, угол подключения, направление ленты и другие, которые требуется изменить для вашего проекта.
Проверьте, что проект компилируется без ошибок. Для этого нажмите на кнопку "Проверить" в панели инструментов Arduino IDE.

![Проверка скетча](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_08_1.png)

Дождитесь окончания сборки проекта компилятором. Об успешном окончании сборки свидетельствуют белые буквы на черном фоне внизу окна редактора. 

Если белые буквы появились, обращать внимание на расположенные выше них оранжевые не нужно. Это диагностические сообщения библиотек. 

Если белых букв не появилось, вместо этого напечаталось сообщение об ошибке - читайте его внимательно, включая весь текст выше и устраняйте причину ошибки.

## Шаг 9

Если сборка проекта завершилась без ошибок - можно скетч загружать в микроконтроллер.
Подключите контроллер к USB кабелем micro-USB, выберите порт к которому подключена плата микроконтроллера, откройте монитор порта, нажав на кнопку в правом верхнем углу окна Arduino IDE.

При необходимости перед присоединением кабеля USB к контроллеру, подключите дополнительное питание к компонентам вашего собранного проекта. 

![Загрузка скетча](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_09_1.png)

Нажмите на кнопку "Загрузка" для загрузки скетча в микроконтроллер. Дождитесь завершения операции загрузки. 

![Загрузка скетча](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_09_2.png)

В черном поле редактора будут отображаться служебные сообщения, а так же прогресс загрузки в процентах.
После завершения операции появится надпись "Leaving... Hard resetting via RTS pin..."

Микроконтроллер автоматически перезагрузится и начнет выполнение скетча.
В мониторе порта отобразится журнал работы приложения, подтверждающий успешное завершение операции и выполнение скетча

![Загрузка скетча](https://github.com/vvip-68/LedPanelWiFi/blob/main/wiki/Arduino/Step_09_3.png)

Для завершения процесса сборки устройства подключитесь к созданной устройством точке доступа PanelAP и откройте в браузере страничку настроек по адресу 192.168.4.1 и [выполните настройку](https://github.com/vvip-68/LedPanelWiFi/wiki/Настройка-параметров-в-приложении)



