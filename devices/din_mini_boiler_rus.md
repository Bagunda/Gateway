# Модуль управления котлом отопления

Содержит в себе:
* Аппаратный мастер шины 1-Wire
* Реле 1A для управления по методу - запрос тепла
* Цифровая шина OpenTherm (Master) для подключения котла
* Цифровая шина OpenTherm (Slave) для подключения комнатного термостата

## Клеммы
1) Шина 1-Wire
2) Питание +5V для шины 1-Wire
3) Цифровая шина OpenTherm Master / Реле (подключение котла, полярность не важна)
4) --/
5) Цифровая шина OpenTherm Slave (подключение комнатного термостата, полярность не важна)
6) --/

## Шина 1-Wire
Устройство содержит в себе аппаратный контроллер шины с активной подтяжкой. 

В большинстве случаев не требуется подключения дополнительных резисторов подтяжки.
Поддерживается паразитное питание и подключение нескольких датчиков шлейфом.

Шлюз поддерживает в данный момент до 10 датчиков Dallas DS18B20.

Управлять устройствами 1-Wire можно на странице /1wire.

## Защита цифровой шины от высокого напряжения
Если необходимо использовать релейное управление и на клеммах котла напряжение выше 24V, 
необходимо снять перемычки на плате для защиты цифровой шины в устройстве, таким образом цифровая шина будет отключена от клемм 3 и 4.

## Релейное управление по методу - запрос тепла
Практически все котлы умеют управляться по методу - запрос тепла, Котел начинает греть ТН до температуры установленной на его встроенном термостате, 
когда замыкаются его клеммы термостата.

Реле в плате шилда подключено по NC схеме и при обесточивании системы будет подан сигнал к нагреву.

## Подключение комнатного термоста OpenTherm
К клеммам 5 и 6 возможно опциональное подключение комнатного термостата OpenTherm, таким образом устройство встает в разрыв шины OpenTherm.

Шлюз может работать в нескольких режимах при подключении термостата:
1) В режиме прозрачного шлюза, когда котлом управляет комнатный термостат, а шлюз прозрачно транслирует его сообщения котлу и обратно, фиксируя текущее состояние системы.
3) В режиме термостата, тогда контроллер самостоятельно или по командом из вне управляет котлом отопления

## Выбор программного режима работы порта
Выбор программного режима работы порта осуществляется через функции *gpio.mode(GPIO, mode)*, где mode может быть *gpio.INPUT* или *gpio.OUTPUT*.

Определение режима удобно прописать в файл *init.lua*, т.к. это требуется только однократно.

### Примеры использования

Задать каналу реле режим выхода и выключить его:
```
gpio.mode(25, gpio.OUTPUT)
gpio.write(25, gpio.LOW)
```

Запуск шлюза OpenTherm:
```
thermo.beginOpenTherm()
```



