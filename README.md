# Домашнее задание к занятию "`Уязвимости и атаки на информационные системы`" - `Тен Денис`

### Задание 1

Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.

Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.

Просканируйте эту виртуальную машину, используя **nmap**.

Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.

Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.

Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.

Ответьте на следующие вопросы:

- Какие сетевые службы в ней разрешены?
- Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)
  
*Приведите ответ в свободной форме.*  

### Решение Задание 1

```sh
└─# nmap -sV 10.46.48.152
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-27 06:52 EDT
Nmap scan report for 10.46.48.152
Host is up (0.0016s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp   open  telnet      Linux telnetd
25/tcp   open  smtp        Postfix smtpd
53/tcp   open  domain      ISC BIND 9.4.2
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp  open  rpcbind     2 (RPC #100000)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp  open  exec        netkit-rsh rexecd
513/tcp  open  login?
514/tcp  open  tcpwrapped
1099/tcp open  java-rmi    GNU Classpath grmiregistry
1524/tcp open  bindshell   Metasploitable root shell
2049/tcp open  nfs         2-4 (RPC #100003)
2121/tcp open  ftp         ProFTPD 1.3.1
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp open  vnc         VNC (protocol 3.3)
6000/tcp open  X11         (access denied)
6667/tcp open  irc         UnrealIRCd
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.07 seconds
```


```sh
┌──(root㉿kali)-[~]
└─# searchsploit vsftpd 2.3.4
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                          |  Path
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
vsftpd 2.3.4 - Backdoor Command Execution                                                                                                                                               | unix/remote/49757.py
vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)                                                                                                                                  | unix/remote/17491.rb
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```
https://www.exploit-db.com/exploits/49757
https://www.exploit-db.com/exploits/17491

```sh
┌──(root㉿kali)-[~]
└─# searchsploit BIND 9.4.2
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                          |  Path
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
BIND 9.4.1 < 9.4.2 - Remote DNS Cache Poisoning (Metasploit)                                                                                                                            | multiple/remote/6122.rb
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```
https://www.exploit-db.com/exploits/6122


```sh
┌──(root㉿kali)-[~]
└─# searchsploit VNC 3.3
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                          |  Path
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
RealVNC 3.3.7 - Client Buffer Overflow (Metasploit)                                                                                                                                     | windows/remote/16489.rb
WinVNC Web Server 3.3.3r7 - GET Overflow (Metasploit)                                                                                                                                   | windows/remote/16491.rb
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```
https://www.exploit-db.com/exploits/16489
https://www.exploit-db.com/exploits/16491


### Задание 2

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

- Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
- Как отвечает сервер?

*Приведите ответ в свободной форме.*

### Решение Задание 2


- Режим SYN:
  - В режиме SYN Nmap отправляет SYN пакеты на целевой порт.
  - Если порт открыт, Nmap получает ответ ACK.
  - Если порт закрыт, Nmap получает RST пакет.
  - Сетевой трафик включает отправку SYN и ACK пакетов.

```sh
└─# nmap -sS 10.159.86.74
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-27 07:39 EDT
Nmap scan report for ubuntu-client.dit.local (10.159.86.74)
Host is up (0.00072s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
512/tcp  open  exec
513/tcp  open  login
514/tcp  open  shell
1099/tcp open  rmiregistry
1524/tcp open  ingreslock
2049/tcp open  nfs
2121/tcp open  ccproxy-ftp
3306/tcp open  mysql
5432/tcp open  postgresql
5900/tcp open  vnc
6000/tcp open  X11
6667/tcp open  irc
8009/tcp open  ajp13
8180/tcp open  unknown
MAC Address: 00:0C:29:FA:DD:34 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.23 seconds

```

- Режим FIN:
  - В режиме FIN Nmap отправляет пакеты без флагов на целевой порт.
  - Если порт закрыт, удаленный хост должен отправить RST пакет в ответ.
  - Если порт открыт, удаленный хост должен проигнорировать пакет.
  - Сетевой трафик включает отправку пакетов без флагов и, возможно, RST пакетов.

```
└─# nmap -sA 10.46.48.152
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-27 07:17 EDT
Nmap scan report for 10.46.48.152
Host is up (0.00096s latency).
All 1000 scanned ports on 10.46.48.152 are in ignored states.
Not shown: 1000 unfiltered tcp ports (reset)

Nmap done: 1 IP address (1 host up) scanned in 0.19 seconds
```

- Режим Xmas:
  - В режиме Xmas Nmap отправляет пакеты с установленными флагами FIN, PSH и URG на целевой порт.
  - Если порт закрыт, удаленный хост должен отправить RST пакет в ответ.
  - Если порт открыт, удаленный хост должен проигнорировать пакет.
  - Сетевой трафик включает отправку пакетов с установленными флагами FIN

```sh
└─# nmap -sX 10.159.86.74
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-27 07:39 EDT
Nmap scan report for ubuntu-client.dit.local (10.159.86.74)
Host is up (0.000060s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE         SERVICE
21/tcp   open|filtered ftp
22/tcp   open|filtered ssh
23/tcp   open|filtered telnet
25/tcp   open|filtered smtp
53/tcp   open|filtered domain
80/tcp   open|filtered http
111/tcp  open|filtered rpcbind
139/tcp  open|filtered netbios-ssn
445/tcp  open|filtered microsoft-ds
512/tcp  open|filtered exec
513/tcp  open|filtered login
514/tcp  open|filtered shell
1099/tcp open|filtered rmiregistry
1524/tcp open|filtered ingreslock
2049/tcp open|filtered nfs
2121/tcp open|filtered ccproxy-ftp
3306/tcp open|filtered mysql
5432/tcp open|filtered postgresql
5900/tcp open|filtered vnc
6000/tcp open|filtered X11
6667/tcp open|filtered irc
8009/tcp open|filtered ajp13
8180/tcp open|filtered unknown
MAC Address: 00:0C:29:FA:DD:34 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 1.40 seconds

```
  Режим UDP:  
  - В режиме UDP, Nmap отправляет UDP пакеты на целевой порт и анализирует ответы. Если порт открыт, Nmap получит ответ. Если порт закрыт, Nmap получит ICMP пакет "Destination Unreachable" или не получит никакого ответа. В Wireshark вы увидите UDP пакеты и возможно ICMP пакеты "Destination Unreachable".

```sh
