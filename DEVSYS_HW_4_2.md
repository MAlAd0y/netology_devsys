Задание 1


Есть скрипт:

#!/usr/bin/env python3
a = 1
b = '2'
c = a + b

Ответ:

Какое значение будет присвоено переменной c?
будет ошибка, т.к. типы не соответсвуют для операции , int и str
Как получить для переменной c значение 12?
привести a к строке:       c=str(a)+b
Как получить для переменной c значение 3?
привести b к целому числу: c=a+int(b)

Задание 2


Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break

Ответ:

#!/usr/bin/env python3

import os

bash_command = ["cd ~/devops3-netology", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)

Задание 3


Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

Ответ:

#!/usr/bin/env python3

import os
import sys

cmd = sys.argv[1]
bash_command = ["cd "+cmd, "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified: ', '')
        print(cmd+prepare_result)

Вывод:

19:30:25 MALAd0y@mubnt(0):./mod.py ~/netology_devsys/
/home/MALAd0y/netology_devsys/README.md
/home/MALAd0y/netology_devsys/DEVSYS_HW_4_1.md
/home/MALAd0y/netology_devsys/DEVSYS_HW_4_2.md

Задание 4

Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: drive.google.com, mail.google.com, google.com.

Ответ:

##!/usr/bin/env python3

import socket as s
import time as t
import datetime as dt

# set variables 
i = 1
wait = 2 # интервал проверок в секундах
srv = {'drive.google.com':'0.0.0.0', 'mail.google.com':'0.0.0.0', 'google.com':'0.0.0.0'}
init=0

print('*** start script ***')
print(srv)
print('********************')

while 1==1 : #отладочное число проверок 
  for host in srv:
    ip = s.gethostbyname(host)
    if ip != srv[host]:
      if i==1 and init !=1:
        print(str(dt.datetime.now().strftime("%Y-%m-%d %H:%M:%S")) +' [ERROR] ' + str(host) +' IP mistmatch: '+srv[host]+' '+ip)
      srv[host]=ip
# счетчик итераций, для бесконечного цикла нужно закомментировать
  i+=1 
  if i >= 50 : 
    break
  t.sleep(wait)


Вывод:

  20:39:03 MALAd0y@mubnt(0):~/python$ ./hw3.py
*** start script ***
{'drive.google.com': '0.0.0.0', 'mail.google.com': '0.0.0.0', 'google.com': '0.0.0.0'}
********************
2022-04-29 20:39:20 [ERROR] drive.google.com IP mistmatch: 0.0.0.0 173.194.73.194
2022-04-29 20:39:20 [ERROR] mail.google.com IP mistmatch: 0.0.0.0 64.233.165.83
2022-04-29 20:39:20 [ERROR] google.com IP mistmatch: 0.0.0.0 173.194.221.139
2022-04-29 20:40:46 [ERROR] mail.google.com IP mistmatch: 64.233.165.83 64.233.165.19
2022-04-29 20:40:48 [ERROR] mail.google.com IP mistmatch: 64.233.165.19 64.233.165.83
2022-04-29 20:40:52 [ERROR] google.com IP mistmatch: 173.194.221.139 173.194.222.101
2022-04-29 20:40:54 [ERROR] google.com IP mistmatch: 173.194.222.101 173.194.222.100
2022-04-29 20:41:18 [ERROR] google.com IP mistmatch: 173.194.222.100 64.233.164.139
2022-04-29 20:41:20 [ERROR] google.com IP mistmatch: 64.233.164.139 64.233.164.138
2022-04-29 20:41:36 [ERROR] drive.google.com IP mistmatch: 173.194.73.194 108.177.14.194
2022-04-29 20:42:04 [ERROR] drive.google.com IP mistmatch: 108.177.14.194 173.194.73.194