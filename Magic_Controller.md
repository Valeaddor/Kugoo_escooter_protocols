### Небольшой анализ "магического" числа контроллера, первое число = номер пакета, второе число = XOR первого чиcла для получения значения "магического".

#### Начиная с пакета 0x40 XOR число начинает повторяться с той же последовательностью.

Пока не сильно помогает, но может уменьшить количество памяти занимаемой таблицей соответсвия в программе.

| № пакета | XOR Значение |
|:----:|:----:|
|    0x00	| 0x80 |
|    0x01	| 0x04 |
|    0x02	| 0x04 |
|    0x03	| 0x28 |
|    0x04	| 0x30 |
|    0x05	| 0x2с |
|    0x06	| 0x2с |
|    0x07	| 0x28 |
|    0x08	| 0x20 |
|    0x09	| 0x24 |
|    0x0a	| 0x24 | 
|    0x0b	| 0x58 |
|    0x0c	| 0x70 |
|    0x0d	| 0x5с |
|    0x0e	| 0x5с |
|    0x0f	| 0x58 |
|    0x10	| 0x40 |
|    0x11	| 0x44 |
|    0x12	| 0x44 |
|    0x13	| 0x68 |
|    0x14	| 0x90 |
|    0x15	| 0x6с |
|    0x16	| 0x6с |
|    0x17	| 0x68 |
|    0x18	| 0x60 |
|    0x19	| 0x64 |
|    0x1a	| 0x64 |
|    0x1b	| 0x78 |
|    0x1c	| 0x10 |
|    0x1d	| 0x7с |
|    0x1e	| 0x7с |
|    0x1f	| 0x78 |
|    0x20	| 0x40 |
|    0x21	| 0x44 |
|    0x22	| 0x44 |
|    0x23	| 0x28 |
|    0x24	| 0x30 | 
|    0x25	| 0x2с |
|    0x26	| 0x2с |
|    0x27	| 0x28 |
|    0x28	| 0x20 |
|    0x29	| 0x24 |
|    0x2a	| 0x24 |
|    0x2b	| 0x18 |
|    0x2c	| 0x70 |
|    0x2d	| 0x1с |
|    0x2e	| 0x1с |
|    0x2f	| 0x18 |
|    0x30	| 0x00 |
|    0x31	| 0x04 |
|    0x32	| 0x04 |
|    0x33	| 0x68 |
|    0x34	| 0x50 |
|    0x35	| 0x6с |
|    0x36	| 0x6с |
|    0x37	| 0x68 |
|    0x38	| 0x60 |
|    0x39	| 0x64 |
|    0x3a	| 0x64 |
|    0x3b	| 0xf8 |
|    0x3c	| 0x50 |
|    0x3d	| 0xfc |
|    0x3e	| 0xfc |
|    0x3f	| 0x78 |

##### Если после операции XOR "магическое" число больше 0x80 то требуется дополнительный XOR с 0x80 (замечено на пакетах 0x40, 0x54, 0x3b, 0x3d, 0x3e)
