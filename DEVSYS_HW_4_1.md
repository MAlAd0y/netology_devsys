Задание 1

a=1
b=2
c=a+b
d=$a+$b
e=$(($a+$b))

c=ab, потому-что в объявлении переменной $c a и b заданы не как переменные, а как символы
d=3, потому-что тут $a и $b указаны как переменные
e=Error: no such file "3", потому-что баш попытается найти файл с именем равным сумме $a и $b

Задание 2

while ((1==1)
do
	curl https://localhost:4757
	if (($? != 0))
	then
		date >> curl.log
	else
        exit()
    fi
done

Задание 3

hosts=(192.168.0.1 173.194.222.113 87.250.250.24)
timeout=5
for i in {1..5}
do
date >>hosts.log
    for h in ${hosts[@]}
    do
	curl -Is --connect-timeout $timeout $h:80 >/dev/null
        echo "    check" $h status=$? >>check_hosts.log
    done
done

Задание 4

hosts=(192.168.0.1 173.194.222.113 87.250.250.24)
timeout=5
res=0

while (($res == 0))
do
    for h in ${hosts[@]}
    do
	curl -Is --connect-timeout $timeout $h:80 >/dev/null
	res=$?
	if (($res != 0))
	then
	    echo "    ERROR on " $h status=$res >>check_hosts_err.log
	fi
    done
done