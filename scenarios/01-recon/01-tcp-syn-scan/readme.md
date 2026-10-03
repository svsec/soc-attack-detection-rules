# Обнаружение TCP SYN scan используя Suricata. 
# Attack Nmap TCP SYN scan 

(`nmap -sS`)
# Механизм детекта: TCP SYN Scan (Nmap -sS)
Реализация кастомного правила Suricata для обнаружения полуоткрытого (стелс) сканирования портов.
# Сигнатура Suricata
```suricata
alert tcp any any -> $HOME_NET any (msg:"SYN_SCAN_DETECTED"; flags:S; detection_filter: track by_src, count 10, seconds 5; sid:1000002; rev:1;)
```
*Логика:* Ищет пакеты с установленным флагом `SYN`. Порог срабатывания — 10 пакетов за 5 секунд с одного IP-адреса источника (`track by_src`).
# Воспроизведение атаки
Команда запуска сканирования с машины атакующего:
```bash
sudo nmap -sS 192.168.10.2
```
# Результат детекта 
После запуска Nmap в логах Suricata успешно фиксируется следующее событие:

```json
{
  "timestamp": "2026-10-01T13:35:29.194461+0000",
  "event_type": "alert",
  "src_ip": "192.168.10.10",
  "dest_ip": "192.168.10.2",
  "dest_port": 705,
  "proto": "TCP",
  "alert": {
    "action": "allowed",
    "signature_id": 1000002,
    "signature": "SYN_SCAN_DETECTED",
    "severity": 3
  }
}
```
# Analysis

Обнаружено массовое SYN-сканирование с `192.168.10.10` на множество портов без завершения handshake (pkts_toclient: 0)
Правило сработало по detection_filter — 10+ SYN пакетов за 5 секунд с одного источника
Правило детектирует любое массовое SYN-сканирование, не только Nmap
