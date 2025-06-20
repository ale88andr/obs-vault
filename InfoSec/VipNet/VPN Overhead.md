
`VPN` подразумевает шифрование трафика и его дальнейшую инкапсуляцию в один из протоколов транспортного уровня с определенным номером порта. Если между партнерами по `VPN` соединению есть межсетевые экраны, то на них нужно сделать дополнительные правила для прохождения шифрованного трафика, т.е. «открыть `VPN` порты».

Если `VPN` протокол проприетарный, то необходимую для правил информацию декларирует производитель. Причем многие производители не регистрируют порты в `IANA` (`Internet Assigned Numbers Authority`), а иногда вообще используют из динамического диапазона (например, `UDP 5577`7 для `ViPNet`). В ряде случаев, порты могут меняться от версии к версии, как у проприетарного протокола `Кода Безопасности`. Там их, кстати, довольно много — 16 штук. 

Со стандартными протоколами все гораздо лучше. Для `SSL/TLS` правила обычно уже есть по умолчанию и в дополнительных действиях нет необходимости.
### Откуда накладные расходы?

Накладные расходы (overhead) в VPN можно условно разделить на две части:

- **Туннелирование трафика** – добавление дополнительных заголовков к пакету, необходимое для сокрытия адресации защищаемых объектов и «проброса» других протоколов,

- **Шифрование** – информация, необходимая для криптографических преобразований.

Величина накладных расходов очень важна, так как увеличение размера пакетов может привести к фрагментации трафика во внешней сети. Фрагментация снижает производительность, а для некоторых сервисов — недопустима в принципе.

Зная `overhead` заранее, можно уменьшить `MTU` в локальной сети или, наоборот, увеличить в канале связи. Это позволит избежать проблем с фрагментацией.

Рассмотрим величину накладных расходов на примере продуктов С-Терра, Континент, ViPNet.

## С-Терра

В продуктах С-Терра используется стандартный `IPSec`, но с ГОСТ алгоритмами. Расчет `overhead` не сложный, можно воспользоваться зарубежными ресурсами (например, у Cisco есть калькулятор), но сделать поправку на российские криптоалгоритмы.

| **Криптографическое преобразование** | **Header** | **Trailer** | **ICV** | **ESP  <br>Итого** |
| ------------------------------------ | ---------- | ----------- | ------- | ------------------ |
| ГОСТ28147 (IMIT)                     | 16         | 2-9         | 4       | 22-29              |
| ГОСТ Р 34.12/13-2015 Кузнечик MGM    | 16         | 2-17        | 12      | 30-45              |
| ГОСТ Р 34.12/13-2015 Магма MGM       | 16         | 2-9         | 8       | 26-33              |

Также стоит отметить, что есть возможность дополнительной инкапсуляции в `http`. Применяется в основном для С-Терра Клиент, чтобы обойти запрет пропуска ESP и UDP500/4500 на промежуточном оборудовании. Дополнительно `+135` байт.

Наиболее популярные случаи:
- `L3 VPN` через интернет (туннельный режим, ГОСТ28147 IMIT, NAT) — `50-57` байт.
- `L2 VPN 10G` через выделенный канал (туннельный режим, ГОСТ Р 34.12/13-2015 Кузнечик MGM, без NAT) — `86-101` байт.
## Континент

Информация про `overhead` в документации Континента весьма скудная, декларируются следующие цифры:
- L3 `52` байта,
- L2 `56` байт.

Сначала подумал, что это ошибка — разница минимальная, всего 4 байта. Но есть [запись вебинара](https://www.youtube.com/watch?t=481&v=u5CDJXc5E7E&feature=youtu.be), в котором информация подтверждается и раскрыты подробности по размеру конкретных заголовков.

| **Режим** | **Заголовок IP** | **Заголовок UDP** | **Криптозаголовок** | **Данные**        |
| --------- | ---------------- | ----------------- | ------------------- | ----------------- |
| L3        | 20               | 8                 | 24                  | Шифрованный пакет |
| L2        | 20               | 8                 | 28                  | Шифрованный фрейм |

На сетевом уровне действительно получается 52 байта.

А вот с канальным есть нюансы. Обратите внимание, что в поле данных находится не пакет, а фрейм целиком. Соответственно, для пользователя дополнительные накладные расходы составят как минимум 14 байт исходного ethernet заголовка. К другим «допам» еще вернемся в конце заметки.

Итого для L2 получается 70 байт.

Континент АП (клиент для пользовательских устройств) — отдельная история, используется инкапсуляция в `TCP` (порт `443`), `overhead` составляет до `49` байт. 
## ViPNet

Подробности по работе протокола `IPlir` нужно собирать по крупицам: что-то есть в документации, что-то в [статьях на сайте](https://infotecs.ru/about/press-centr/publikatsii/printsipy-marshrutizatsii-i-preobrazovaniya-ip-trafika-v-vpn-seti-sozdannoy-s-ispolzovaniem-tekhnolo.html). После стандартизации в `ТК26` описание протокола в виде рекомендаций по стандартизации доступно публично — `Р 1323565.1.034-2020` «Информационная технология. Криптографическая защита информации. Протокол безопасности сетевого уровня», но насколько оно соблюдается в продуктах ViPNet — известно только производителю.

Для туннелирования используется протокол `IP/241`. Такая инкапсуляция применяется, если устройства находятся в одном широковещательном домене, например, два VPN-клиента в рамках одной подсети. Если же между узлами ViPNet есть устройства сетевого уровня, то происходит инкапсуляция в `IP` и дополнительно в `UDP` (по умолчанию порт `55777`). Если по `UDP` установить связь не удаётся, то вместо него используется `TCP` (по умолчанию порт `80`), обычно это актуально для `ViPNet Client`.

Протокол `IPlir` может работать в трех режимах – туннельный, транспортный и легкий туннель.
- **Туннельный** – шифрование исходного `IP`-пакета целиком
- **Транспортный** – добавление заголовка `IPlir` после заголовка IP исходного пакета
- **Легкий туннель** – аналог транспортного, но с небольшими изменениями внутри протокола `IPlir` , касающихся поддерживаемой версионности.

Возможности настройки у пользователя нет, фактически всегда используется туннельный.

Размер заголовка `IPlir` составляет максимум `57` байт.

Дополнительные накладные расходы режима **L2VPN** (вендорское название — **L2overIP**) составляют `36` байт. Из них `22` байт — инкапсуляция фреймов в EtherIP. Так как фрейм перехватывается целиком, то традиционно добавляем в `overhead` исходный `ethernet` заголовок `+14` байт.

Наиболее распространённые случаи:
- **L3 VPN** через интернет (`UDP` не запрещен) — `65` байт.
- **L2 VPN** через выделенный канал — `93` байт.
# L2VPN

Обратите внимание, что в `L2` режиме фреймы перехватываются целиком независимо от вендора. Поэтому в `overhead` включен исходный `ethernet` заголовок без `FCS`. Если используются дополнительные протоколы, то `overhead` увеличивается на соответствующий размер заголовка, например:
- **VLAN** `+4` байта,
- **MPLS** `+4` байта.