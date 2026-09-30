### Правило для обнаружения :
```
alert tcp any any -> $HOME_NET any (msg:"SYN_SCAN_DETECTED"; flags:S; detection_filter: track by_src, count 10, seconds 5; sid:1000002; rev:1;)
```
### Описание:
```
- Анализирует только TCP трафик из любого источника на любой внутренний порт.
- Фильтр flags:S ищет любые SYN пакеты.
- detection_filter отсекает единичные запросы, алерт активируется если один IP отправляет 10 SYN-пакетов за 5 секунд
```
 