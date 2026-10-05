---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "1179020555"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/1179020555"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Отправка уведомлений через POST-запрос / Тестирование приема POST-запросов"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Тестирование приема POST-запросов

> [!info] Раздел: System.Collections.Hashtable[@{Id=1179020555; ReuseId=; Title=Тестирование приема POST-запросов; Depth=3; Path=Справочник разработчика / Отправка уведомлений через POST-запрос / Тестирование приема POST-запросов; Segments=System.Object[]; Index=736}.Id])
> @{Id=1179020555; ReuseId=; Title=Тестирование приема POST-запросов; Depth=3; Path=Справочник разработчика / Отправка уведомлений через POST-запрос / Тестирование приема POST-запросов; Segments=System.Object[]; Index=736}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/1179020555)

---

В качестве примера ПО для приема POST-запросов и получения данных уведомления приводится сценарий на языке Python. В сценарии реализованы следующие возможности:

- При получении POST-запроса в интерфейс командной строки выводится его текст.
- При получении POST-запроса в MaxPatrol VM отправляется подтверждение с кодом ответа 2xx.
- По ссылке в POST-запросе из MaxPatrol VM скачиваются не более 10 страниц данных уведомления и выводятся в интерфейс командной строки.
- При необходимости по ссылке в POST-запросе из MaxPatrol VM скачивается схема данных уведомления и выводится в интерфейс командной строки.
- При получении данных из MaxPatrol VM по протоколу HTTPS для проверки подлинности сервера может быть предоставлен сертификат SSL.

Для выполнения сценария на сервере должны быть установлены операционные системы Windows 2012 R2 или Debian 9 и интерпретатор языка Python версии 3.6 или 3.7 с библиотеками Flask и Requests.

> [!note] Примечание
> Для отправки уведомлений на сервер в веб-интерфейсе MaxPatrol VM нужно создать задачу на отправку уведомлений через PОST-запросы. В параметрах задачи нужно указать URL сервера в виде `http://<IP-адрес или FQDN>/handle`.

**Задача.** Чтобы настроить прием POST-запросов и получение данных уведомления:

1. Создайте файл с расширением .py и скопируйте в него код сценария.
2. Если требуется, с помощью параметра `HOST` укажите сетевой интерфейс сервера.
3. Если требуется выводить в интерфейс командной строки схему данных уведомления, для параметра `SHOW_SCHEMA` укажите значение `True`.
4. Если требуется, с помощью параметров `HTTP_PORT` и `HTTPS_PORT` измените порты для приема запросов по протоколам HTTP и HTTPS.
5. Если требуется использовать протокол HTTPS, для параметра `USE_HTTPS` укажите значение `True`.
   > [!warning] Внимание
   > Для проверки подлинности при получении сценарием данных из MaxPatrol VM по протоколу HTTPS нужно выпустить сертификат SSL. Файлы сертификата и ключа к нему нужно поместить в одну папку с файлом сценария. С помощью параметров сценария `certfile` и `keyfile` нужно указать имена файлов сертификата и ключа.
6. Сохраните файл.
7. Откройте интерфейс командной строки и запустите сценарий:
   ```
   python <Имя файла сценария>.py
   ```

> Прием POST-запросов настроен. Данные уведомлений будут выводиться в интерфейс командной строки.

## Код сценария для приема POST-запросов

```
"""pip install flask requests"""
import requests
from pathlib import Path
from flask import Flask, request
HOST = '0.0.0.0'
HTTP_PORT = 10080
HTTPS_PORT = 10443
SHOW_SCHEMA = False
USE_HTTPS = False
cert_dir = Path(__file__).parent
certfile = cert_dir / '<Имя файла сертификата>.crt'
keyfile = cert_dir / '<Имя файла ключа>.pem'
app = Flask('ExampleTriggerService')
@app.route('/handle', methods=['POST'])
def handle():
    data = request.get_json()
    print('INCOMING DATA', data)
    if data['notification_type'] == 'test':
        return print('TEST') or 'test'
    if SHOW_SCHEMA:
        print('GET SCHEMA', data['schema_uri'])
        schema_data = requests.get(data['schema_uri'], verify=False).text
        print('SCHEMA DATA', schema_data)
    page_uri = data['uri']
    for page_num in range(10):
        if not page_uri:
            break
        print('GET PAGE', page_num, page_uri)
        page_data = requests.get(page_uri, verify=False).json()
        print('PAGE DATA', page_data)
        page_uri = page_data['dataSetInfo']['nextDataSetUri']
    return print('OK') or 'ok'
if __name__ == '__main__':
    if USE_HTTPS:
        import ssl
        context = ssl.SSLContext(ssl.PROTOCOL_TLS)
        # context = ssl.SSLContext(ssl.PROTOCOL_TLSv1_2) # для Python версии 3.6
        context.load_cert_chain(str(certfile), str(keyfile))
        port = HTTPS_PORT
    else:
        context = None
        port = HTTP_PORT
    app.run(ssl_context=context, host=HOST, port=port, debug=True)
```
