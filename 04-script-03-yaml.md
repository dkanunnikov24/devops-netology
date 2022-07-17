### Как сдавать задания

Вы уже изучили блок «Системы управления версиями», и начиная с этого занятия все ваши работы будут приниматься ссылками на .md-файлы, размещённые в вашем публичном репозитории.

Скопируйте в свой .md-файл содержимое этого файла; исходники можно посмотреть [здесь](https://raw.githubusercontent.com/netology-code/sysadm-homeworks/devsys10/04-script-03-yaml/README.md). Заполните недостающие части документа решением задач (заменяйте `???`, ОСТАЛЬНОЕ В ШАБЛОНЕ НЕ ТРОГАЙТЕ чтобы не сломать форматирование текста, подсветку синтаксиса и прочее, иначе можно отправиться на доработку) и отправляйте на проверку. Вместо логов можно вставить скриншоты по желани.

# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"


## Обязательная задача 1
Мы выгрузили JSON, который получили через API запрос к нашему сервису:
```
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис
  
  ОТВЕТ:
  { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : "7175"
            }
            { "name" : "second",
            "type" : "proxy",
            "ip" : "71.78.22.43"
            }
        ]
    }
    
    Добавлены кавычки в значение ip "7175" и добавлены отсутствующие кавычки в ключе "ip" и в значении "71.78.22.43"

## Обязательная задача 2
В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. Формат записи YAML по одному сервису: `- имя сервиса: его IP`. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

### Ваш скрипт:
```python
#!/usr/bin/env python3
import socket
import time
import json
import yaml

dns_svc=['drive.google.com','mail.google.com','google.com']
ip_svc_array_old=["unknown IP","unknown IP","unknown IP"]
ip_svc_array_new=["","",""]
json_array=["","",""]


while 1==1:
    x=0
    for i in dns_svc:
        ip_svc_array_new[x] = socket.gethostbyname(i)
        print(dns_svc[x] + ' - ' + ip_svc_array_new[x])
        if ip_svc_array_new[x] != ip_svc_array_old[x]:
            print("[ERROR] "+ dns_svc[x] +" IP mismatch: " + ip_svc_array_old[x] + " " + ip_svc_array_new[x])
            json_zip = dict(zip(dns_svc, ip_svc_array_new))
            with open("socket.json", "w") as js1:
                json.dump(json_zip, js1, indent=4)
            with open("socket.yaml", "w") as ya1:
                ya1.write(yaml.dump(json_zip, explicit_start=True, explicit_end=True))
        ip_svc_array_old[x] = ip_svc_array_new[x]
        x +=1
        time.sleep(1)
```

### Вывод скрипта при запуске при тестировании:
```
vagrant@vagrant:~$ python3 script_socket_yaml2.py
drive.google.com - 74.125.131.194
[ERROR] drive.google.com IP mismatch: unknown IP 74.125.131.194
mail.google.com - 173.194.73.83
[ERROR] mail.google.com IP mismatch: unknown IP 173.194.73.83
google.com - 142.251.1.113
[ERROR] google.com IP mismatch: unknown IP 142.251.1.113
drive.google.com - 74.125.131.194
mail.google.com - 173.194.73.17
[ERROR] mail.google.com IP mismatch: 173.194.73.83 173.194.73.17
google.com - 142.251.1.113
drive.google.com - 74.125.131.194
mail.google.com - 173.194.73.17
google.com - 142.251.1.113
drive.google.com - 74.125.131.194
mail.google.com - 173.194.73.17
google.com - 142.251.1.113
drive.google.com - 74.125.131.194
mail.google.com - 173.194.73.17
google.com - 142.251.1.113
drive.google.com - 74.125.131.194
mail.google.com - 173.194.73.17
google.com - 142.251.1.113
drive.google.com - 74.125.131.194
mail.google.com - 173.194.73.17
google.com - 142.251.1.113
drive.google.com - 74.125.131.194
```

### json-файл(ы), который(е) записал ваш скрипт:
```json
{
    "drive.google.com": "74.125.131.194",
    "mail.google.com": "173.194.73.17",
    "google.com": "142.251.1.113"
}
```

### yml-файл(ы), который(е) записал ваш скрипт:
```yaml
drive.google.com: 74.125.131.194
google.com: 142.251.1.113
mail.google.com: 173.194.73.17
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Так как команды в нашей компании никак не могут прийти к единому мнению о том, какой формат разметки данных использовать: JSON или YAML, нам нужно реализовать парсер из одного формата в другой. Он должен уметь:
   * Принимать на вход имя файла
   * Проверять формат исходного файла. Если файл не json или yml - скрипт должен остановить свою работу
   * Распознавать какой формат данных в файле. Считается, что файлы *.json и *.yml могут быть перепутаны
   * Перекодировать данные из исходного формата во второй доступный (из JSON в YAML, из YAML в JSON)
   * При обнаружении ошибки в исходном файле - указать в стандартном выводе строку с ошибкой синтаксиса и её номер
   * Полученный файл должен иметь имя исходного файла, разница в наименовании обеспечивается разницей расширения файлов

### Ваш скрипт:
```python
???
```

### Пример работы скрипта:
???
