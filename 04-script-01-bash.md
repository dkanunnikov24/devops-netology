### Как сдавать задания

Вы уже изучили блок «Системы управления версиями», и начиная с этого занятия все ваши работы будут приниматься ссылками на .md-файлы, размещённые в вашем публичном репозитории.

Скопируйте в свой .md-файл содержимое этого файла; исходники можно посмотреть [здесь](https://raw.githubusercontent.com/netology-code/sysadm-homeworks/devsys10/04-script-01-bash/README.md). Заполните недостающие части документа решением задач (заменяйте `???`, ОСТАЛЬНОЕ В ШАБЛОНЕ НЕ ТРОГАЙТЕ чтобы не сломать форматирование текста, подсветку синтаксиса и прочее, иначе можно отправиться на доработку) и отправляйте на проверку. Вместо логов можно вставить скриншоты по желани.

---


# Домашнее задание к занятию "4.1. Командная оболочка Bash: Практические навыки"

## Обязательная задача 1

Есть скрипт:
```bash
a=1
b=2
c=a+b
d=$a+$b
e=$(($a+$b))
```

Какие значения переменным c,d,e будут присвоены? Почему?

| Переменная  | Значение | Обоснование |
| ------------- | ------------- | ------------- |
| `c`  | "a+b"  | так как указали текст а не переменные |
| `d`  | "1+3"  | команда преобразовала вывела значения переменных, но не выполнила арифметическйо операции так как по умолчанию это строки  |
| `e`  | "3"  | так как теперь за счет скобок мы дали команду на выполнение арифметической операции со значениями переменных |


## Обязательная задача 2
На нашем локальном сервере упал сервис и мы написали скрипт, который постоянно проверяет его доступность, записывая дату проверок до тех пор, пока сервис не станет доступным (после чего скрипт должен завершиться). В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на Жёстком Диске постоянно уменьшается. Что необходимо сделать, чтобы его исправить:
```bash
while ((1==1)
do
	curl https://localhost:4757
	if (($? != 0))
	then
		date >> curl.log
	fi
done
```

### Ваш скрипт:
```bash
1. в условии цикла нет закрывающей скобки ) 
2. нужно добавить sleep $timeout для задания интервала, чтобы не было частых проверок
3. нужно добавить  else exit  чтоб выйти из цикла
      

while (( 1 == 1 ))
    do
        curl https://localhost:4757
        if (($? != 0))
        then
            date >> curl.log
        else exit
        fi
        sleep 5
    done

```

## Обязательная задача 3
Необходимо написать скрипт, который проверяет доступность трёх IP: `192.168.0.1`, `173.194.222.113`, `87.250.250.242` по `80` порту и записывает результат в файл `log`. Проверять доступность необходимо пять раз для каждого узла.

### Ваш скрипт:
```bash
#!/usr/bin/env bash
a=192.168.0.1
b=173.194.222.113
c=87.250.250.242
d=80
declare -i i
i=1
while ((i<=5))
do
  echo -e "\n test no.$i \n" &>> curl.log
  echo -e "\n IP $a \n" &>> curl.log
  curl http://$a:$d --connect-timeout 2 -s -S &>> curl.log
  echo -e "\n IP $b \n" &>> curl.log
  curl http://$b:$d --connect-timeout 2 -s -S &>> curl.log
  echo -e "\n IP $c \n" &>> curl.log
  curl http://$c:$d --connect-timeout 2 -s -S &>> curl.log
  let "i+=1"
done
```

## Обязательная задача 4
Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, пока один из узлов не окажется недоступным. Если любой из узлов недоступен - IP этого узла пишется в файл error, скрипт прерывается.

### Ваш скрипт:
```bash
#!/usr/bin/env bash
a=192.168.0.1
b=173.194.222.113
c=87.250.250.242
array_ip=($b $c $a)
d=80
while ((1==1))
do
 for k in ${array_ip[@]}
 do
  curl http://$k:$d --connect-timeout 2 -s -S
  if (($?!=0))
  then
  echo IP $k &>> curl.error
  exit
  fi
 done
done
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Мы хотим, чтобы у нас были красивые сообщения для коммитов в репозиторий. Для этого нужно написать локальный хук для git, который будет проверять, что сообщение в коммите содержит код текущего задания в квадратных скобках и количество символов в сообщении не превышает 30. Пример сообщения: \[04-script-01-bash\] сломал хук.

### Ваш скрипт:
```bash
???
```