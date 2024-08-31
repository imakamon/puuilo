# puuilo
#### Интернет для всех, даром, и пусть никто не уйдет обиженным!
Проект позволит создать из бытового маршрутизатора устройство, которое можно передать технически не грамотному человеку, он его подключит в любую локальную сеть и получит доступ в Интернет без ограничений и без торможения YouTube. Будут работать и сайты типа Госуслуг, куда можно зайти только с российских IP и сайты типа Instagram, куда нельзя зайти с российских IP. Воткнул/подключился к появившемуся WiFi/живёшь без ограничений. И на телевизоре и на плейстейшн и на утюге. Дополнительных платежей не нужно, только купить маршрутизатор приблизительно за 1,5 тысяч рублей. Придётся пожить без торрентов, правда если ОЧЕНЬ хочется - решение в разделе "Вопрос - Ответ". 

## Usecase
Допустим у нас есть племянник, который умеет запускать Linux, знает как там вызвать командную строку, знает как подключиться по SSH к нужному ip адресу (этих знаний уже достаточно) и у него есть любимая тётя, которая очень страдает без YouTube и просмотра роликов Майкла Наки. Племянник может потратить 1,5 тысячи рублей и 2 часа, настроить для тёти коробочку, передать её и всё. Тётя подключит Puuilo за 2 минуты,<b> минуты 3 после включения даст ему просто постоять </b>и прийти в себя. Уже через 5 минут тётя будет очень признательна племяннику. 

## Инструкция
Это инструкция для изготовления дешёвого маршрутизатора <b>Puuilo из Mi Router 4C.</b> 
Стек технологий:
1. Маршрутизатор с OpenWRT https://openwrt.org/ - Открытая, бесплатная прошивка для маршрутизаторов. Ценна нам так как мы получаем Linux на маршрутизаторе и можем ставить пакеты для подключения к стойкому к блокировкам VPN и настраивать там много всего.
2. VPN Generator https://t.me/vpngen - Проект общества защиты интернета, позволяющий каждому желающему бесплатно получить свой собственный максимально стойкий к блокировкам VPN сервер. Единственная обязанность за право распоряжаться этим сервером нужно раздать его ещё 4 пользователям, например тёте. Подробности у них в инструкции и документах
3. Вроде всё. Получим свой сервер, настроим ключи для пользователей, настроим маршрутизатор, подключим его к серверу, настроим правила маршрутизации трафика. Я же говорил что справиться по инструкции не сложно.

Начнём. Покупаем роутер Mi Router 4C, модель выбрана за дешевизну. 1,5к рублей доступны даже бедному племяннику. 
Перепрошиваем роутер, ставим туда OpenWRT, для этого понадобится Linux на виртуальной машине или на ноутбуке. Для простоты считаем, что у племянника есть Linux на ноутбуке. Если есть только машина с Windows всё легко решается виртуальными машинами. Посмотрите эту часть в видеоинструкции по прошивке. 
Видеонструкция по прошивке https://www.youtube.com/watch?v=SLbkce-M2nE 
Прошивать будем с помощью https://github.com/acecilia/OpenWRTInvasion там тоже есть инструкция. 

Как обычно, инструкции есть, но я предпочитаю свою, по идее Вам нужна только она и всё. Ну разве что про поднятие виртуалки с Linux я не писал.  Итак, нам нужно прошить OpenWRT на этот маршрутизатор.  
Взяли ноутбук, подключили его к Интернет, запустили терминал.
```
sudo apt update 
sudo apt upgrade 
sudo apt install git python3-pip python3 
git clone https://github.com/acecilia/OpenWRTInvasion.git 
cd OpenWRTInvasion/ 
pip3 install -r requirements.txt
```
Теперь подключаем роутер. WAN порт роутера Puuilo (он синий и немного в стороне) подключаем в домашнюю сеть нынешнюю, просто в один из портов старого маршрутизатора, а в LAN порт Puuilo (их два, они рядом) подключаем ноутбук. Подключаем питание роутера, если на ноутбуке запустится Hotspot Login, не пугаемся. Наша задача увидеть адрес роутера, нашему ноутбуку он выдал адрес по DHCP, но нам нужен адрес именно роутера Puuilo (DHCP сервера). Смотрим это один из тысячи способов, например в консоли пишем ```ip r``` оно напишет одной из первых строчек ```default via <ip address>```  это как раз тот адрес, что нам нужен. Ну или делаем, как предложено в видео - Кликаем в Убунте на значок сети и смотрим свойства. Шлюз по умолчанию и DNS  сервер - адрес маршрутизатора, то что нам нужно. Наш маршрутизатор Puuilo будет для ноутбука и DHCP сервером и DNS сервером и маршрутом по умолчанию, поэтому все эти адреса одинаковые, достаточно увидеть хотя-бы что-то из них. У меня это оказалось 192.168.31.1 Заходим в браузер, вводим URL = IP роутера, у меня 192.168.31.1 Откроется окно настройки маршрутизатора Mi Router 4C,  вводим страну, соглашаемся с лицензией, жмём Try it now,  Задаём пароль WiFi, оставляем галку, что пароль администрирования роутера совпадёт с паролем WiFi, оно пишет Complete, сохраните пароль. Снова в браузере идём на 192.168.31.1 показывает QR код и предлагает ввести пароль. Вводим пароль открывается окно, в адресной строке браузера stok код, может потребуется ввести его позже но скорее всего нет.  
Запускаем в окне терминала  
```python3 remote_command_execution_vulnerability.py```
Вводим ip роутера вводим пароль роутера, программа предлагает выбор из двух опций выбираем 2 – Download  оно что-то делает и говорит что можно подключиться. 
Подключаемся по telnet c логином и паролем root / root: 
в консоли пишем ```telnet 192.168.31.1``` спросит логин, пишем root спросит пароль, пишем root
Теперь нужно скачать образ OpenWRT для нашего роутера. На ноутбуке открываем в браузере https://openwrt.org/toh/xiaomi/xiaomi_mi_router_4c
В разделе Installation находим ссылку на Firmware OpenWRT Install URL, он будет в табличке маленькой. Нажимем на ссылке правую кнопку мышки и жмём “Copy Link”. Теперь ссылка у нас в буфере обмена, возвращаемся в окно telnet 
```
cd /tmp 
wget  <вставляем ссылку из буфера обмена>
```
заменяем https на http, не меняя ссылку. Оно скачает образ в /tmp 
Теперь нужно сделать  
```
mtd -r write <файл образа> OS1  
```
Не копируйте название руками, пара букв, стрелка вверх и там будет единственный файл bin, не забываем OS1 в конце строки добавить. Для текущей версии у меня было  
```
mtd -r write /tmp/openwrt-23.05.4-ramips-mt76x8-xiaomi_mi-router-4c-squashfs-sysupgrade.bin OS1
```
Если образы поменяются, поменяются цифры, но мы же не копируем команду тупо, так что всё норм. 

Начнут мигать w – e, важно не дёргать при этом провода, не отключать питание, просто ждать, потом маршрутизатор перезагрузится. 

Тут, если Вы настраиваете всё не с ноутбука а с виртуальной машины нужно понимать, что дальше нам важно быть подключенным в LAN порт маршрутизатора Puuilo, чтобы он выдал нам IP адрес. Linux дальше нам не обязателен, просто нужна машина с любой ОС, подключенная в LAN порт маршрутизатора, станция с Windows, на которой запускали виртуальную машину вполне подойдет, виртуалка больше не нужна.
Отключаем wan кабель, заходим браузером ноутбука на 192.168.1.1 видим интерфейс Open WRT. Ура, мы его перепрошили, теперь нужно настраивать, но сначала получим ключ от VPN Generator. 

##### Ключ VPN Generator 
Открываем инструкцию https://bit.ly/VPNgenManual  цель - стать бригадиром. Получаем первый ключ. Это бригадирский ключ, ставим на компьютер или телефон Outline, добавляем ключ. Это бригадирское подключение, оно будет нужно Вам всякий раз, когда Вы будете создавать ключи для друзей и знакомых. Прямо сейчас нам нужно создать ключ для нашего будущего маршрутизатора. Включаем этот VPN, заходим в браузере на VPN.works и впредь делаем так всегда, чтобы выписать новый ключ друзьям-товарищам. Делать это придётся, так как если в бригаде меньше 5 активных человек её закрывают и проверяется это ежемесячно. 
Жмём добавить пользователя, копируем ключ. Скопирую его сюда, но не пытайтесь пользоваться им, я поменял ключевые поля немного. 
```
ss://Y2hhY2hhMjAtaWV0Zi1wb2x5MTMwNToyNG1jeFhFV2prTW1uUXJNNEF1NEZER2hmZ2g1NmZnaDU2eWZkZGZnZGYzRG9lSmhiVDd2NVVNb29yR0s3VmhaVVpQYnIzZ2s1TW54cThkamJGTnZwSnoxQjNqZzk2@show.memagic.top:16439/?outline=1&prefix=%16%03%01%00%C2%A8%01%01#085+%D0%9D%D0%B5%D1%83%D0%BB%D0%BE%D0%B2%D0%B8%D0%BC%D1%8B%D0%B9%2B%D0%94%D0%B6%D0%BE
```
Если в дальнейшей настройке будет принимать участие компьютер, с которого Вы выписывали ключи - завершите на нём бригадирскую VPN сессию, чтобы не путаться. 

Теперь пора браться за наш новый маршрутизатор. Придвигаем к себе ноутбук с Linux, который подключен в его LAN порт. 

##### Настройка OpenWRT 
###### Первый вход
Если Вы уверены, что подсеть 192.168.1.0/24 не используется ни у Вас дома ни дома у тёти, то смену подсети на 192.168.4.0/24 можно пропустить. Но если уверенности нет - лучше потратить пару минут. На данный момент WAN кабель нового маршрутизатора отключен, ноутбук остаётся подключённым к его LAN интерфейсу. Загружаем маршрутизатор, на LAN интерфейсах будет работать DHCP, на ноутбуке заходим браузером по адресу 192.168.1.1  логин/пароль = root / root 

Передвинем внутреннюю сеть на 192.168.4.0/24 чтобы она не конфликтовала ни с нашей, ни с тётиной домашней подсетью. Дадим маршрутизатору адрес 192.168.4.1   Удалим IPv6 

Network – Interfaces  
wan6 – Delete\
lan – Edit, IPv4 address = 192.168.4.1 - Save\
Save & Apply – Apply and keep settings 

###### Продолжаем настройку
На ноутбуке обновляем ip адрес 
После этого подключаемся браузером уже на адрес 192.168.4.1 
Подключаем WAN кабель 
На ноутбуке проверяем что появился доступ в интернет - идём на 2ip.ru и видим, что мы пришли туда от нашего провайдера. 

Установим пакеты для подключения к скрытному VPN от VPN Generator и настроим подключение 
Заходим на маршрутизатор (на 192.168.4.1) через SSH 
Ставим пакеты для работы с Shadowsocks 
```
opkg update 
 
opkg install shadowsocks-libev-ss-local shadowsocks-libev-ss-redir shadowsocks-libev-ss-rules shadowsocks-libev-ss-tunnel 
 
opkg install luci-app-shadowsocks-libev
```
Идём в веб интерфейс маршрутизатора, делаем log out и входим туда снова. 

Сверху добавилось меню Services 

Services - shadowsocks-libev 
Закладка Remote Servers 
Import Links – В открывшееся окно вставляем ключ от VPN Generator – Import 
Появилась новая учётная запись для сервера, название генерируется автоматом, у меня это “cfg08769c”  оно вообще нагенерит в нескольких местах похожие имена, их сразу видно. 
Жмём Save 
 
Закладка Local instances 
ss_tunnel.cfg0249c0 - Edit - Снимаем галку с Disable, в Remote server выбирам наш cfg08769c 
Жмём Save 

Настраиваем ss_redir  , давайте, например, править ss_redir.hj 

ss_redir.hj - Edit - Снимаем галку с Disable, в Remote server выбирам наш cfg08769c Жмём Save 
 
Идём в закладку Redir Rules 
Откроется закладка General Settings 
Убираем Disable, намтраиваем наш hj-tcp_and_udp в ss-redir для TCP и UDP 
Local-out default = checkdst 
Extra tcp expression вписываем tcp dport { 80, 443 } 
Extra udp expression вписываем udp dport { 53 - 65535 } 
Жмём Save 

В закладке Destination Settings – Очищаем Dst ip/net forward
Dst default делаем  forward то есть пока всё завернём через прокси.
Жмём Save & Apply  

При желании можно проверить все настройки Shadowsocks в файле /etc/config/shadowsocks-libev
После любых настроек Shadowsocks лучше перезапускать службу из консоли, чтобы изменения точно применились.
```
/etc/init.d/shadowsocks-libev restart
```
Заходим в 2ip.ru и видим что мы пришли из какой-нибудь интересной страны, например “Нидерланды” в ключнице vpn.works наш ключ, выданный маршрутизатору, стал зелёным и его статус “Всё отлично!” с этим нет смысла спорить. Открываем заблокированный в России сайт, например meduza.io

Теперь настроим разные маршруты для заблокированного и не заблокированного трафика. Мы воспользуемся списком novpn проекта Puuilo. Так как нам нужно будет редактировать файлы, а совсем не все легко работают в vi, поставим nano (если кому привычнее без nano – моё увожение, но инструкция для всех и должна быть простой). 
Подключаемся к консоли маршрутизатора 
```
opkg update 
opkg install nano
```
Скачам файл novpn.lst 
```
wget https://raw.githubusercontent.com/imakamon/puuilo/main/novpn.lst -P /etc/shadowsocks-libev/ -q
```
Выберем его в качестве списка адресов, куда трафик должен ходить мимо тоннеля. 
В админке идём Services -  Shadowsocks-libev – Redir rules – Destination settings - Dst ip/net bypass file – Select file - выбираем novpn.lst - Save and Apply 
В консоли перезапускаем тоннель
```
/etc/init.d/shadowsocks-libev restart
```
Заходим на сайт 2ip.ru и снова видим российский адрес - он добавлен для проверок в список сайтов мимо VPN. А вот если зайти на https://www.iplocation.net/  - адрес будет как при заходе из-за границы. Хотя базы  у них с 2ip.ru разные и страна может различаться, у меня один показывал Нидерланды, а второй Великобританию.

Настроим автоматическое обновление списка novpn. Нужно сделать скриптик. Идём в консоль. 
```
nano /etc/shadowsocks-libev/update.sh
```
Пишем скрипт 
```
#!/bin/sh  
wget https://raw.githubusercontent.com/imakamon/puuilo/main/novpn.lst -O /etc/shadowsocks-libev/novpn.lst -q 
/etc/init.d/shadowsocks-libev restart
```
Сохраняем. Жмём <Ctrl>+<X>, <Y> на предложение сохранить. Название файла не меняем жмём <Enter> 

Делаем его исполняемым 
```
chmod +x /etc/shadowsocks-libev/update.sh
```
Ну и, наконец, добавляем в crontab задание - каждые два часа обновлять список адресов для захода мимо VPN.
```
EDITOR=nano crontab -e
```
```
0 */2 * * * /etc/shadowsocks-libev/update
```
Сохраняем. Жмём <Ctrl>+<X>, <Y> на предложение сохранить. <Enter>

Всё, осталось настроить WiFi. 
В админке Network – Wireless - Строка с SSID задисейблена, её нужно настроить и сделать Enable.
Edit, Задаём SSID: ESSID = Puuilo (ну или свой придумайте), 
Закладка  Wireless Security
Encryption = WPA2-PSK/WPA3-SAE Mixed
Key = задаём пароль. Save и Enable. Save & Apply

Если ещё не сделали - задаём пароль на маршрутизатор. System - Administration  

Всё сохраняем.  Готово. Отправляем тёте и ждём от неё фотографий в Инстаграм, как она на даче вырастила укроп. 

## Вопрос - Ответ
<b>Остальные устройства в сети тёти будут работать как прежде?</b> Да, без блокировок будут работать только устройства, подключенные к нашему маршрутизатору.

<b>За счёт чего живёт проект с бесплатным VPN?</b> Это проект Общества защиты Интернета ozi-ru.org  люди борются за отсутствие цензуры в нашем сегменте Интернет. Они сделали проект VPN Generator и проект прям крутой, уважаю! При этом решение бесплатно для пользователей. Не могу сказать, откуда у проекта деньги, но это совершенно не космические средства, сейчас в некоторых  датацентрах цена аренды серверов начинается с 3-5$ в месяц и, думаю, на одном арендованном сервере можно разместить больше одного сервера VPN. Cреди создателей есть весьма известные люди, видимо проект финансируется из источников других фондов, борющихся за демократизацию России, в частности в нём, как я понимаю, принимают участие Леонид Волков и Сергей Бойко из команды ФБК. Поэтому если Вы лично не любите ФБК, я не рекомендовал бы использовать это решение, живите с заблокированным Интернетом, а то получается “Я тёщу ненавижу, но поеду к ней пожрать блины на халяву” это явно не позиция человека с чувством самоуважения. Основатель и директор ОЗИ - Михаил Климарёв, раз уж я пишу какие-то фамилии не упомянуть его было бы несправедливо. Внутреннюю кухня в не знаю, ни с кем не знаком, если упустил что-то важное - без тайного умысла. 

<b>Наверное они могут украсть и продать мои данные?</b> VPN это только дополнительный слой шифрования. Никакой VPN сервис не может видеть ваши банковские пароли, всё это всегда зашифровано сайтом клиент-банка и только он может эти данные расшифровать. VPN сервис только видит, что Вы сейчас пользуетесь сайтом банка. При отсутствии VPN всё это видит Ваш провайдер и перехватывать трафик там намного проще, чем делать для этого VPN сервер, а толку ни там ни там нет, иначе все деньги со всех клиент-банков уже давно были бы украдены так как мошенников или криворуких специалистов в наших государственных, и не только, компаниях пруд пруди и данные из них утекают постоянно, а значит если бы могли - уже бы давно украли. Пишу про это отдельно так как вижу, что нашими государственными органами тратится достаточно много сил, чтобы просто запугать не слишком грамотных людей и представить VPN сервисы каким-то пугающим местом, где нужно передать владельцам сервиса всю информацию о себе и чуть ли не дать им возможность совершать от Вашего имени платежи. Это очевидное враньё и манипуляция. Если даже вдруг VPN сервис злонамеренный и будет даже создан сотрудниками ФСБ - они просто увидят ровно ту же информацию, которую они видят про всех, кто VPN сервисами не пользуется. А хакерам - злодеям куда проще не VPN сервис налаживать, а устроиться работать к провайдеру. Усилий меньше, ещё и зарплата будет капать, а толку ни там ни там нет особого. 

<b>Почему просто не настроить VPN на маршрутизаторе?</b> Обычные маршрутизаторы работают с популярными протоколами VPN. В этом случае провайдеру легко увидеть, что Ваш маршрутизатор сделал куда-то VPN подключение, они не прячутся. Сейчас в России Роскомнадзор имеет возможность просто обрывать такие соединения и регулярно это делает. Так как эта технология нужна очень многим для работы бизнесов они пока не обрубают их надолго и массово, но уже регулярно обрубают их на некоторое время (от часов до дней) для разных протоколов и для разных провайдеров. Поэтому приходится пользоваться скрытными VPN, которые не только шифруют трафик, но для провайдера выглядят, например, как обычный трафик просмотра сайта браузером.

<b>Если VPN Generator так хорош, зачем вся эта морока с маршрутизатором?</b> Обычный домашний маршрутизатор не умеет сам работать со скрытными VPN протоколами. Значит придётся ставить приложение на компьютеры, телефоны и так далее. А, например, на умный телевизор его не поставить. Поэтому вне дома мы будем пользоваться VPN установленном на ноутбуке и телефоне, а в квартире VPN-ом, который будет стоять на маршрутизаторе.

<b> Обещали про торренты сказать! </b> Да, про торренты. Вот самое корректное решение: После всех настроек у нас будет старая сеть, с блокировками и новая сеть Puuilo. В сети Puuilo у нас будут доступны торрент треккеры, например Rutracker.org  идём туда, находим торренты, которые хотим скачать, качаем торрент файлы. Отключаемся от сети Puuilo, переключаемся на старую сеть с блокировками, включаем торрент клиента и спокойно качаем/раздаём сколько угодно свои торренты.

<b>Не открывается клиент-банк банка ****, как быть?</b> Пишите на адрес imakamon@protonmail.com

<b>Как помочь / как угостить автора пивом? </b> Если хотите помочь, пишите на imakamon@protonmail.com, я обращусь к Вам если помощь потребуется. Но достаточно просто распространять свободный интернет в нашей многострадальной стране. Если очень хочется задонатить - задонатьте Обществу Защиты Интернета, или ФБК, или политзаключённым. Ну или просто политзаключённому письмо напишите. https://t.me/politzekam_bot  Я буду Вам заочно благодарен.
