### 1) Редактируем конфиг:
```
sudo nano /var/lib/suricata/rules/local.rules 
```
### 2) Прописываем правило: 

```
alert icmp any any -> $HOME_NET any (msg:"TEST ICMP"; sid:1000001; rev:1;)
```
### 3) Проверяем синтаксис:
```
sudo suricata -T -c /etc/suricata/suricata.yaml
```
### 4) Проверяем работоспособность:
```
sudo tail -f /var/log/suricata/eve.json | jq 'select(.event_type=="alert")' 
```
, где смотрим логи в реальном времени, фильтрует JSON из `eve.json`, оставляет только строки где `event_type` равен `alert`
