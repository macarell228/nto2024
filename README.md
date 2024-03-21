# nto2024
# Web1

…

# Web2

1) Декомпилировали .jar файл при помощи jd-gui утилиты linux

2) Поняли, что используется фреймворк Spring

3) Погуглили его уязвимости, нашли на [сайте](https://www.veracode.com/blog/secure-development/spring-view-manipulation-vulnerability) точно такой же обработчик, как в задании и поняли, что идем в правильном направлении (уязвимость spring view)

4) На этом же сайте был приложен эксплоит уязвимости, мы поменяли в нем `"touch execution"` на `"cat flag"`, потому что что делает первое мы не знаем, а второе выводит флаг

Ссылка: [`http://192.168.12.13:809](http://192.168.12.13:8090/)0/doc/__${T(java.lang.Runtime).getRuntime().exec("cat flag")}__::..x`

# Web3

1) По конфигу прокси поняли, что haproxy запрещает доступ ко всем запросам, которые начинаются с `/flag`, попытались это обойти при помощи уязвимости Haproxy с переполнением Content-Length (request smuggling), но в итоге просто ввели [`http://192.168.12.11:8001/](http://192.168.12.11:8001/)/flag` и получили доступ

2) В параметре name передают что-то, приложение банит какие-то особенные символы, в следсвтие чего мы подумали, что надо туда ввести что-нибудь, что выкинет нам содержимое flag.txt (не зря же он лежит в папке с приложением)

3) Поняли, что в коде имя на страницу вставляется при помощи движка у flask, то есть благодаря Jinja2, [погуглили уязвимости](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection/jinja2-ssti) и нашли эксплоит без запрещенных слов

Ссылка:  [`http://192.168.12.11:8001//flag?name={{ get_flashed_messages.__globals__.__builtins__.open("flag.txt").read() }](http://192.168.12.11:8001//flag?name=%7B%7B%20get_flashed_messages.__globals__.__builtins__.open(%22flag.txt%22).read()%20%7D)}`
