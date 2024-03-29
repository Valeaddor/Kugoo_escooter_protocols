# G-Booster протокол 1 версии (старые модели)
### Описание протокола приема-передачи данных между контроллером и дисплеем (курком)


От дисплея к контроллеру МК идут UART пакеты по 15 байт с параметрами скорости 1200 8n1 (1200 бод! это не ошибка), и промежутками (timeout) порядка 90-100ms.

| Байт | Значение | Описание |
|:----:|:-------------:|:-----:|
| 01 | 0x01 | Первый байт сигнатуры начала пакета |
| 02 | 0x03 | Второй байт сигнатуры начала пакета |
| 03 | 0x00-0xFF | Порядковый номер пакета, от 0x00 до 0xFF потом опять начинается с 0x00 |
| 04 | 0x00 | Обычно 0x00, значение не выяснено |
| 05 | 0x00 | Обычно 0x00, значение не выяснено |
| 06 | Magic+Speed | "Магический" байт значение которого зависит от порядкого номера пакета и вычисляется из массива gb1_disp + значение скорости (1 = 0x00, 2 = 0x05, 3 = 0x0a) [^ремарка1] |
| 07 | 0x00 | Круиз-контроль = 0x00 - выкл, 0x04 - включен  (еще бывает 0x02, значение надо бы выяснить) [^ремарка5] |
| 08 | 0x64 | Мощность/токоотдача в % (0x64 = 100 это 100%) |
| 09 | 0x00 | Обычно 0x00, значение не выяснено |
| 10 | 0x80 | Обычно 0x80, значение не выяснено |
| 11 | 0x00 | Значение параметра P9 (электротормоз) , цифра 0x00 = выкл, 0x01 = стандарт, 0x02 = максимальный |
| 12 | 0x00 | Резкий старт = 0x00 - выкл, 0x64 - включен |
| 13 | 0x0F | Количество пар магнитов МК (0x0F = 15) [^mpairs] |
| 14 | 0x0A | Диаметр Мотор Колеса в дюймах (0x0A = 10) [^diam] |
| 15 | CRC | Контрольная сумма. Считается как XOR над всеми байтами в пакете, включая заголовок. Пример: CRC = Byte01 ^ Byte02 ^...^ Byte14 |


От контроллера МК к дисплею идут UART пакеты по 15 байт с параметрами скорости 1200 8n1, и промежутками (timeout) порядка 90-100ms.

| Байт | Значение | Описание |
|:----:|:-------------:|:-----:|
| 01 | 0x36 | Байт сигнатуры начала пакета |
| 02 | 0x00-0xFF | Порядковый номер пакета, от 0x00 до 0xFF потом опять начинается с 0x00 |
| 03 | 0x00 | Обычно 0x00, значение не выяснено |
| 04 | Magic | "Магический" байт значение которого зависит от порядкого номера пакета и вычисляется из массива gb1_contr [^ремарка3] |
| 05 | Magic | "Магический" байт |
| 06 | Magic | "Магический" байт |
| 07 | 0x00 | Обычно 0x00, значение не выяснено |
| 08 | Magic+ | "Магический" байт + highByte значения "скорости" (кол-во оборотов в секунду?) [^ремарка4] |
| 09 | Magic+ | "Магический" байт + lowByte значения "скорости" (кол-во оборотов в секунду?) [^ремарка4] |
| 10 | Magic+ | "Магический" байт + 0x00 обычно, возможно highByte значения тока? |
| 11 | Magic+ | "Магический" байт + (lowByte?) значения тока |
| 12 | Magic+0x75 | "Магический" байт + 0x75 обычно, значения не выяснено |
| 13 | Magic+0x31 | "Магический" байт + 0x31 обычно, значения не выяснено |
| 14 | Magic | "Магический" байт |
| 15 | CRC | Контрольная сумма. Считается как XOR над всеми байтами в пакете, включая заголовок. Пример: CRC = Byte01 ^ Byte02 ^...^ Byte14 |



[^ремарка1]: "Магический" байт "дисплея" меняется для пакетов с номерами от 0x00 до 0x7F, затем идет повторение, то есть  0x80 = 0x00 ... 0xFF = 0x7F. Так же сюда добавляется значение скорости если она больше 1, если скорость 2 то + 0x05, если скорость 3 то + 0x0a. Возможно есть какая-либо формула вычисления "магического" байта, но я пока не нашел.. 
[^diam]: Для курка JH-01 можно поменять от 0 до 99  
[^mpairs]: 0x0F = 15 * 2 = 30 магнитов MK, можно менять от .. до .. 
[^ремарка3]: "Магический" байт "контроллера" (не равен дисплею) меняется для пакетов с номерами от 0x00 до 0x7F, затем идет повторение, то есть  0x80 = 0x00 ... 0xFF = 0x7F.
[^ремарка4]: Это значение оборотов колеса в секунду и надо применить формулу длины окружности с диаметром колеса, пока имперически высчитал что полученное значение деленное на 15 дает цифру скорости показываемую дисплеем при диаметре колеса 10 дюймов.
[^ремарка5]: возможно 0x02 это включение круиза за более коротое время ..  
