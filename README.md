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
PID     TTY     STAT    TIME    USER    COMMAND
1     ?         S    9066    root      systemd
2     ?         S    133     root      kthreadd
3     ?         I    0       root      rcu_gp
4     ?         I    0       root      rcu_par_gp
5     ?         I    0       root      slub_flushwq
6     ?         I    0       root      netns
8     ?         I    0       root      kworker/0:0H-events_highpri
10    ?         I    0       root      mm_percpu_wq
11    ?         I    0       root      rcu_tasks_kthread
12    ?         I    0       root      rcu_tasks_rude_kthread
13    ?         I    0       root      rcu_tasks_trace_kthread
14    ?         S    348237  root      ksoftirqd/0
15    ?         I    148165  root      rcu_preempt
16    ?         S    2795    root      migration/0
18    ?         S    0       root      cpuhp/0
19    ?         S    0       root      cpuhp/1
20    ?         S    2926    root      migration/1
21    ?         S    211915  root      ksoftirqd/1
23    ?         I    0       root      kworker/1:0H-events_highpri
24    ?         S    0       root      cpuhp/2
25    ?         S    2995    root      migration/2
26    ?         S    184748  root      ksoftirqd/2
28    ?         I    0       root      kworker/2:0H-events_highpri
29    ?         S    0       root      cpuhp/3
30    ?         S    2896    root      migration/3
31    ?         S    226322  root      ksoftirqd/3
33    ?         I    0       root      kworker/3:0H-events_highpri
34    ?         S    0       root      cpuhp/4
35    ?         S    2806    root      migration/4
36    ?         S    413435  root      ksoftirqd/4
38    ?         I    0       root      kworker/4:0H-events_highpri
39    ?         S    0       root      cpuhp/5
40    ?         S    2796    root      migration/5
41    ?         S    180725  root      ksoftirqd/5
43    ?         I    0       root      kworker/5:0H-events_highpri
44    ?         S    0       root      cpuhp/6
45    ?         S    2780    root      migration/6
46    ?         S    154128  root      ksoftirqd/6
48    ?         I    0       root      kworker/6:0H-events_highpri
49    ?         S    0       root      cpuhp/7
50    ?         S    2978    root      migration/7
51    ?         S    145991  root      ksoftirqd/7
53    ?         I    0       root      kworker/7:0H-events_highpri
62    ?         S    0       root      kdevtmpfs
63    ?         I    0       root      inet_frag_wq
64    ?         S    0       root      kauditd
65    ?         S    372     root      khungtaskd
66    ?         S    0       root      oom_reaper
67    ?         I    0       root      writeback
68    ?         S    28886   root      kcompactd0
69    ?         S    0       root      ksmd
71    ?         S    7295    root      khugepaged
72    ?         I    0       root      kintegrityd
73    ?         I    0       root      kblockd
74    ?         I    0       root      blkcg_punt_bio
81    ?         I    0       root      tpm_dev_wq
82    ?         I    0       root      edac-poller
83    ?         I    0       root      devfreq_wq
84    ?         I    215     root      kworker/1:1H-kblockd
85    ?         S    434     root      kswapd0
91    ?         I    0       root      kthrotld
93    ?         S    0       root      irq/122-aerdrv
94    ?         S    0       root      irq/123-aerdrv
95    ?         S    0       root      irq/124-aerdrv
96    ?         S    0       root      irq/124-pciehp
97    ?         I    0       root      acpi_thermal_pm
98    ?         S    6180    root      hwrng
100   ?         I    0       root      mld
101   ?         I    388     root      kworker/0:1H-kblockd
102   ?         I    0       root      ipv6_addrconf
107   ?         I    0       root      kstrp
112   ?         I    0       root      zswap-shrink
113   ?         I    0       root      kworker/u17:0-hci0
157   ?         I    155     root      kworker/2:1H-kblockd
168   ?         I    202     root      kworker/3:1H-kblockd
169   ?         I    149     root      kworker/5:1H-kblockd
170   ?         I    327     root      kworker/7:1H-kblockd
171   ?         I    1951    root      kworker/6:1H-kblockd
182   ?         I    222     root      kworker/4:1H-kblockd
202   ?         S    0       root      irq/126-SYNA3067:00
203   ?         I    0       root      ata_sff
209   ?         S    0       root      scsi_eh_0
210   ?         I    0       root      scsi_tmf_0
211   ?         S    0       root      scsi_eh_1
212   ?         I    0       root      scsi_tmf_1
213   ?         S    0       root      scsi_eh_2
214   ?         I    0       root      scsi_tmf_2
224   ?         I    0       root      kdmflush/254:0
225   ?         I    0       root      kdmflush/254:1
262   ?         S    4690    root      jbd2/dm-0-8
263   ?         I    0       root      ext4-rsv-conver
308   ?         S    2371    root      systemd-journal
328   ?         S    1489    root      systemd-udevd
390   ?         S    0       root      irq/130-mei_me
431   ?         S    0       root      watchdogd
486   ?         I    0       root      cfg80211
489   ?         I    0       root      cryptd
514   ?         S    0       root      irq/131-iwlwifi
623   ?         S    0       root      card0-crtc0
624   ?         S    0       root      card0-crtc1
625   ?         S    0       root      card0-crtc2
626   ?         I    0       root      kworker/u17:1-hci0
632   ?         I    0       root      ext4-rsv-conver
693   ?         S    3587    systemd-timesyncsystemd-timesyn
733   ?         S    6       root      bluetoothd
734   ?         S    1512    root      cron
735   ?         S    716     messagebusdbus-daemon
737   ?         S    3900    root      systemd-logind
744   ?         S    3801    root      wpa_supplicant
798   1025      S    0       root      agetty
802   ?         S    15      root      sshd
1053  ?         S    49      root      systemd
1054  ?         S    0       root      sd-pam
3651  ?         I    0       root      iprt-VBoxWQueue
3654  ?         S    247     root      iprt-VBoxTscThread
3803  ?         S    0       root      dbus-daemon
14399 ?         I    0       root      dio/dm-0
42395 ?         S    560102  root      VBoxSVC
66511 ?         S    28882   root      VBoxNetDHCP
123432?         S    478900154root      VBoxHeadless
152104?         S    401     _rpc      rpcbind
152499?         I    0       root      rpciod
152500?         I    0       root      xprtiod
152694?         S    0       statd     rpc.statd
152713?         I    0       root      nfsiod
169314?         I    2434    root      kworker/3:3-events
179393?         I    517     root      kworker/5:0-events
181177?         I    0       root      tls-strp
181604?         I    414     root      kworker/1:1-events
182253?         I    614     root      kworker/0:3-events
199929?         S    67749679root      VBoxHeadless
201904?         I    187     root      kworker/6:0-events
201924?         I    167     root      kworker/4:0-events
202544?         I    1329    root      kworker/7:3-events
202597?         I    100     root      kworker/2:0-events
202822?         I    0       root      kworker/2:1-cgroup_destroy
203286?         I    0       root      kworker/7:0
203346?         I    0       root      kworker/6:2
203362?         I    0       root      kworker/0:1
203377?         I    0       root      kworker/5:1
203393?         I    0       root      kworker/1:0
203426?         I    0       root      kworker/4:2-cgroup_destroy
203441?         I    0       root      kworker/3:1-cgroup_destroy
203491?         S    10      root      sshd
203497?         S    0       root      bash
203509?         S    51      root      sshd
207039?         I    0       root      kworker/u16:2-events_unbound
214131?         I    2       root      kworker/u16:0-events_unbound
214135?         I    0       root      kworker/u16:1-events_unbound
214137?         I    0       root      kworker/u16:3-events_unbound
20351534816     S    5       root      bash
21413934816     S    0       root      getpoclist
21414034816     S    15      root      getpoclist
21414134816     S    0       root      sort
```
</details>
