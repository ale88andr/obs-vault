
# Где лежат ключи ViPNet в Linux?

#### Основные пути к файлам ViPNet

#### После установки пакета файлы располагаются следующим образом:

|Тип файлов|Путь|
|---|---|
|Исполняемые файлы|`/opt/itcs/bin/`|
|Заголовочные файлы|`/opt/itcs/include/`|
|Примеры использования|`/opt/itcs/share/samples/`|
|Файлы настроек (системные)|`/etc/opt/itcs/`|
|Данные приложений|`/var/opt/itcs/vipnet-csp/`|
|Пользовательские контейнеры ключей|`~/.itcs/vipnet-csp/containers/`|

#### Контейнеры закрытых ключей

```
# Путь по умолчанию для текущего пользователя:
~/.itcs/vipnet-csp/containers/

# Пример полного пути:
/home/username/.itcs/vipnet-csp/containers/le-b324dba8-7f71-45ad-b9f6-021ea2e9c924
```

#### Просмотр доступных контейнеров

```
# Через утилиту командной строки:
/opt/itcs/bin/csp-gost print_containers

# Пример вывода:
Printing available containers (for user):
  0. /home/user/.itcs/vipnet-csp/containers/test1234
  1. SafeNet eToken (eToken Aladdin): abn_0199.key
SUCCESSED
```

#### Просмотр свойств контейнера

```
/opt/itcs/bin/csp-gost container_content --container /home/user/.itcs/vipnet-csp/containers/test1234
```

#### Полезные команды для работы с ключами

#### Установка ключей из .dst-файла

```
# Для ViPNet Client:
vipnetclient installkeys file.dst

# Важно: dst-файлы должны быть сформированы с ролью linux-client
```

#### Работа с сертификатами

```
# Добавить сертификат УЦ в хранилище CA:
/opt/itcs/bin/certmgr add_certificate --store CA --container <имя_контейнера>

# Добавить пользовательский сертификат в хранилище My:
/opt/itcs/bin/certmgr add_certificate --store My --container <имя_контейнера>

# Просмотр установленных сертификатов:
/opt/itcs/bin/certmgr print_certificates --store My
```

#### Подпись и проверка файлов

```
# Подпись файла:
/opt/itcs/bin/cryptofile sign --in=file.doc --out=file.doc.sig --fingerprint="SHA1_HASH" --nodetach --DER

# Проверка подписи:
/opt/itcs/bin/cryptofile verify --in=file.doc.sig --out=file.doc
```

#### Настройка окружения

#### Добавление путей в PATH (опционально)

```
# Чтобы не указывать полный путь к утилитам:
export PATH=/opt/itcs/bin:$PATH

# Для постоянного добавления добавьте в ~/.bashrc или ~/.profile:
echo 'export PATH=/opt/itcs/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

#### Проверка статуса лицензии

```
/opt/itcs/bin/license status --product csp_linux
```

#### Важные замечания

1. **Формат ключей**: Файлы `.dst` для ViPNet Client на Linux должны быть сформированы с ролью `linux-client`. Ключи для Windows без этой роли не работают.
    
2. **Права доступа**: Для системных операций (установка, регистрация) может потребоваться запуск команд с `sudo`.
    
3. **Графический интерфейс**: В новых версиях доступна утилита `certmgr-gui` для работы с сертификатами через графический интерфейс.
    
4. **Аппаратные токены**: Контейнеры ключей могут храниться не только в файловой системе, но и на аппаратных носителях (Рутокен ЭЦП, eToken и др.).
    
5. **Режим замкнутой программной среды**: При работе на Astra Linux Special Edition требуется дополнительная настройка контроля целостности.
    

---

#### Совет:

Перед установкой ключей убедитесь, что пакет `vipnetclient` или `vipnet-csp` установлен корректно и служба запущена. Проверить состояние можно командой `vipnetclient info`.