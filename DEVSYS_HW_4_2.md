Задание 1

Какое значение будет присвоено переменной c?
будет ошибка, т.к. типы не соответсвуют для операции , int и str
Как получить для переменной c значение 12?
привести a к строке:       c=str(a)+b
Как получить для переменной c значение 3?
привести b к целому числу: c=a+int(b)

Задание 2

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


19:30:25 MALAd0y@mubnt(0):./mod.py ~/netology_devsys/
/home/MALAd0y/netology_devsys/README.md
/home/MALAd0y/netology_devsys/DEVSYS_HW_4_1.md
/home/MALAd0y/netology_devsys/DEVSYS_HW_4_2.md

Задание 4

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