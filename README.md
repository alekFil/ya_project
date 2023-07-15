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

##### Исправление коммита

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

