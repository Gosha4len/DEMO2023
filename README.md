# DEMO2023
![Static Badge](https://img.shields.io/badge/Debian-12-a80030) ![Static Badge](https://img.shields.io/badge/Windows-10-0079d7) ![Static Badge](https://img.shields.io/badge/Windows-Server%202022-001190) ![Static Badge](https://img.shields.io/badge/Cisco-CSR-00a0da)
# Образец задания
Образец задания для демонстрационного экзамена по комплекту оценочной
документации.

![image](/screenshots/scheme_logo.png)

## Виртуальные машины и коммутация.
Необходимо выполнить создание и базовую конфигурацию виртуальных
машин.

1. На основе предоставленных ВМ или шаблонов ВМ создайте отсутствующие виртуальные машины в соответствии со схемой.  
   -	Характеристики ВМ установите в соответствии с Таблицей 1;
   -	Коммутацию (если таковая не выполнена) выполните в соответствии со схемой сети.	 
2.  Имена хостов в созданных ВМ должны быть установлены в соответствии со схемой.
3.  Адресация должна быть выполнена в соответствии с Таблицей 1;
4.  Обеспечьте ВМ дополнительными дисками, если таковое необходимо в соответствии с Таблицей 1;

## Таблица 1. Характеристики ВМ

|Name VM         |ОС                  |RAM             |CPU             |IP                    |Additionally                       |
|  ------------- | -------------      | -------------  |  ------------- |  -------------       |  -------------                    |  
|RTR-L           |Cisco CSR           |4 GB            |1               |4.4.4.100/24          |                                   |
|                |                    |                |                |192.168.100.254/24    |                                   |
|RTR-R           |Cisco CSR           |4 GB            |1               |5.5.5.100/24          |                                   |
|                |                    |                |                |172.16.100.254 /24    |                                   |
|SRV             |WS 2022             |2 GB            |1               |192.168.100.200/24    |Доп диски 2 шт по 1 GB             |
|WEB-L           |Debian 12           |1 GB            |1               |192.168.100.100/24    |                                   |
|WEB-R           |Debian 12           |1 GB            |1               |172.16.100.100/24     |                                   |
|ISP             |Debian 12           |2 GB            |1               |4.4.4.1/24            |                                   |
|                |                    |                |                |5.5.5.1/24            |                                   |
|                |                    |                |                |3.3.3.1/24            |                                   |
|CLI             |Win 10              |1 GB            |1               |3.3.3.10/24           |                                   |


### 1. На основе предоставленных ВМ или шаблонов ВМ создайте отсутствующие виртуальные машины в соответствии со схемой.
Убедитесь что все ВМ созданы в соответствии со схемой 

![image](/screenshots/vm.png)


### 2.  Имена хостов в созданных ВМ должны быть установлены в соответствии со схемой.

### 3.  Адресация должна быть выполнена в соответствии с Таблицей 1;

#### CLI
1. "Изменение хостнейма"
```
1) ЛКМ по значку Windows 
   2) Windows PowerShell
      3) вводим команду rename-computer -newname CLI
         4) Перезагружаем VM
```
2. "Настройка сети"
```
1) ПКМ по иконке Network
   2) Open Network & Internet settings
      3) Change adapter options
       5) ПКМ по сетевому адаптеру
         6) Выбираете ваш сетевой адаптер и заполняете его:
```
![image](/screenshots/cli_ipv4.png)

#### ISP

Подключить DVD debian-BD-1.iso

```
apt-cdrom add
apt install network-manager -y
```
1. "Изменение хостнейма"
```
1) root@debian:~# nmtui
   2) Set system hostname
      3) Выдаём имя ISP
         4) OK
            5) Quit
               6) root@debian:~# bash
```
2. "Настройка сети"
```
1) root@ISP:~# nmtui
   2) Edit a connection
      3) Wired connection 3
         4) IPv4 CONFIGURATION ставим значение  <Manual>
            5) Заполняем:
```
![image](/screenshots/isp_ens256.png)
```
6) OK
   7) Wired connection 2
      8) IPv4 CONFIGURATION ставим значение  <Manual>
         9)  Заполняем:
```
![image](/screenshots/isp_ens224.png)
```
10) OK
   11) Wired connection 2
      12) IPv4 CONFIGURATION ставим значение  <Manual>
         13)  Заполняем:
```
![image](/screenshots/isp_ens192.png)
```
11) OK
   12) Back
      13) Quit
```
3. "Проверка ip-адресов"
```
root@ISP:~# ip a
```
![image](/screenshots/isp_ipa.png)

#### RTR-L
1. "Изменение хостнейма"
```
router>
router>en
router#conf t
router(config)#hostname RTR-L
```
2. "Настройка сети"
```
RTR-L(config)#int gi1
RTR-L(config-if)#ip add 4.4.4.100 255.255.255.0
RTR-L(config-if)#no sh
```
```
RTR-L(config)#int gi2
RTR-L(config-if)#ip add 192.168.100.254 255.255.255.0
RTR-L(config-if)#no sh
```
3. "Проверка адаптеров и ip-адресов"
```
RTR-L#sh ip int b
```
![image](/screenshots/rtr-l_sh1.png)
#### RTR-R
1. "Изменение хостнейма"
```
router>
router>en
router#conf t
router(config)#hostname RTR-R
```
2. "Настройка сети"
```
RTR-R(config)#int gi1
RTR-R(config-if)#ip add 5.5.5.100 255.255.255.0
RTR-R(config-if)#no sh
```
```
RTR-R(config)#int gi2
RTR-R(config-if)#ip add 172.16.100.254 255.255.255.0
RTR-R(config-if)#no sh
```
3. "Проверка адаптеров и ip-адресов"
```
RTR-L#sh ip int b
```
![image](/screenshots/rtr-r_sh1.png)

#### WEB-L

Подключить DVD debian-BD-1.iso

```
apt-cdrom add
apt install network-manager -y
```
1. "Изменение хостнейма"
```
1) root@debian:~# nmtui
   2) Set system hostname
      3) Выдаём имя WEB-L
         4) OK
            5) Quit
               6) root@debian:~# bash
```
2. "Настройка сети"
```
1) root@WEB-L:~# nmtui
   2) Edit a connection
      3) Wired connection 1
         4) IPv4 CONFIGURATION ставим значение  <Manual>
            5) Заполняем:
```
![image](/screenshots/web-l_ens192.png)
```
6) OK
   7) Back
      8) Quit
```
3. "Проверка ip-адресов"
```
root@WEB-L:~# ip a
```
![image](/screenshots/web-l_ipa1.png)

#### SRV
1. "Изменение хостнейма"
```
1) WIN + R
   2) вводим sconfig
      4) вводим число 2 (Computer Name):
```
![image](/screenshots/srv_sconfig1.png)
```
5) вводим хостнейм SRV:
```
![image](/screenshots/srv_sconfig2.png)
```
6) Перезагружаем VM
```      
2. "Настройка сети"
```
1) WIN + R
   2) вводим sconfig
      3) вводим команду sconfig
         4) вводим число 8 (Network Settings):
```
![images](/screenshots/srv_sconfig3.png)
```
5) выбираем сетевой адаптер 1:
```
![images](/screenshots/srv_sconfig4.png)
```
6) пишем S, ставим статику, выдаём ip-адрес, маску, gateway:
```
![images](/screenshots/srv_sconfig5.png)
```
7) вводим число 2 (Set DNS Servers), вводим основной DNS сервер и альтернативный:
```
![images](/screenshots/srv_sconfig6.png)

3. "Отключение Firewall`a"
```
1) WIN + R
   2) вводим powershell
      3) вводим команду: 
```
![image](/screenshots/srv_firewall.png)
#### WEB-R
```
1) root@debian:~# nmtui
   2) Set system hostname
      3) Выдаём имя WEB-R
         4) OK
            5) Quit
               6) root@debian:~# bash
```
2. "Настройка сети"
```
1) root@WEB-R:~# nmtui
   2) Edit a connection
      3) Wired connection 1
         4) IPv4 CONFIGURATION ставим значение <Manual>
            5) Заполняем:
```
![image](/screenshots/web-r_ens192.png)
```
6) OK
   7) Back
      8) Quit
```
3. "Проверка ip-адресов"
```
root@WEB-R:~# ip a
```
![image](/screenshots/web-r_ipa1.png)


## Сетевая связность.
В рамках данного модуля требуется обеспечить сетевую связность между
регионами работы приложения, а также обеспечить выход ВМ в имитируемую
сеть “Интернет”

1. Сети, подключенные к ISP, считаются внешними:
   - Запрещено прямое попадание трафика из внутренних сетей во внешние и наоборот;
2. Платформы контроля трафика, установленные на границах регионов, должны выполнять трансляцию трафика, идущего из соответствующих внутренних сетей во внешние сети стенда и в сеть Интернет.
   - Трансляция исходящих адресов производится в адрес платформы, расположенный во внешней сети.    
3. Между платформами должен быть установлен защищенный туннель, позволяющий осуществлять связь между регионами с применением внутренних адресов.
   - Трафик, проходящий по данному туннелю, должен быть защищен:
     - Платформа ISP не должна иметь возможности просматривать содержимое пакетов, идущих из одной внутренней сети в другую.
   - Туннель должен позволять защищенное взаимодействие между платформами управления трафиком по их внутренним адресам
     - Взаимодействие по внешним адресам должно происходит без применения туннеля и шифрования
   - Трафик, идущий по туннелю между регионами по внутренним адресам, не должен транслироваться.
 4. Платформа управления трафиком RTR-L выполняет контроль входящего трафика согласно следующим правилам:
    - Разрешаются подключения к портам DNS, HTTP и HTTPS для всех клиентов;
      - Порты необходимо для работы настраиваемых служб
    - Разрешается работа выбранного протокола организации защищенной связи; 
      - Разрешение портов должно быть выполнено по принципу “необходимо и достаточно”
    - Разрешается работа протоколов ICMP;
    - Разрешается работа протокола SSH;
    - Прочие подключения запрещены;
    - Для обращений к платформам со стороны хостов, находящихся внутри регионов, ограничений быть не должно;
5. Платформа управления трафиком RTR-R выполняет контроль входящего трафика согласно следующим правилам:
   - Разрешаются подключения к портам HTTP и HTTPS для всех клиентов;
     - Порты необходимо для работы настраиваемых служб
   - Разрешается работа выбранного протокола организации защищенной связи;
     - Разрешение портов должно быть выполнено по принципу необходимо и достаточно”
   - Разрешается работа протоколов ICMP;
   - Разрешается работа протокола SSH;
   - Прочие подключения запрещены;
   - Для обращений к платформам со стороны хостов, находящихся внутри регионов, ограничений быть не должно;
6. Обеспечьте настройку служб SSH региона Left:
   - Подключения со стороны внешних сетей по протоколу к платформе управления трафиком RTR-L на порт 2222 должны быть перенаправлены на ВМ Web-L;
   - Подключения со стороны внешних сетей по протоколу к платформе управления трафиком RTR-R на порт 2244 должны быть перенаправлены на ВМ Web-R;

### 1. Сети, подключенные к ISP, считаются внешними:
#### ISP forward
```
1) root@ISP:~# nano /etc/sysctl.conf
   2) раскомментировать строчку net.ipv4.ip_forward=1 :
```
![image](/screenshots/isp_1.png)
```
3) Ctrl + x
   4) Ctrl + y
      5) Enter
         6)root@ISP:~# sysctl -p
```
#### RTR-L Gateway
```
RTR-L(config)#ip route 0.0.0.0 0.0.0.0 4.4.4.1
```
#### RTR-R Gateway
```
RTR-R(config)#ip route 0.0.0.0 0.0.0.0 5.5.5.1
```
### 2. Платформы контроля трафика, установленные на границах регионов, должны выполнять трансляцию трафика, идущего из соответствующих внутренних сетей во внешние сети стенда и в сеть Интернет.
#### NAT
на внутреннем интерфейсе - ip nat inside

на внешнем интерфейсе - ip nat outside
#### RTR-L NAT
1. "Настройка NAT"
```
RTR-L(config)#int gi1
RTR-L(config-if)#ip nat outside

RTR-L(config)#int gi2
RTR-L(config-if)#ip nat inside

RTR-L(config)#access-list 1 permit 192.168.100.0 0.0.0.255
RTR-L(config)#ip nat inside source list 1 int gi1 overload
```
2. "Проверка NAT" 
```
Вы должны ping`овать с WEB-L до ISP:
```
![image](/screenshots/web-l_nat_ping.png)
#### RTR-R NAT
1. "Настройка NAT"
```
RTR-R(config)#int gi1
RTR-R(config-if)#ip nat outside

RTR-R(config)#int gi2
RTR-R(config-if)#ip nat inside

RTR-R(config)#access-list 1 permit 172.16.100.0 0.0.0.255
RTR-R(config)#ip nat inside source list 1 int gi1 overload
```
2. "Проверка NAT"
```
Вы должны ping`овать с WEB-R до ISP:
```
![image](/screenshots/web-r_nat_ping.png)
### 3. Между платформами должен быть установлен защищенный туннель, позволяющий осуществлять связь между регионами с применением внутренних адресов.
#### RTR-L GRE
1. "Настройка GRE"
```
RTR-L(config)#int tun1
RTR-L(config-if)#ip address 172.16.1.1 255.255.255.0
RTR-L(config-if)#tunnel mode gre ip
RTR-L(config-if)#tunnel source 4.4.4.100
RTR-L(config-if)#tunnel destination 5.5.5.100

RTR-L(config)#router eigrp 1
RTR-L(config-router)#network 192.168.100.0 0.0.0.255
RTR-L(config-router)#network 172.16.1.0 0.0.0.255
```
2. "Проверка GRE с применением внутренних адресов"
```
Вы должны ping`овать устройства из подсети 192.168.100.0 до 172.16.100.0 :
```
![image](/screenshots/web-l_gre_ping.png)
#### RTR-R GRE
1. "Настройка GRE"
```
RTR-R(config)#int tun1
RTR-R(config-if)#ip address 172.16.1.2 255.255.255.0
RTR-R(config-if)#tunnel mode gre ip
RTR-R(config-if)#tunnel source 5.5.5.100
RTR-R(config-if)#tunnel destination 4.4.4.100

RTR-R(config)#router eigrp 1
RTR-R(config-router)#network 172.16.100.0 0.0.0.255
RTR-R(config-router)#network 172.16.1.0 0.0.0.255
```
2. "Проверка GRE с применением внутренних адресов"
```
Вы должны ping`овать устройства из подсети 172.16.100.0 до 192.168.100.0 :
```
![image](/screenshots/web-r_gre_ping.png)
![image](/screenshots/web-r_gre_ping2.png)
#### RTR-L IPSec
1. "Настройка IPSec"
```
RTR-L(config)#crypto isakmp policy 1
RTR-L(config-isakmp)#encr aes
RTR-L(config-isakmp)#authentication pre-share
RTR-L(config-isakmp)#hash sha256
RTR-L(config-isakmp)#group 14

RTR-L(config)#crypto isakmp key secret address 5.5.5.100
RTR-L(config)#crypto isakmp nat keepalive 5
RTR-L(config)#crypto ipsec transform-set IPSEC esp-aes 256 esp-sha256-hmac
RTR-L(cfg-crypto-trans)#mode tunnel

RTR-L(config)#crypto ipsec profile VTI
RTR-L(ipsec-profile)#set transform-set IPSEC

RTR-L(config)#int tun1
RTR-L(config)#tunnel mode ipsec ipv4
RTR-L(config)#tunnel protection ipsec profile VTI
```
2. "Проверка IPSec с применением внутренних адресов"
```
Вы должны ping`овать устройства из подсети 192.168.100.0 до 172.16.100.0 :
```
![image](/screenshots/web-l_ipsec_ping.png)

#### RTR-R IPSec
1. "Настройка IPSec"
```
RTR-R(config)#crypto isakmp policy 1
RTR-R(config-isakmp)#encr aes
RTR-R(config-isakmp)#authentication pre-share
RTR-R(config-isakmp)#hash sha256
RTR-R(config-isakmp)#group 14

RTR-R(config)#crypto isakmp key secret address 4.4.4.100
RTR-R(config)#crypto isakmp nat keepalive 5
RTR-R(config)#crypto ipsec transform-set IPSEC esp-aes 256 esp-sha256-hmac
RTR-R(cfg-crypto-trans)#mode tunnel

RTR-R(config)#crypto ipsec profile VTI
RTR-R(ipsec-profile)#set transform-set IPSEC

RTR-R(config)#int tun1
RTR-R(config)#tunnel mode ipsec ipv4
RTR-R(config)#tunnel protection ipsec profile VTI
```
2. "Проверка IPSec с применением внутренних адресов"
```
Вы должны ping`овать устройства из подсети 172.16.100.0 до 192.168.100.0 :
```
![image](/screenshots/web-r_ipsec_ping.png)
![image](/screenshots/web-r_ipsec_ping2.png)

###  4. Платформа управления трафиком RTR-L выполняет контроль входящего трафика согласно следующим правилам:
#### RTR-L ACL
```
RTR-L(config)#ip access-list extended Lnew
```
```
RTR-L(config-ext-nacl)#permit tcp any any established
RTR-L(config-ext-nacl)#permit tcp any host 4.4.4.100 eq 80 
RTR-L(config-ext-nacl)#permit tcp any host 4.4.4.100 eq 443 
RTR-L(config-ext-nacl)#permit tcp any host 4.4.4.100 eq 2222 
```
```
RTR-L(config-ext-nacl)#permit udp host 4.4.4.1 eq 123 any
RTR-L(config-ext-nacl)#permit udp host 4.4.4.100 eq 53 any
RTR-L(config-ext-nacl)#permit udp host 5.5.5.100 host 4.4.4.100 eq 500
```
```
RTR-L(config-ext-nacl)#permit esp any any
RTR-L(config-ext-nacl)#permit icmp any any
```
```
RTR-L(config)#int gi1 
RTR-L(config-if)#ip access-group Lnew in
```
#### RTR-R ACL
```
RTR-R(config)#ip access-list extended Rnew
RTR-R(config-ext-nacl)#permit tcp any any established
RTR-R(config-ext-nacl)#permit tcp any host 5.5.5.100 eq 80 
RTR-R(config-ext-nacl)#permit tcp any host 5.5.5.100 eq 443 
RTR-R(config-ext-nacl)#permit tcp any host 5.5.5.100 eq 2244 
```
```
RTR-R(config-ext-nacl)#permit udp host 4.4.4.100 host 5.5.5.100 eq 500
```
```
RTR-R(config-ext-nacl)#permit esp any any
RTR-R(config-ext-nacl)#permit icmp any any
```
```
RTR-R(config)#int gi1 
RTR-R(config-if)#ip access-group Rnew in
```

### 6. Обеспечьте настройку служб SSH региона Left:
#### RTR-L SSH
```
RTR-L(config)#ip nat inside source static tcp 192.168.100.100 22 4.4.4.100 2222
```
#### RTR-R SSH
```
RTR-R(config)#ip nat inside source static tcp 172.16.100.100 22 5.5.5.100 2244
```
#### SSH WEB-L
1. "Настройка SSH"
```
1)root@WEB-L:~# apt install openssh-server ssh -y
   2)root@WEB-L:~# systemctl start sshd
      3)root@WEB-L:~# systemctl enable ssh
         4)root@WEB-L:~# nano /etc/ssh/sshd_config
            5) раскомментировать и дополнить строчку:
```
![image](/screenshots/web-l_ssh.png)
```
6) Ctrl + x
   7) Ctrl + y
      8) Enter
         9)root@WEB-L:~# systemctl restart sshd

```
2. "Проверка SSH"
```
Вы должны подключиться с CLI до WEB-L по SSH:
```
![image](/screenshots/cli_ssh_web-l.png)
#### SSH WEB-R
1. "Настройка SSH" 
```
1)root@WEB-R:~# apt insapt itall openssh-server ssh -y
   2)root@WEB-R:~# systemctl start sshd
      3)root@WEB-R:~# systemctl enable ssh
         4)root@WEB-R:~# nano /etc/ssh/sshd_config
            5) раскомментировать и дополнить строчку:
```
![image](/screenshots/web-r_ssh.png)
```
6) Ctrl + x
   7) Ctrl + y
      8) Enter
         9)root@WEB-R:~# systemctl restart sshd
```
2. "Проверка SSH"
```
Вы должны подключиться с CLI до WEB-R по SSH:
```
![image](/screenshots/cli_ssh_web-r.png)
## Инфраструктурные службы

В рамках данного модуля необходимо настроить основные
инфраструктурные службы и настроить представленные ВМ на применение этих
служб для всех основных функций. 

1. Выполните настройку первого уровня DNS-системы стенда:
   - Используется ВМ ISP;
   - Обслуживается зона demo.wsr
     - Наполнение зоны должно быть реализовано в соответствии с Таблицей 2;
   - Сервер делегирует зону int.demo.wsr на SRV;
     - Поскольку SRV находится во внутренней сети западного региона, делегирование происходит на внешний адрес маршрутизатора данного региона. 
     - Маршрутизатор региона должен транслировать соответствующие порты DNS-службы в порты сервера SRV
   - Внешний клиент CLI должен использовать DNS-службу, развернутую на ISP, по умолчанию;
2. Выполните настройку второго уровня DNS-системы стенда;
   - Используется ВМ SRV;
   - Обслуживается зона int.demo.wsr;
     - Наполнение зоны должно быть реализовано в соответствии с Таблицей 2;
   - Обслуживаются обратные зоны для внутренних адресов регионов
     - Имена для разрешения обратных записей следует брать из Таблицы 2;
   - Сервер принимает рекурсивные запросы, исходящие от адресов внутренних регионов;
     - Обслуживание клиентов (внешних и внутренних), обращающихся к к зоне int.demo.wsr, должно производится без каких-либо ограничений по адресу источника;
   - Внутренние хосты регионов (равно как и платформы управления трафиком) должны использовать данную DNS-службу для разрешения всех запросов имен;
3. Выполните настройку первого уровня системы синхронизации времени:
   - Используется сервер ISP. 
   - Сервер считает собственный источник времени верным, stratum=4;
   - Сервер допускает подключение только через внешний адрес соответствующей платформы управления трафиком;
     - Подразумевается обращение SRV для синхронизации времени;
   - Клиент CLI должен использовать службу времени ISP;
4. Выполните конфигурацию службы второго уровня времени на SRV
   - Сервер синхронизирует время с хостом ISP;
     - Синхронизация с другими источникам запрещена;
   - Сервер должен допускать обращения внутренних хостов регионов, в том числе и платформ управления трафиком, для синхронизации времени;
   - Все внутренние хосты (в том числе и платформы управления трафиком) должны синхронизировать свое время с SRV;
5. Реализуйте файловый SMB-сервер на базе SRV
   - Сервер должен предоставлять доступ для обмена файлами серверам WEB-L и WEB-R;
   - Сервер, в зависимости от ОС, использует следующие каталоги для хранения файлов:
     - /mnt/storage для систем на базе Linux;
     - Диск R:\ для систем на базе Windows;
   - Хранение файлов осуществляется на диске (смонтированном по указанным выше адресам), реализованном по технологии RAID типа “Зеркало”;
6. Сервера WEB-L и WEB-R должны использовать службу, настроенную на SRV, для обмена файлами между собой:
   - Служба файлового обмена должна позволять монтирование в виде стандартного каталога Linux
     - Разделяемый каталог должен быть смонтирован по адресу /opt/share;
   - Каталог должен позволять удалять и создавать файлы в нем для всех пользователей;
7. Выполните настройку центра сертификации на базе SRV:
   - В случае применения решения на базе Linux используется центр сертификации типа OpenSSL и располагается по адресу /var/ca
   - Выдаваемые сертификаты должны иметь срок жизни не менее 500 дней;
   - Параметры выдаваемых сертификатов:
     - Страна RU;
     - Организация DEMO.WSR;
     - Прочие поля (за исключением CN) должны быть пусты;


## Таблица 2. DNS-записи зон

|Zone            |Type                |Key             |Meaning         |
|  ------------- | -------------      | -------------  |  ------------- |
| demo.wsr       | A                  | isp            | 3.3.3.1        |
|                | A                  | www            | 4.4.4.100      |
|                | A                  | www            | 5.5.5.100      |
### 1. Выполните настройку первого уровня DNS-системы стенда:
#### ISP
1.  "Настройка DNS"
```
1)root@ISP:~# apt install bind9 -y
   2)root@ISP:~# mkdir /opt/dns
      3)root@ISP:~# cp /etc/bind/db.local /opt/dns/demo.db
         4)root@ISP:~# chown -R bind:bind /opt/dns
            5)root@ISP:~# nano /etc/apparmor.d/usr.sbin.named добавить строчку /opt/dns/** rw, :
```
![image](/screenshots/apparmor.png)
```
6) Ctrl + X
   7) Ctrl + Y
      8) Enter
         9) root@ISP:~# systemctl restart apparmor.service
            10) root@ISP:~# nano /etc/bind/named.conf.options добавим и изменим несколько строчек:
```
![image](/screenshots/isp_named.png)

```
11) Ctrl + X
   12) Ctrl + Y
      13) Enter
         14)root@ISP:~# nano /etc/bind/named.conf.default-zones добавим и изменим несколько строчек:
```
![image](/screenshots/isp_zones.png)
```
15) Ctrl + X
   16) Ctrl + Y
      17) Enter
         18)root@ISP:~# nano /opt/dns/demo.db добавим и изменим несколько строчек:
```
![image](/screenshots/demo_db.png)
```
19) Ctrl + X
   20) Ctrl + Y
      21) Enter
         22)root@ISP:~# systemctl restart bind9
```
2. "Проверка DNS"
```
root@ISP:~# systemctl status bind9
```
![image](/screenshots/isp_status_bind9.png)

#### RTR-L

b. Маршрутизатор региона должен транслировать соответствующие порты DNS-службы в порты сервера SRV.

```
RTR-L(config)#ip nat inside source static tcp 192.168.100.200 53 4.4.4.100 53
RTR-L(config)#ip nat inside source static udp 192.168.100.200 53 4.4.4.100 53
```

### 2. Выполните настройку второго уровня DNS-системы стенда;

#### SRV
1. "Настройка DNS"
```
1) Server Manager > Dashboard ЛКМ по Manage:
```
![image](/screenshots/srv_addroles_dns_1.png)
```
2) Add Roles and Features
```
![image](/screenshots/srv_addroles_dns_2.png)
```
3) Next:
```
![image](/screenshots/srv_addroles.png)
```
4) Next:
```
![image](/screenshots/srv_addroles2.png)
```
5) Next:
```
![image](/screenshots/srv_addroles3.png)
```
6) ЛКМ по DNS Server :
```
![image](/screenshots/srv_addroles_dns.png)
```
7) Add Features:
```
![image](/screenshots/srv_addroles_dns2.png)
```
8) Next:
```
![image](/screenshots/srv_addroles_dns3.png)
```
9) Next:
```
![image](/screenshots/srv_addroles_dns4.png)
```
10) Next:
```
![image](/screenshots/srv_addroles_dns5.png)
```
11) Install:
```
![image](/screenshots/srv_addroles_dns6.png)
```
12) Close:
```
![image](/screenshots/srv_addroles_dns7.png)
```
13)  Server Manager > Dashboard ЛКМ по Tools:
```
![image](/screenshots/srv_tools.png)
```
14) DNS:
```
![image](/screenshots/srv_tools2.png)
```
15) DNS
   16) SRV
      17) ПКМ по Forward Lookup Zones, New Zone...
```
![image](/screenshots/srv_dns_manager1.png)
```
18) Next:
```
![image](/screenshots/srv_dns_manager2.png)
```
19) Next:
```
![image](/screenshots/srv_dns_manager3.png)
```
20) вписываем зону int.demo.wsr , Next:
```
![image](/screenshots/srv_dns_manager4.png)
```
21) Next:
```
![image](/screenshots/srv_dns_manager5.png)
```
22) Next:
```
![image](/screenshots/srv_dns_manager6.png)
```
23) Next:
```
![images](/screenshots/srv_dns_manager7.png)
```
24) ПКМ по Forward Reverse Lookup Zones, New Zone..
```
![image](/screenshots/srv_dns_manager8.png)
```
25) Next:
```
![image](/screenshots/srv_dns_manager9.png)
```
26) Next: 
```
![image](/screenshots/srv_dns_manager10.png)
```
27) Next:
```
![image](/screenshots/srv_dns_manager11.png)
```
28) Вводим адерс и нажимаем Next:
```
![image](/screenshots/srv_dns_manager12.png)
```
29) Next:
```
![image](/screenshots/srv_dns_manager13.png)
```
30) Next:
```
![iamge](/screenshots/srv_dns_manager14.png)
```
31) Finish:
```
![iamge](/screenshots/srv_dns_manager15.png)
```
32) ПКМ по Forward Reverse Lookup Zones, New Zone..
```
![image](/screenshots/srv_dns_manager16.png)
```
33) Next:
```
![image](/screenshots/srv_dns_manager9.png)
```
34) Next:
```
![image](/screenshots/srv_dns_manager10.png)
```
35) Next:
```
![image](/screenshots/srv_dns_manager11.png)
```
36) Next:
```
![image](/screenshots/srv_dns_manager17.png)
```
37) Next:
```
![image](/screenshots/srv_dns_manager18.png)
```
38) Next
```
![image](/screenshots/srv_dns_manager14.png)
```
39) Finish:
```
![image](/screenshots/srv_dns_manager19.png)
```
40) ПКМ по int.demo.wsr и создаём Host`ы и Alias`ы как указано нижe:
!!!(не забываем прожимать галочку на пункте - Create associated pointer (PTR) record)!!!
```
![image](/screenshots/srv_dns_newhost.png)

![image](/screenshots/srv_dns_manager20.png)
```
41) Перезагружаем VM
```
2. "Проверка DNS"
```
Вы должны ping`овать WEB-L и WEB-R по доменному имени SRV
```
![image](/screenshots/web-l_dns_ping.png)
![image](/screenshots/web-r_dns_ping.png)

### 3. Выполните настройку первого уровня системы синхронизации времени:
#### ISP NTP
```
1)root@ISP:~# apt install chrony -y
   2)root@ISP:~# nano /etc/chrony/chrony.conf
      3) Добавляем строчки:
```
```
local stratum 4  
allow 4.4.4.0/24
allow 3.3.3.0/24
```
![image](/screenshots/isp_chrony.png)
```
4) Ctrl + X
   5) Ctrl + Y
      6) Enter
         7)root@ISP:~# systemctl restart chronyd 
            8)root@ISP:~# reboot
```
### 4. Выполните конфигурацию службы второго уровня времени на SRV

#### CLI NTP
"Настройка NTP":
```
1) ПКМ по плитке времени:
```
![image](/screenshots/cli_ntp1.png)
```
2) Нажимаем на Adjust data/time:
```
![image](/screenshots/cli_ntp2.png)
```
3) Нажимаем на Add clocks for different time zones:
```
![image](/screenshots/cli_ntp3.png)
```
3) Нажимаем на Internet Time, Change Settings... :
```
![image](/screenshots/cli_ntp4.png)
```
4) Вводим адрес NTP сервера, нажимаем на Update now и ждём синхронизации:
```
![image](/screenshots/cli_ntp5.png)

#### SRV NTP
1. "Настройка Firewall`a для NTP"
```
1) ЛКМ по Win находим приложение Windows Defender Firewall:
```
![image](/screenshots/srv_win_def.png)
```
2) Нажимаем на Advanced settings : 
```
![image](/screenshots/srv_def.png)
```
3) Выбираем Inbound Rules, и добавляем новое правило:
```
![image](/screenshots/srv_firewall1.png)
```
4) Указываем правило для порта:
```
![image](/screenshots/srv_firewall2.png)
```
5) Указываем протокол и порт:
```
![image](/screenshots/srv_firewall3.png)
```
6) Разрешаем подключение:
```
![image](/screenshots/srv_firewall4.png)
```
7) Применяем правило к:
```
![image](/screenshots/srv_firewall5.png)
```
8) Выдаем имя и завершаем добавление правила:
```
![image](/screenshots/srv_firewall6.png)


2. "Настройка NTP":
```
1) ПКМ по плитке времени:
```
![image](/screenshots/srv_ntp.png)
```
2) Нажимаем на Adjust data/time:
```
![image](/screenshots/srv_ntp1.png)
```
3) Нажимем на Add clocks for different time zones:
```
![image](/screenshots/srv_ntp2.png)
```
3) Нажимаем на Internet Time, Change Settings... :
```
![image](/screenshots/srv_ntp3.png)
```
4) Вводим адрес NTP сервера, нажимаем на Update now и ждём синхронизации:
```
![image](/screenshots/srv_ntp4.png)

#### RTR-L NTP
```
RTR-L(config)#ip domain name int.demo.wsr
RTR-L(config)#ip name-server 192.168.100.200
RTR-L(config)#ntp server chrony.int.demo.wsr
```
#### RTR-R NTP
```
RTR-R(config)#ip domain name int.demo.wsr
RTR-R(config)#ip name-server 192.168.100.200
RTR-R(config)#ntp server chrony.int.demo.wsr
```
#### WEB-L NTP
```
1)root@WEB-L:~# apt install chrony -y
   2)root@WEB-L:~# nano /etc/chrony/chrony.conf
      3) Изменяем строчку:
```
```
pool chrony.int.demo.wsr iburst
```

![image](/screenshots/web-l_ntp1.png)
```
4) Ctrl + x
 5) Ctrl + Y
   6) Enter
      7)root@WEB-L:~# systemctl restart chrony
         8)root@WEB-L:~# reboot
```
#### WEB-R NTP
```
1)root@WEB-R:~# apt install chrony -y
   2)root@WEB-R:~# nano /etc/chrony/chrony.conf
      3) Изменяем строчку:
```
```
pool chrony.int.demo.wsr iburst
```
![image](/screenshots/web-l_ntp1.png)
```
4) Ctrl + x
 5) Ctrl + Y
   6) Enter
      7)root@WEB-R:~# systemctl restart chrony
         8)root@WEB-R:~# reboot
```

3. "Проверка NTP" 
#### CLI NTP
```
1) ПКМ по плитке времени:
```
![image](/screenshots/cli_ntp1.png)
```
2) Нажимаем на Adjust data/time:
```
![image](/screenshots/cli_ntp2.png)
```
3) Нажимаем  на Add clocks for different time zones:
```
![image](/screenshots/cli_ntp3.png)
```
3) Нажимаем на Internet Time, Change Settings... :
```
![image](/screenshots/cli_ntp4.png)
```
4) Нажимаем на Update now и ждём синхронизации:
```
![image](/screenshots/cli_ntp6.png)

#### SRV NTP
```
1) ПКМ по плитке времени:
```
![image](/screenshots/srv_ntp.png)
```
2) Нажимаем на Adjust data/time:
```
![image](/screenshots/srv_ntp1.png)
```
3) Нажимем на Add clocks for different time zones:
```
![image](/screenshots/srv_ntp2.png)
```
3) Нажимаем на Internet Time, Change Settings... :
```
![image](/screenshots/srv_ntp3.png)
```
4) Нажимаем на Update now и ждём синхронизации:
```
![image](/screenshots/srv_ntp5.png)

#### WEB-L
```
1)root@WEB-L:~# timedatectl
   2) Проверяем что бы пункты были синхронизированы:
```
![image](/screenshots/web-l_ntp.png)
```
3)root@WEB-L:~# chronyc sources
   4) Проверяем что бы был ресурс синхронизации:
```
![image](/screenshots/web-l_ntp2.png)
#### WEB-R
```
1)root@WEB-R:~# timedatectl
   2) Проверяем что бы пункты были синхронизированы:
```
![image](/screenshots/web-r_ntp.png)
```
3)root@WEB-R:~# chronyc sources
   4) Проверяем что бы был ресурс синхронизации:
```
![image](/screenshots/web-r_ntp1.png)

### 5. Реализуйте файловый SMB-сервер на базе SRV
#### SRV RAID
```
1) Пкм по WIN, находим и нажимаем Disk Management:
```
![image](/screenshots/srv_raid.png)
```
2) ПКМ по Disk 0, Offline :
```
![image](/screenshots/srv_raid1.png)
```
3) ПКМ по Disk 1, Offline :
```
![image](/screenshots/srv_raid2.png)
```
4) ПКМ по Disk 0? Initialize Disk :
```
![image](/screenshots/srv_raid3.png)
```
5) Выбираем оба диска 
```
![image](/screenshots/srv_raid4.png)
```
6) ПКМ по Disk 1, New mirrored Volume... :
```
![image](/screenshots/srv_raid5.png)
```
7) Next:
```
![image](/screenshots/srv_raid6.png)
```
8) Добавляем второй диск
```
![image](/screenshots/srv_raid7.png)
```
9) Next:
```
![image](/screenshots/srv_raid8.png)
```
10) Выдаём букву диску 
```
![image](/screenshots/srv_raid9.png)
```
11) Выдаём имя RAID:
```
![image](/screenshots/srv_raid10.png)
```
12) Finish:
```
![image](/screenshots/srv_raid11.png)
```
13) Yes:
```
![image](/screenshots/srv_raid12.png)

#### SRV SMB
1. "Установка служб SMB"
```
1) Server Manager > Dashboard ЛКМ по Manage:
```
![image](/screenshots/srv_smb1.png)
```
2) Add Roles and Features
```
![image](/screenshots/srv_smb2.png)
```
3) Next:
```
![image](/screenshots/srv_addroles.png)
```
4) Next:
```
![image](/screenshots/srv_addroles2.png)
```
5) Next:
```
![image](/screenshots/srv_addroles3.png)
```
6) Добавляем сервис File and iSCSI Services:
```
![image](/screenshots/srv_smb3.png)
```
7) Next:
```
![image](/screenshots/srv_smb4.png)
```
8) Install:
```
![image](/screenshots/srv_smb5.png)
```
9) Close:
```
![image](/screenshots/srv_smb6.png)

2. "Настройка SMB"
```
1) Нажимаем на File and Storage Services
```
![image](/screenshots/srv_smb7.png)
```
2) Нажимаем на Shares:
```
![image](/screenshots/srv_smb8.png)
```
3) Нажимаем на To create a file share, start the New Share Wizard :
```
![image](/screenshots/srv_smb9.png)
```
4) Выбираем SMB Share - Quick:
```
![image](/screenshots/srv_smb10.png)
```
5) Выбираем RAID, создаём там папку с именем storage:
```
![image](/screenshots/srv_smb12.png)
```
6) Next:
```
![image](/screenshots/srv_smb13.png)
```
7) Выдаём имя SMB :
```
![image](/screenshots/srv_smb14.png)
```
8) Next:
```
![image](/screenshots/srv_smb15.png)
```
9) Нажимаем на Customize permissions... :
```
![image](/screenshots/srv_smb16.png)
```
10) Нажимаем на Share:
```
![image](/screenshots/srv_smb17.png)
```
11) Выбираем и изменяем правила:
```
![image](/screenshots/srv_smb18.png)
```
12) Выдаём Full Control:
```
![image](/screenshots/srv_smb19.png)
```
13) Apply и OK:
```
![image](/screenshots/srv_smb20.png)
```
14) Next:
```
![image](/screenshots/srv_smb21.png)
```
15) Create:
```
![image](/screenshots/srv_smb22.png)
```
16) Close:
```
![image](/screenshots/srv_smb23.png)

### 6. Сервера WEB-L и WEB-R должны использовать службу, настроенную на SRV, для обмена файлами между собой:
#### WEB-L SMB
1. "Настройка SMB"
```
1)root@WEB-L:~# apt install -y cifs-utils
   2)root@WEB-L:~# nano /root/.smbclient
      3) Добавляем строчки:
```
![image](/screenshots/web-l_sbm1.png)
```
4) Ctrl + X
   5) Ctrl + Y
      6) Enter
         7)root@WEB-L:~# nano /etc/fstab
            8) Добавляем строчку:
```
```
//srv.int.demo.wsr/smb /opt/share cifs user,rw,_netdev,credentials=/root/.smbclient 0 0
```
![image](/screenshots/web-l_sbm2.png)
```
9) Ctrl + X
   10) Ctrl + Y
      11) Enter
         12)root@WEB-L:~# mkdir /opt/share
            13)root@WEB-L:~# mount -a
```
2. "Проверка SMB"
```
1)root@WEB-L:~# cd /opt/share
   2)root@WEB-L:~# mkdir WEB-L
      3) Проверить созданную папку в SMB через SRV:
```
![image](/screenshots/srv_smb_web-l.png)
#### WEB-R SMB
```
1)root@WEB-R:~# apt install -y cifs-utils
   2)root@WEB-R:~# nano /root/.smbclient
      3) Добавляем строчки:
```
![image](/screenshots/web-l_sbm1.png)
```
4) Ctrl + X
   5) Ctrl + Y
      6) Enter
         7)root@WEB-R:~# nano /etc/fstab
            8) Добавляем строчку:
```
```
//srv.int.demo.wsr/smb /opt/share cifs user,rw,_netdev,credentials=/root/.smbclient 0 0
```
![image](/screenshots/web-l_sbm2.png)
```
9) Ctrl + X
   10) Ctrl + Y
      11) Enter
         12)root@WEB-R:~# mkdir /opt/share
            13)root@WEB-R:~# mount -a
```
2. "Проверка SMB"
```
1)root@WEB-R:~# cd /opt/share
   2)root@WEB-R:~# mkdir WEB-R
      3) Проверить созданную папку в SMB через SRV:
```
![image](/screenshots/srv_smb_web-r.png)

### 7. Выполните настройку центра сертификации на базе SRV:
#### SRV ADCS

1. "Установка и настройка служб ADCS "
```
1) Server Manager > Dashboard ЛКМ по Manage:
```
![image](/screenshots/srv_smb1.png)
```
2) Add Roles and Features
```
![image](/screenshots/srv_smb2.png)
```
3) Next:
```
![image](/screenshots/srv_addroles.png)
```
4) Next:
```
![image](/screenshots/srv_addroles2.png)
```
5) Next:
```
![image](/screenshots/srv_addroles3.png)
```
6) Добавляем сервис Active Directory Certificate Services :
```
![image](/screenshots/srv_adcs1.png)
```
7) Add Features :
```
![image](/screenshots/srv_adcs2.png)
```
8) 
```
![image](/screenshots/srv_adcs3.png)
```
9) Добавляем сервис Web Server (IIS) :
```
![image](/screenshots/srv_adcs4.png)
```
10) Next:
```
![image](/screenshots/srv_adcs5.png)
```
11) Next:
```
![image](/screenshots/srv_adcs6.png)
```
12) Next:
```
![image](/screenshots/srv_adcs7.png)
```
13) Прожимаем галочки на пунктах Certification Authority и Certification Authority Web Enrollment - Next:
```
![image](/screenshots/srv_adcs8.png)
```
14) Add Features:
```
![image](/screenshots/srv_adcs9.png)
```
15) Next:
```
![image](/screenshots/srv_adcs10.png)
```
16) Next:
```
![image](/screenshots/srv_adcs11.png)
```
17) Next:
```
![image](/screenshots/srv_adcs12.png)
```
18) Install:
```
![image](/screenshots/srv_adcs13.png)
```
19) Close:
```
![image](/screenshots/srv_adcs14.png)
```
20) ЛКМ по значку Notifications 
```
![image](/screenshots/srv_adcs15.png)
```
21) Configure Active Directory Certificate Services on the destination server:
```
![image](/screenshots/srv_adcs16.png)
```
22) Next:
```
![image](/screenshots/srv_adcs17.png)
```
23) Прожимаем галочки на пунктах Certification Authority и Certification Authority Web Enrollment - Next:
```
![image](/screenshots/srv_adcs18.png)
```
24) Next:
```
![image](/screenshots/srv_adcs19.png)
```
25) Root CA - Next:
```
![image](/screenshots/srv_adcs20.png)
```
26) Create a new private key - Next:
```
![image](/screenshots/srv_adcs21.png)
```
27) Next:
```
![image](/screenshots/srv_adcs22.png)
```
28) Выдаём имя центру сертификации Demo.wsr - Next
```
![image](/screenshots/srv_adcs23.png)
```
29) Next:
```
![image](/screenshots/srv_adcs24.png)
```
30) Next:
```
![image](/screenshots/srv_adcs25.png)
```
31) Configure:
```
![image](/screenshots/srv_adcs26.png)
```
32) Close:
```
![image](/screenshots/srv_adcs27.png)


2. "Настройка Web Server`a"
```
1) ЛКМ по плитке Tools:
```
![image](/screenshots/srv_web1.png)
```
2) Internet Information Services (IIS) Manager:
```
![image](/screenshots/srv_web2.png)
```
3) Start Page - SRV (Srv\Administrator) - Sites - Default Web Site - Bindings
```
![image](/screenshots/srv_web3.png)
```
4) Add... :
```
![iamge](/screenshots/srv_web4.png)
```
5) Изменяем и добавляем значения как указано ниже - OK:
```
![image](/screenshots/srv_web5.png)
```
6) Удаляем старый http сайт:
```
![image](/screenshots/srv_web6.png)
```
7)  Yes:
```
![image](/screenshots/srv_web7.png)
```
8) Close:
```
![image](/screenshots/srv_web8.png)
```
9) Restart:
```
![image](/screenshots/srv_web9.png)

## Инфраструктура веб-приложения.
Данный блок подразумевает установку и настройку доступа к веб-приложению, выполненному в формате контейнера Docker
1. Образ Docker (содержащий веб-приложение) расположен на ISO-образе дополнительных материалов;
   - Выполните установку приложения AppDocker0;
2. Пакеты для установки Docker расположены на дополнительном ISO-образе;
3. Инструкция по работе с приложением расположена на дополнительном ISO-образе;
4. Необходимо реализовать следующую инфраструктуру приложения.
   - Клиентом приложения является CLI (браузер Edge);
   - Хостинг приложения осуществляется на ВМ WEB-L и WEB-R;
   - Доступ к приложению осуществляется по DNS-имени www.demo.wsr;
     - Имя должно разрешаться во “внешние” адреса ВМ управления трафиком в обоих регионах;
     - При необходимости, для доступа к приложению допускается реализовать реверс-прокси или трансляцию портов;
   - Доступ к приложению должен быть защищен с применением технологии TLS;
     - Необходимо обеспечить корректное доверие сертификату сайта, без применения “исключений” и подобных механизмов;
   - Незащищенное соединение должно переводится на защищенный канал автоматически;
5. Необходимо обеспечить отказоустойчивость приложения;
   - Сайт должен продолжать обслуживание (с задержкой не более 25 секунд) в следующих сценариях:
     - Отказ одной из ВМ Web
     - Отказ одной из ВМ управления трафиком. 

### 1. Образ Docker (содержащий веб-приложение) расположен на ISO-образе дополнительных материалов;

```
ESXI:
```
![image](/screenshots/esxi_docker_iso.png)

```
VMware Workstation:
```
![image](/screenshots/vmware_docker_iso.png)

### 2. Пакеты для установки Docker расположены на дополнительном ISO-образе;

<span style="color:red"> Не забываем добавлять ISO образ Docker`a к VM WEB-L и WEB-R </span>

#### WEB-L Docker

1. "Установка Docker и поднятие контейнера"
```
1) root@WEB-L:~# apt-cdrom add
 2) root@WEB-L:~# apt install docker-ce -y
   3) root@WEB-L:~# systemctl start docker
      4) root@WEB-L:~# systemctl enable docker
         5) root@WEB-L:~#mkdir /mnt/app
            6) root@WEB-L:~# mount /dev/sr1 /mnt/app
               7) root@WEB-L:~# docker load < /mnt/app/app.tar
                  8) Проверяем репозиторий командой docker images :
```
![image](/screenshots/docker_images1(web-l).png)
```
9) Если появился, то поднимаем контейнер:
   10) root@WEB-L:~# docker run --name app  -p 8080:80 -d app
```
2. "Проверка работы контейнера"
```
root@WEB-L:~# docker ps
```
![image](/screenshots/docker_ps1(web-l).png)

3. "Установка правила на автоподнятие контейнера, после рестарта системы"
```
1) root@WEB-L:~# docker update --restart=always <id контейнера> :
```
![image](/screenshots/docker_auto(web-l).png)
```
2) root@WEB-L:~# reboot
   3) Проверяем автоподнятие:
```
![image](/screenshots/docker_end(web-l).png)

#### WEB-R Docker

1. "Установка Docker и поднятие контейнера"

```
1) root@WEB-R:~# apt-cdrom add
 2) root@WEB-R:~# apt install docker-ce -y
   3) root@WEB-R:~# systemctl start docker
      4) root@WEB-R:~# systemctl enable docker
         5) root@WEB-R:~#mkdir /mnt/app
            6) root@WEB-R:~# mount /dev/sr1 /mnt/app
               7) root@WEB-R:~# docker load < /mnt/app/app.tar
                  8) Проверяем репозиторий командой docker images :
```
![image](/screenshots/docker_images1(web-r).png)
```
9) Если появился, то поднимаем контейнер:
   10) root@WEB-R:~# docker run --name app  -p 8080:80 -d app
```

2. "Проверка работы контейнера"

```
root@WEB-R:~# docker ps
```
![image](/screenshots/docker_ps1(web-r).png)

3. "Установка правила на автоподнятие контейнера, после рестарта системы"

```
1) root@WEB-R:~# docker update --restart=always <id контейнера> :
```
![image](/screenshots/docker_auto(web-r).png)
```
2) root@WEB-L:~# reboot
   3) Проверяем автоподнятие:
```
![image](/screenshots/docker_end(web-r).png)

#### RTR-L Docker  
1) Отключение http сервера 
```
RTR-L(config)#no ip http server
RTR-L(config)#no ip http secure-server
RTR-L(config)#wr
RTR-L(config)#reload
```
2) Добавляем правила NAT 
```
RTR-L(config)#ip nat inside source static tcp 192.168.100.100 80 4.4.4.100 80 
RTR-L(config)#ip nat inside source static tcp 192.168.100.100 443 4.4.4.100 443 
```

#### RTR-R Docker 
1) Отключение http сервера 
```
RTR-R(config)#no ip http server
RTR-R(config)#no ip http secure-server
RTR-R(config)#wr
RTR-R(config)#reload
```
2) Добавляем правила NAT 
```
RTR-R(config)#ip nat inside source static tcp 172.16.100.100 80 5.5.5.100 80  
RTR-R(config)#ip nat insid source static tcp 172.16.100.100 443 5.5.5.100 443 
```

### 3. Инструкция по работе с приложением расположена на дополнительном ISO-образе;
В данной момент инструкции к приложению нет, приложением является одностраничный  сайт

### 4. Необходимо реализовать следующую инфраструктуру приложения

#### SRV SSL
 "Создание сертификатов"

```
1) В браузере вводим ссылку https://localhost/certsrv/ :
```
![image](/screenshots/srv_web11.png)
```
2) OK :
```
![image](/screenshots/srv_web12.png)
```
3) Request a certificate: 
```
![image](/screenshots/srv_web13.png)
```
4) advanced certificate request:
```
![image](/screenshots/srv_se1.png)
```
5) Create and sumbit a request to this CA:
```
![image](/screenshots/srv_se2.png)
```
6) Yes:
```
![image](/screenshots/srv_se3.png)
```
7) Вводим следующие значения и прожимаем галочки:
```
![image](/screenshots/srv_se4.png)
```
8) OK :
```
![image](/screenshots/srv_se5.png)
```
9) Win - Certification Authority :
```
![image](/screenshots/srv_se6.png)
```
10) Demo.wsr - Pending Requests - All Tasks - Issue : 
```
![image](/screenshots/srv_se7.png)
```
11) View the status of a pending certificate request : 
```
![image](/screenshots/srv_se8.png)
```
12) Server Authentication Certificate (...):
```
![image](/screenshots/srv_se9.png)
```
13) Yes:
```
![image](/screenshots/srv_se10.png)
```
14) Install this certificate:
```
![image](/screenshots/srv_se11.png)
```
15) Home:
```
![image](/screenshots/srv_se12.png)
```
16) Win + R
   17) вводим mmc 
      18) File - Add/Remove Snap-in... :
```
![image](/screenshots/srv_se13.png)
```
17) Certificates - Add>
```
![image](/screenshots/srv_se14.png)
```
18) My user account - Finish : 
```
![image](/screenshots/srv_se15.png)
```
19) Certificates - Add>
```
![image](/screenshots/srv_se16.png)
```
20) Computer account - Next>
```
![image](/screenshots/srv_se17.png)
```
21) Finish:
```
![image](/screenshots/srv_se18.png)
```
22) OK
```
![image](/screenshots/srv_se19.png)

```
23) Certificates - Current User - Personal - Certificates - www.demo.wsr - All Tasks - Export... :
```
![image](/screenshots/srv_se20.png)
```
24) Next:
```
![image](/screenshots/srv_se21.png)
```
25) Yes, export the private key - Next:
```
![image](/screenshots/srv_se29.png)
```
26) Next:
```
![image](/screenshots/srv_se30.png)
```
27) Ставим галочку на пункте Password вводим любой пароль - Next:
```
![image](/screenshots/srv_se31.png)
```
28) Browse... :
```
![image](/screenshots/srv_se24.png)
```
29) Создаём зашифрованный сертификат www.pfx в папке R:\storage\
```
![image](/screenshots/srv_se32.png)
```
30) Next:
```
![image](/screenshots/srv_se33.png)
```
31) Next:
```
![image](/screenshots/srv_se34.png)
```
32) OK:
```
![image](/screenshots/srv_se35.png)
```
33) Certificates (Local C)
```
![image](/screenshots/srv_se36.png)
```
34) Certificates (Local Computer) - Personal - Certificates - All Tasks - Export... :
```
![image](/screenshots/srv_se37.png)
```
35) Next:
```
![image](/screenshots/srv_se21.png)
```
36) Next:
```
![image](/screenshots/srv_se22.png)
```
37) Next:
```
![image](/screenshots/srv_se23.png)
```
38) Browse... :
```
![image](/screenshots/srv_se24.png)
```
39) Создаём сертификат CLI.cer в C:\Users\Administrator\Desktop\ :
```
![image](/screenshots/srv_se38.png)
```
40) Next:
```
![image](/screenshots/srv_se39.png)
```
41) Finish:
```
![image](/screenshots/srv_se40.png)
```
42 ) OK:
```
![image](/screenshots/srv_se36.png)

4. "Проверка ранее созданных сертификатов"

![image](/screenshots/srv_se41.png)

### 5. Необходимо обеспечить отказоустойчивость приложения;

#### WEB-L SSL и NGINX
```
1) root@WEB-L:~# apt install nginx -y 
   2) root@WEB-L:~# cd /opt/share
      3) root@WEB-L:/opt/share# openssl pkcs12 -nodes -nocerts -in www.pfx -out www.key
         4) root@WEB-L:/opt/share# openssl pkcs12 -nodes -nokeys -in www.pfx -out www.cer
            5) root@WEB-L:/opt/share# cp /opt/share/www.key /etc/nginx/www.key
               6) root@WEB-L:/opt/share# cp /opt/share/www.cer /etc/nginx/www.cer
                  7) root@WEB-L:/opt/share# cd /etc/nginx/
                     8) root@WEB-L:/etc/nginx# openssl rsa -in www.key -out www.key
                        9) root@WEB-L:/etc/nginx# nano /etc/nginx/sites-available/default
                           10) Изменяем структуру файла:
```
```
upstream backend {
      server 192.168.100.100:8080 fail_timeout=25;
      server 172.16.100. 100:8080 fail_timeout=25;
}

server {
   listen 80;
   server_name www.demo.wsr;
   return 301 https://www.demo.wsr;
}

server {
   listen 443 ssl;
   server_name www.demo.wsr;
   ss1_certificate /etc/nginx/www.cer;
   ss1_certificate_key /etc/nginx/www.key;

   location / {
      proxy_pass http://backend;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      }
}
```
![image](/screenshots/web_l_back.png)
```
11) root@WEB-L:/opt/share# systemctl reload nginx
```
#### WEB-R SSL и NGINX
```
1) root@WEB-R:~# apt install nginx -y 
   2) root@WEB-R:~# cd /opt/share
      3) root@WEB-R:/opt/share# cp /opt/share/www.key /etc/nginx/www.key
         4) root@WEB-R:/opt/share# cp /opt/share/www.cer /etc/nginx/www.cer
            5) root@WEB-R:/etc/nginx# openssl rsa -in www.key -out www.key
               6) root@WEB-R:/etc/nginx# nano /etc/nginx/sites-available/default
                  7) Добавляем и изменяем структуру файла:
```
```
upstream backend {
      server 192.168.100.100:8080 fail_timeout=25;
      server 172.16.100. 100:8080 fail_timeout=25;
}

server {
   listen 80;
   server_name www.demo.wsr;
   return 301 https://www.demo.wsr;
}

server {
   listen 443 ssl;
   server_name www.demo.wsr;
   ss1_certificate /etc/nginx/www.cer;
   ss1_certificate_key /etc/nginx/www.key;

   location / {
      proxy_pass http://backend;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      }
}
```
![image](/screenshots/web_l_back.png)
```
8) root@WEB-R:/opt/share# systemctl reload nginx
```


#### 6. Установка сертификатов и проверка приложения

1. "Перемещение сертификатов из share на CLI"
##### SRV SSL
```
1) Переносим сертификат CLI.cer в папку share : 
```
![image](/screenshots/srv_ssl1.png)

##### CLI SSH-SSL
```
1) WIN + R 
   2) Вводим cmd 
      3) Прописываем  команду scp -P 2222 root@4.4.4.100:/opt/share/CLI.cer C:\Users\CLI\Desktop\
         4) Вводим пароль от WEB-L :
```
![image](/screenshots/cli_ssh_ssl3.png)

2. "Установка сертификатов"
#### CLI Cert
```
1) ПКМ по сертификату CLI.cer - install certificate : 
```
![image](/screenshots/cli_inst_ssl1.png)
```
2) Local Machine - Next :
```
![image](/screenshots/cli_inst_ssl2.png)
```
3) Place all certificates in the following store - Browse... :
```
![image](/screenshots/cli_inst_ssl3.png)
```
4) Trusted Root Certification Authorities:
```
![image](/screenshots/cli_inst_ssl4.png)
```
5) Next:
```
![image](/screenshots/cli_inst_ssl5.png)
```
6) Finish:
```
![image](/screenshots/cli_inst_ssl6.png)
```
7) OK:
```
![image](/screenshots/cli_inst_ssl7.png)

3. "Проверка работы приложения"

```
На данном этапе вам следует проверить версию стенда (можно узнать при подключении по SSH к стенду)

Актуальные версии Gosha.ova (на 10.02.2023): 
        1) Для домашнего использования - v.1.7
        2) Для учебных учреждений - v1.8.1
```
##### Проверка для версии 1.8.x
```
1) Запускаем браузер Edge 
   2) Вводим и переходим по ссылке http://www.demo.wsr/
```
![image](/screenshots/cli_end1.png)
```
3) Должен отработать редирект соединения с http на https, и открыться приложение:
```
![image](/screenshots/cli_end2.png)

##### Проверка для версии 1.7.x
```
1) Запускаем браузер IE(Internet Explorer) 
   2) Вводим и переходим по ссылке http://www.demo.wsr/
```
![image](/screenshots/cli_end3.png)
```
3) Должен отработать редирект соединения с http на https, и открыться приложение:
```
![image](/screenshots/cli_end4.png)
