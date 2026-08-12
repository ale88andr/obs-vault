Работает для версии КриптоПроCSP 5

## Для пользователей Windows

Удаляем в реестре ссылки на параметры (по одной ссылке)

```Windows Registry
HKEY_CLASSES_ROOT\WOW6432Node\CLSID\{4BE57065-DC50-4239-8E32-11FABAF5ECF5}
HKEY_CLASSES_ROOT\WOW6432Node\CLSID\{C8B655BB-28A0-4BB6-BDE1-D0826457B2DF}
```

или вводим по-очереди команды для командной строки:

```cmd
reg delete "HKEY_CLASSES_ROOT\WOW6432Node\CLSID\{4BE57065-DC50-4239-8E32-11FABAF5ECF5}" /f
reg delete "HKEY_CLASSES_ROOT\WOW6432Node\CLSID\{C8B655BB-28A0-4BB6-BDE1-D0826457B2DF}" /f
```

Запускаем инсталляцию `CSPSetup`, выбирая «**Исправить**». Перезагружать не нужно!