Команда `git init` запускает процесс инициализации нового Git-репозитория в текущей директории.

## Что создаётся

- Появляется скрытая папка `.git` — это **основной репозиторий**. В ней хранятся:
    
    - `HEAD` — указатель на текущую ветку
        
    - `config` — настройки репозитория
        
    - `objects/` — база данных всех коммитов, файлов и деревьев
        
    - `refs/` — ссылки на ветки и теги
        
    - `hooks/` — скрипты, которые можно запускать при событиях (например, перед коммитом)

Git — это не просто система версий, это **база данных объектов**, и всё, что мы видим (файлы, коммиты, ветки) — это ссылки на эти объекты.

Git хранит всё в виде **4 типов объектов**, каждый из которых имеет уникальный `SHA-1` хеш:

![[Pasted image 20250829121403.png]]

### 1. **Blob (Binary Large Object)** Файлы

- Содержит **содержимое файла** (только данные, без имени или прав доступа).
    
- Один и тот же файл с одинаковым содержимым будет иметь один и тот же blob — даже в разных коммитах.

Пример: файл `README.md` → blob с хешем `a1b2c3...`

### 2. **Tree** (Директории)

- Представляет **директорию**: содержит список файлов и поддиректорий, указывая на blob'ы и другие tree'ы.
    
- Хранит имена файлов, права доступа и ссылки на объекты.

Пример: корневая папка проекта → tree с ссылками на blobs и другие trees

### 3. **Commit**

- Содержит:
    
    - Ссылку на tree (состояние проекта)
        
    - Ссылку на родительский коммит (или несколько, если это merge)
        
    - Метаданные: автор, дата, сообщение

Пример: `git commit -m "Initial commit"` → создаёт commit-объект

### 4. **Tag**

- Указывает на commit (или другой объект) и может содержать подпись.
    
- Бывают **аннотированные** (с метаданными) и **легковесные** (просто ссылка).

Пример: `v1.0.0` → tag на коммит с релизом

**Как они связаны**

```text
Commit
  └── Tree
        ├── Blob (file1.txt)
        ├── Blob (main.py)
        └── Tree (subfolder/)
               └── Blob (nested.txt)
```

## Как посмотреть объекты

```bash
git cat-file -p <hash>   # Посмотреть содержимое объекта
git ls-tree HEAD         # Посмотреть дерево текущего коммита
git rev-parse HEAD       # Получить хеш текущего коммита
```

## Как **Git** работает под капотом? 

Мы создадим `blob`, `tree` и `commit` вручную, без `git add` и `git commit`.

##### Шаг 1: Инициализируем репозиторий

```shell
mkdir git-lab && cd git-lab
git init
```

##### Шаг 2: Создаём blob-объект

Создаём файл:

```shell
echo "Hello Git World" > hello.txt
```

Создаём **blob** вручную:

```shell
git hash-object -w hello.txt
```

**Git** вернёт `SHA-1` хеш — это `ID` blob-объекта. 

```
ea701271a58054773256b0ff0dbf2fde425f19d6
```

И создаст `blob` объект в директории `.git/objects`

Посмотреть содержимое:

```shell
git cat-file -p <blob_hash>
```

##### Шаг 3: Создаём tree-объект

Создаем индекс вручную:

```shell
git update-index --add hello.txt
```

Создай tree:

```shell
git write-tree
```

Это создаёт **snapshot** текущего состояния файлов. 

Посмотреть дерево:

```shell
git ls-tree <tree_hash>
```

```shell
100644 blob ea701271a58054773256b0ff0dbf2fde425f19d6    hello.txt
```

##### Шаг 4: Создаём commit вручную


```shell
git commit-tree <tree_hash> -m "Мой ручной коммит"
```

**Git** вернёт хеш коммита.

```
565c1e81e364d72767d6c0c95ae0513473cdb237
```

Посмотреть:

```shell
git cat-file -p <commit_hash>
```

```shell
tree e497723a6beb377202329333c7fa5b074f298fb7
author Alexandr <ale88andr@gmail.com> 1756380844 +0000
committer Alexandr <ale88andr@gmail.com> 1756380844 +0000

Мой ручной коммит
```

##### Шаг 5: Привязываем HEAD

```shell
git update-ref HEAD <commit_hash>
```

при этом в `.git/refs/heads/main` изменится `SHA-1` коммита на `<commit_hash>`

| Объект | Команда               | Назначение                   |
| ------ | --------------------- | ---------------------------- |
| Blob   | `git hash-object -w`  | Содержимое файла             |
| Tree   | `git write-tree`      | Структура файлов             |
| Commit | `git commit-tree`     | Снимок состояния + сообщение |
| HEAD   | `git update-ref HEAD` | Указатель на текущий коммит  |
Создадим ветку вручную:

Создадим новый файл в директории `.git/refs/heads`

```
sudo nano .git/refs/heads/new
```

и занесем туда хеш коммита из существующей ветки, например, `main`

проверяем ветки:

```shell
git branch 
* main
  new
```

## Области **Git**

![[Pasted image 20250828175325.png]]

## Commit Messages


![[Pasted image 20250829092356.png]]
## Conventional commits

![[Pasted image 20250829121125.png]]

```
<type>(<scope>): <subject>
<body>
<footer>
```

**Types**
- `feat` - new feature
- `fix` - bug fix
- `docs` - documentation only
- `style` - formatting, no code change
- `refactor` - code restructuring, no functional change
- `perf` - performance improvements
- `test` - adding tests
- `build` - build system or dependencies
- `ci` - Cl configuration files
- `chore` - other changes (tooling, configs)
- `revert` - revert previous commit

**Common Scopes**
- `арi` - API endpoints, contracts
- `auth` - authentication, authorization
- `core` - core functior>lity
- `db` - database, migrations
- `deps` - dependencies
- `config` - configuration files
- `ui` - user interface
- `ux` - user experience
- `i18n` - internationalization
- `a11y` -accessibility
- `security` - security fixes
- `utils` - utilities, helpers
- `types` - type definitions
- `tests` - test infrastructure
- `docs` - documentation
- `build` - build scripts
- `ci` - continuous integration
- `docker` - Docker configuration
- `k8s` - Kubernetes configs

**Examples**
- feat(auth): Add 0Auth2 integration 
- fix(api): handle null in user response 
- docs(readme): update installation steps 
- style(ui): format button components 
- refactor(core): extract validation logic 
- test(api): add user endpoint tests 
- build(deps): upgrade React to vl8 
- chore(config): update eslint rules

# Git Commands

⚙️ `git cat-file -p 75675257f7f4180abd2d01fbc6e6d71dfb238f41`

- Show content of git object in pretty format

⚙️ `git add -p`

- Pick & stage changes piece by piece

⚙️ `git diff --cached`

- Show what’s staged (ready to commit)

⚙️ `git reset --soft HEAD~1`

- Uncommit → changes remain staged (undoes commit but keeps changes staged)

⚙️ `git reset --mixed HEAD~1`

- Undoes commit, keeps changes unstaged

⚙️ `git reset HEAD~1`

- Same as above (--mixed is default)

⚙️ `git reset --hard HEAD~1`

- COMPLETELY discards commit and all changes (WARNING: Destructive! All changes are LOST)

⚙️ `git checkout 84e9069`

- Temporary view → requires stashing changes (detached HEAD state)

⚙️ `git switch 84e9069`

- Modern alternative (detached HEAD state, but safer)

## ВЕТКИ И УПРАВЛЕНИЕ ИСТОРИЕЙ

`git merge` и `git rebase` — это два способа объединения изменений из одной ветки в другую, но они делают это по-разному и с разными последствиями для истории коммитов.

![[Pasted image 20250901121508.png]]

### `git merge`: сохраняет историю как есть

- **Что делает**: объединяет две ветки, создавая новый коммит слияния.
- **История**: сохраняет все коммиты обеих веток и добавляет один новый — merge-коммит.
- **Плюсы**:
    - История прозрачна: видно, когда и какие ветки были объединены.
    - Безопасно для общих веток (например, `main`), особенно при совместной работе.
- **Минусы**:
    - История может стать запутанной, особенно при множественных слияниях.

**Пример**:

```bash
git checkout main
git merge feature-branch
```

### `git rebase`: переписывает историю

- **Что делает**: переносит коммиты из одной ветки на другую, как будто они были созданы там изначально.
- **История**: становится линейной, без merge-коммитов.
- **Плюсы**:
    - Чистая, линейная история — удобно читать и анализировать.
    - Идеально для подготовки ветки перед отправкой в `main`.
- **Минусы**:
    - Меняет историю коммитов — может вызвать проблемы, если ветка уже опубликована.
    - Требует осторожности при совместной работе.

**Пример**:

```bash
git checkout feature-branch
git rebase main
```

|Ситуация|Лучше использовать|
|---|---|
|Работа в команде|`merge`|
|Подготовка ветки перед пушем|`rebase`|
|Нужно сохранить историю|`merge`|
|Хочешь чистую линейную историю|`rebase`|
Пример `rebase`

перепишем историю последних 3-х коммитов

```bash
# Первый коммит
echo "console.log('Hello');" > script.js
git add script.js
git commit -m "Добавил скрипт"

# Второй коммит
echo "console.log('World');" >> script.js
git add script.js
git commit -m "Дописал вывод"

# Третий коммит
sed -i '' 's/World/WORLD/g' script.js  # Меняем 'World' на 'WORLD' 
git add script.js
git commit -m "Исправил регистр"

git rebase -i HEAD~3
```

`rebase` предлагает переписать историю:

![[Pasted image 20250901133741.png]]

переписываем используя нужные нам команды, следующим образом

```bash
reword fcfdbb1 Добавил скрипт
reword 7444a9f Дописал вывод
reword a145834 Исправил регистр
```

и выходим из редактора `rebase`, который сразу же предлагает нам изменить коммит `fcfdbb1` (изменить его коммит-сообщение), переписываем сообщение, сохраняем изменения и выходим. Редактор `rebase` предложит нам отредактировать следующий коммит `7444a9f` и т.д. В конце получаем сообщение

```bash
Successfully rebased and updated refs/heads/new.
```

Смотрим `git log` сообщения переписались:

```bash
git log --oneline
77b00f6 (HEAD -> new) fix: set uppercase of string
08f1bbe feat: add console.log
842c6ba feat: create first version of script
```









### Стратегии ветвления

![[Pasted image 20250901122342.png]]

## 1. GitFlow

```
time →   0----1----2----3----4----5----6----7----8----9----10---11---12
         |    |    |    |    |    |    |    |    |    |    |    |    |

master   *----*----*---------*-----------------*----*---------*----*     ← Releases only
         |    |    |         ↑                 ↑    ↑         ↑    ↑
         |    |    |         │(4)              │(9) │(11)     │    │(12)
         |    |    |       merge            merge merge    merge  merge
                             
hotfix   |    |    |    *----*                 |    |         |    |     ← Emergency fixes
         |    |    |    ↑    │(3)              |    |         |    |
         |    |    |  branch │fix              |    |         |    |
         |    |    |    │    ↓                 |    |         |    |
         
release  |    |    |----*----*----*----*----*  |    *----*----*    |     ← Release preparation
         |    |    ↑    │(2) │    │    │    │  |    ↑    │    │    |
         |    |  branch │test│bug │test│ ok │  |  branch │test│ ok │
         |    |    │    ↓    ↓    ↓    ↓    ↓  |    │    ↓    ↓    |
         
develop  *----*----*----*----*----*----*----*--*----*----*----*----*     ← Main development
         │(1) │    ↑    │    │    │    │    │  │    │    │    │    │
         │    │  merge  │    │    │    │    │  │    │    │    │    │
         ↓    ↓    │    ↓    ↓    ↓    ↓    ↓  ↓    ↓    ↓    ↓    ↓
         
feature1 *----*----*    |    |    |    |    |  |    |    |    |    |     ← Features developed in parallel
feature2 |    *----*----*    |    |    |    |  |    |    |    |    |
feature3 |    |    |    *----*----*    |    |  |    |    |    |    |
feature4 |    |    |    |    |    *----*----*  |    |    |    |    |
feature5 |    |    |    |    |    |    |    |  *----*----*    |    |
feature6 |    |    |    |    |    |    |    |  |    |    *----*----*

Change Flow:
(1) Create feature branches from develop
(2) Feature → merge into develop after completion  
(3) Hotfix → created from master, merged into master AND develop
(4) Release → created from develop, merged into master after testing
(5) Master → stable releases only, never direct commits
```

## 2. GitHub Flow - Simple Pipeline

```
time →   0----1----2----3----4----5----6----7----8----9----10---11---12
         |    |    |    |    |    |    |    |    |    |    |    |    |

main     *----*----*----*----*----*----*----*----*----*----*----*----*    ← Main branch (always deployable)
         │(1) ↑(2) ↑(3) ↑(4) ↑(5) ↑(6) ↑(7) ↑(8) ↑(9) ↑(10)↑(11)↑(12)
         │   merge│   merge│   merge│   merge│   merge│   merge│   merge
         │    │    │    │    │    │    │    │    │    │    │    │    │
         │    │    │    │    │    │    │    │    │    │    │    │    │
         ↓    │    │    │    │    │    │    │    │    │    │    │    │
         
feature1 *----*PR  |    |    |    |    |    |    |    |    |    |    |    ← Short-lived feature branches
feature2 |    *----*PR  |    |    |    |    |    |    |    |    |    |
feature3 |    |    *----*PR  |    |    |    |    |    |    |    |    |
bugfix1  |    |    |    *----*PR  |    |    |    |    |    |    |    |
feature4 |    |    |    |    *----*----*PR  |    |    |    |    |    |    ← Sometimes slightly longer
hotfix1  |    |    |    |    |    *PR  |    |    |    |    |    |    |    ← Quick fixes
feature5 |    |    |    |    |    |    *----*PR  |    |    |    |    |
feature6 |    |    |    |    |    |    |    *----*----*PR  |    |    |
feature7 |    |    |    |    |    |    |    |    *----*PR  |    |    |
feature8 |    |    |    |    |    |    |    |    |    *----*----*PR  |

Change Flow:
(1) Create feature branch from main
(2) Pull Request → Code Review → Merge into main
(3) Every merge into main = potential release
(4) Deploy directly from main (often automatic)
(5) Hotfix = regular feature branch but with priority

PR = Pull Request (Code Review mandatory)
```

