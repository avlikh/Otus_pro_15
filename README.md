# OTUS PRO Homework 15 Process

## Домашняя работа 15: Управление процессами 

### Домашнее задание:
1. написать свою реализацию ps ax используя анализ /proc
*  Результат ДЗ - рабочий скрипт который можно запустить   
---
## Выполнение задания:

Создадим скрипт в папке  **/usr/bin/**   
Скрипт назовем: **getpoclist**
```
cat > /usr/bin/getpoclist
```
1. Скопируйте и вставьте текст скрипта:
```
#!/bin/bash

# Скрипт получения информации о процессах
    echo -e "PID\tTTY\tSTAT\tTIME\tUSER\tCOMMAND"
    for pid in /proc/[0-9]*; do
        pid=${pid#/proc/}  # Получаем номер процесса
        if [[ -d "/proc/$pid" ]]; then
            # Чтение информации из /proc/[pid]/stat
            stat_info=$(cat /proc/$pid/stat)
            comm=$(echo $stat_info | awk '{print $2}' | tr -d '()')  # Извлекаем имя процесса
            state=$(echo $stat_info | awk '{print $3}')
            ppid=$(echo $stat_info | awk '{print $4}')
            tty=$(echo $stat_info | awk '{print $7}')
            time=$(echo $stat_info | awk '{print $14 + $15}')
            
            # Чтение информации о пользователе из /proc/[pid]/status
            uid=$(grep -m 1 "^Uid:" /proc/$pid/status | awk '{print $2}')
            user=$(getent passwd $uid | cut -d: -f1)

            # Вывод информации о процессе
            if [[ "$tty" == "0" ]]; then
                tty="?"
            fi
            printf "%-6s%-10s%-5s%-8s%-10s%-s\n" "$pid" "$tty" "$state" "$time" "$user" "$comm"
        fi
    done  | sort -n  # Сортировка по PID (по умолчанию)
```
2. Нажмите комбинацию кнопок: **[enter]** и **ctrl+d**

3. Сделаем скрипт исполняемым: 
```
chmod +x /usr/bin/getpoclist
```
4. Проверим работу скрипта:
```
getpoclist
```
<details>
<summary> результат выполнения команды: </summary>

```

```
</details>
