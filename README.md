# Основные аспекты работы с Git-репозиториями

## Инициализация Git-репозитория
Создать папку _%НАЗВАНИЕ_ПРОЕКТА%_, перейти в неё командой **cd** и сделать её Git-репозиторием командой **git init**.

```bash
$ cd ~/%ПУТЬ_К_ПАПКЕ_С_ПРОЕКТОМ%/%НАЗВАНИЕ_ПРОЕКТА% # перешли в нужную папку
$ git init # создали репозиторий
```

## Подготовка файлов к сохранению
Для подготовки файлов к отслеживанию осуществить переход в папку Git-репозитория и использовать команду **git add** в вариантах:  
все файлы

```bash
$ git add --all # подготовили к сохранению все файлы в репозитории
$ git status # проверили статус 
```

файлы по одному

```bash
$ git add file1.txt
$ git add file2.txt
$ git status 
```

текущую папку целиком

```bash
$ git add . # добавить всю текущую папку
$ git status 
```

## Выполнение коммита
Для выполнения коммита перейти в папку Git-репозитория и использовать команду **git commit** c аттрибутом **-m ""**

```bash
$ git commit -m "Сообщение коммита: описание произведенных изменений" 
```

## Просмотр истории коммитов
Для просмотра истории коммитов перейти в папку Git-репозитория и использовать команду **git log**

```bash
$ git log 
```

---
## Основные команды для работы с GitHub

### Создание SSH-ключа
Для генерации SSH-пары использовать программу **ssh-keygen**

```bash
$ ssh-keygen -t ed25519 -C "электронная почта, к которой привязан ваш аккаунт на GitHub" 
$ ssh-keygen -t rsa -b 4096 -C "электронная почта, к которой привязан ваш аккаунт на GitHub" 
```

### Привязка SSH-ключа к GitHub
1. Скопировать открытый ключ в буфер обмена

```bash
# скопировать содержимое ключа в буфер обмена:
$ clip < ~/.ssh/id_rsa.pub
# для ed25519:
$ clip < ~/.ssh/id_ed25519.pub 
```

2. В настойках аккаунта GitHub в **Settings** -> **SSH and GPG keys** создать новый ключ (**New SSH key**)/
3. Заполнить поля **Title**, **Key type** (должно быть значение **Authentication Key**), в поле **Key** вставить скопированный публичный ключ.
4. Нажать на кнопку **Add SSH key**.
5. Проверить правильность ключа с помощью команды **ssh -T**

```bash
$ ssh -T git@github.com 
```

### Привязка удалённого репозитория к локальному
Перейти в папку Git-репозитория и использовать команду **git remote add**

```bash
$ cd ~/%ПУТЬ_К_ПАПКЕ_С_ПРОЕКТОМ%/%НАЗВАНИЕ_ПРОЕКТА%
$ git remote add origin git@github.com:%ИМЯ_АККАУНТА%/%НАЗВАНИЕ_ПРОЕКТА%.git 
```

Команде необходимо передать два параметра: имя удалённого репозитория и его URL. В качестве имени можно использовать слово origin. А URL необходимо скопировать со страницы удалённого репозитория GitHub 

### Изменение наименование ветки удаленного репозитория
Раньше основная ветка в репозиториях, созданных на GitHub, называлась **master**, но с 1 октября 2020 года её переименовали в main. 
Во всех репозиториях, созданных раньше этой даты, название основной ветки не поменялось. Поэтому в проектах, которые начали именно с master, и в руководствах по работе с Git вы по-прежнему можете встретить имя master.
Для изменения наименования ветки удаленного репозитория перейти в папку Git-репозитория и использовать команду **git branch -M master**

```bash
$ cd ~/%ПУТЬ_К_ПАПКЕ_С_ПРОЕКТОМ%/%НАЗВАНИЕ_ПРОЕКТА%
$ git branch -M %Наименование_ветки_например_master% 
```

### Отправить изменения на удалённый репозиторий  
Перейти в папку Git-репозитория и использовать команду **git push**

В первый раз команда вызвается с флагом -u и параметрами origin (имя удалённого репозитория) и main или master (название текущей ветки). Флаг -u свяжет локальную ветку с одноимённой удалённой

```bash
$ git push -u origin main # Если команда приведёт к ошибке, попробуйте 
                          # заменить main на master. 
```

### Стадии жизненного цикла файла в Git

    * Файл только что создали. Git про него ещё ничего не знает. Состояние: untracked.
    * Файл добавили в staging area с помощью git add. Состояние: staged (+ tracked).
    * Возможно, изменили файл ещё раз. Состояния: staged, modified (+ tracked).
    * Обратите внимание: staged и modified у одного файла, но у разных его версий.
    * Ещё раз выполнили git add. Состояние: staged (+ tracked).
    * Сделали коммит с помощью git commit. Состояние: tracked.
    * Изменили файл. Состояние: modified (+ tracked).
    * Снова добавили в staging area с помощью git add. Состояния: staged (+ tracked).
    * Сделали коммит. Состояния: tracked.

### Игнорирование файлов в Git

Чтобы Git игнорировал файлы и не пытался добавить их в репозиторий, необходимо создать файл **.gitignore** и записать в него названия игнорируемых файлов.

```
# вот так можно писать комментарии;
# они ничего не значат для .gitignore,
# но они могут быть полезны, чтобы понять, зачем было добавлено то или иное правило 

# для macOS
.DS_Store

# игнорировать все файлы, которые заканчиваются на .jpeg
*.jpeg

# игнорировать все файлы "tmp" во всех подпапках папки docs
docs/*/tmp 

# игнорировать все файлы с один любым символом после "file"
file?.txt 

# игнорировать файлы file0.txt, file1.txt и file2.txt
# при этом не игнорировать file3.txt, file4.txt, ...
file[0-2].txt 

# игнорировать todo.txt в корне репозитория
/todo.txt
# для сравнения: spam.txt будет игнорироваться во всех папках
spam.txt 

# игнорировать папку build
build/

# игнорировать файлы "docs/current/tmp", "docs/old/tmp",
# а также "docs/old/saved/a/b/c/d/tmp"
# и даже "docs/tmp", потому что ноль вложенных папок тоже подходит
docs/**/tmp

# игнорировать только "docs/current/tmp" и "docs/old/tmp"
# файл "docs/old/saved/a/b/c/d/tmp" не попадает в правило
docs/*/tmp 

# игнорировать все JPEG-файлы
*.jpeg

# но только не мем с Doge
!doge.jpeg 

```

### Исправление коммита

Внести правки в уже сделанный коммит с помощью опции --amend команды commit: **git commit --amend --no-edit** (--no-edit означает, что сообщение коммита не изменяется).

```bash
$ git add %%имя_файла%% # добавили в список на коммит
$ git commit --amend --no-edit
```

Изменить соообщение коммита - **git commit --amend -m "Новое сообщение"**

```bash
$ git log --oneline
a31fa24 Добавить главную страницу
$ git add %%имя_файла%% # добавили в список на коммит недостающий файл
$ git commit --amend --no-edit # изменили коммит без правки сообщения
$ git commit --amend -m "Добавить главную страницу и стили" # исправили сообщение коммита
$ git log --oneline
a31fa24 Добавить главную страницу и стили
```

### "Откат" изменений

Команда **git restore --staged <file>** переведёт файл из staged обратно в modified или untracked.
Команда **git reset --hard <commit hash>** "откатит" историю до коммита с хешем <hash>. Более поздние коммиты потеряются!
Команда **git restore <file>** "откатит" изменения в файле до последней сохранённой (в коммите или в staging) версии.

Удаление коммита из удаленного репозитория - **git push origin -f localBranch:remoteBranch**, однако это вызовет проблемы у других пользователей, которые уже извлекли изменения с удаленного репозитория.

Лучше отменить коммиты без изменения истории - **git revert <commit hash>**

### Изучение изменений в файлах

Команда **git diff** сравнит последнюю закоммиченную версию файла с той, что находится в состоянии _modified_.
Команда **git diff --staged** покажет изменения в staged-файлах относительно последних закоммиченных версий.

### Сравнение коммитов

Команда **git diff <commit hash A> <commit hash B>** сравнит коммиты А и В - список инструкций: как превратить состояние A в состояние B.

Git поддерживает суффикс навигации **~**. С его помощью можно сослаться на предыдущие коммиты. Для вывода разницы между тем коммитом, который был три коммита назад, и текущим, необходимо выполнить **git diff main~3 main**.

```bash
$ git diff HEAD~ HEAD 
$ git diff 2ea56ab~ 2ea56ab 
```

### Клонирование репозиториев 

Команда **git clone** копирует проект на локальный компьютер и автоматически связывает локальный репозиторий с удалённым.

```bash
$ git clone https://github.com/yandex-praktikum/git-clone-lesson
$ cd git-clone-lesson
$ git remote -v
origin    git@github.com:yandex-praktikum/git-clone-lesson.git (fetch)
origin    git@github.com:yandex-praktikum/git-clone-lesson.git (push) 
```

### Работа с ветками (branches)

#### Просмотр веток

С помощью команды **git branch** можно посмотреть, какие в проекте есть ветки и в какой из них вы сейчас находитесь

```bash
$ git branch 
* main # мы в основной ветке

# чтобы выйти из просмотра веток, может понадобиться Q! 
```

#### Создание веток

Создать ветку — **git branch <название_ветки>**

```bash
$ git branch feature/add-branch-info # создали ветку feature/add-branch-info
$ git branch # посмотрели ветки
  
  feature/add-branch-info  # появилась новая
* main                     # * значит, что мы находимся в основной ветке 
```

#### Переключение между ветками

Начать работу в другой ветке необходимо с использованием команды **git checkout %%НАЗВАНИЕ_ВЕТКИ%%**.

```bash
$ git checkout feature/add-branch-info # перешли в новую ветку
Switched to branch 'feature/add-branch-info'

$ git branch # проверили

* feature/add-branch-info # теперь находимся тут
  main 
```

Создать ветку и сразу переключиться на неё — **git checkout -b %%НАЗВАНИЕ_ВЕТКИ%%**

#### Сравнение веток

Для сравнения веток по их названиям используется **git diff %%НАЗВАНИЕ_ВЕТКИ_А%% %%НАЗВАНИЕ_ВЕТКИ_В%%**.

```bash
$ git diff main feature
```

Для сравнения веток по их названиям и хешем коммитов используется **git diff %%НАЗВАНИЕ_ВЕТКИ_А%% %%Хеш_коммита%%**.

```bash
$ git diff main 2ea56ab
```

Для сравнения веток с использованием суффикса навигации ~ используется **git diff %%НАЗВАНИЕ_ВЕТКИ_А%% %%НАЗВАНИЕ_ВЕТКИ_А%%~N**.

```bash
$ git diff main~3 main
```

#### Создание ветки в удаленном репозитории из локального

##### Вариант, предложенный Я.П

Для создания ветки в удаленном репозитории из локального используется команда **git push -u origin %%НАЗВАНИЕ_ВЕТКИ%%**

```bash
$ git push -u origin feature/merge-request
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'feature/merge-request' on GitHub by visiting:
remote:      https://github.com/%ВАШ_АККАУНТ%/git-branches/pull/new/feature/merge-request
remote: 
To github.com:%ВАШ_АККАУНТ%/git-branches.git
 * [new branch]      feature/merge-request -> feature/merge-request
branch 'feature/merge-request' set up to track 'origin/feature/merge-request'. 
```

##### Вариант, предложенный Git

Для создания ветки в удаленном репозитории из локального используется команда **git push --set-upstream origin %%НАЗВАНИЕ_ВЕТКИ%%**

```bash
$ git push --set-upstream origin feature/add-brach-info
Enumerating objects: 11, done.
#...
 * [new branch]      feature/add-brach-info -> feature/add-brach-info
branch 'feature/add-brach-info' set up to track 'origin/feature/add-brach-info'.
```

#### Объединение веток

Для слияния двух веток используется **git merge %%НАЗВАНИЕ_ВЕТКИ%%**

```bash
$ git checkout main # переключились на главную ветку

$ git merge feature/diff # объединили ветки
Updating 079cfbf..f30d441
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+) 
```

Для удаления ветки после объединения использууется **git branch -D %%НАЗВАНИЕ_ВЕТКИ%%** или **git branch -d %%НАЗВАНИЕ_ВЕТКИ%%**. Второй вариант удалит ветку только если она была полностью объединена с другой.

```bash
$ git branch # проверяем местоположение
  bugfix/fix-branch
  feature/add-branch-info
  feature/diff
* main

$ git checkout main # если не в основной, переходим в неё

$ git branch -D feature/diff # удаляем поглощаемую ветку
Deleted branch feature/diff (was f30d441). 
```

#### Скачивание изменений из удаленного репозитория

Для того, чтобы забрать изменения из удалённого репозитория используется команда **git pull**

---

#### Дополнительные сведения и полезные функции
1. Проверить состояние репозитория — git status
2. Если случайно сделана Git-репозиторием не та папка - её можно «разгитить»: удалить скрытую подпапку .git.

```bash
$ cd ~/%ПУТЬ_К_ПАПКЕ_С_ПРОЕКТОМ%/%НАЗВАНИЕ_ПРОЕКТА% # перешли в нужную папку
$ rm -rf .git # удалили подпапку .git
```

3. Для проверки связи репозиториев (локального и удаленного) используется команда **git remote -v**

```bash
$ git remote -v
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (fetch)
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (push) 
```

4. README.md - файл со сведениями о проекте:  
    - Название проекта и его краткое описание: кем создан, для чего, какие решает задачи и какие закрывает проблемы.  
    - Технологии, которые применяются в проекте. В чём его отличие от аналогичных.  
    - Документация проекта — подробная инструкция о том, что представляет собой проект.
    - Планы проекта.  
    Преимущество README.md в том, что средства командной работы могут отображать его содержимое в браузере в виде удобной разметки. Для этого нужно не просто залить текст, но и настроить шрифт, заголовки и отступы с помощью markdown. Маркда́ун — это специальный язык разметки. Он позволяет красиво отформатировать текстовый документ.  
5. Получить сокращённый лог — **git log --oneline**.
6. Получить сокращённый лог можно с помощью команды **git log** с флагом **--oneline** (умещается максимум 72 первых символа сообщения).
7. Файл HEAD — один из служебных файлов папки .git. Он указывает на коммит, который сделан последним. Внутри HEAD — ссылка на служебный файл: refs/heads/master (или refs/heads/main). При работе с Git указатель HEAD используется довольно часто. Если нужно передать в команду последний коммит, то вместо его хеша можно просто написать слово HEAD.
8. Если необходимо отобразить все игнорируемые файлы в выводе команды **git status** используется ключ **--ignored**: **git status --ignored**.
9. Комбинация **«форк» + clone** используется для внесения изменений в публичные репозитории. В этом случае «форк» становится подготовительным этапом перед клонированием чужого репозитория на ваш компьютер. Если репозиторий приватный или это репозиторий вашей компании, при работе с ним достаточно **clone**. Копия, которая получена с помощью **«форка»**, полностью независима от оригинального проекта — изменения не будут синхронизированы.
10. Работа с механизмом Pull request описана в файле Pull_request.pdf. Перед созданием нового пул-реквеста считается хорошей практикой перейти в главную ветку, «подтянуть» в неё изменения, а затем добавить эти изменения в вашу ветку с помощью git merge main.