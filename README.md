git-init-bare - простая утилита для выполнения команды git init --bare, для
использования совместо с git-shell в качестве одной из дополнительных команд,
размещаемых в каталоге ~/git-shell-commands.

При настройке использования git на сервере в самом простом варианте, как описано
[в документации](https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server),
на сервере создаётся пользователь git, которому в качестве командной оболочки
устанавлиается программа git-shell, позволяющая выполнять только ограниченный
набор команд, необходимый для работы git. Возникает вопрос, как при этом
создавать репозитории на сервере. Чтобы эту операцию мог делать пользователь
git, сделан простой скрипт, который по сути выполняет одну команду (проверив
перед этим корректность имени репозитория):

    git init --bare путь_к_репозиторию

Данный скрипт вызывается при подключении к серверу по ssh под именем
пользователя git (или другого, по вашему выбору), для этого скрипт должен быть
размещён в каталоге ~/git-shell-commands, после подключения будет доступна
команда git-init-bare, которой, в качестве параметра, нужно передать имя
репозитория (без суффикса .git, который будет добавлен автоматически).

Например:

    $ ssh git@git.example.com
    git> git-init-bare my_repository

После выполнения подобной команды будет создан пустой репозиторий, доступный по
адресу вида git@git.example.com:/srv/git/my_repository.git

При этом репозиторий будет создан в каталоге /srv/git (пользователь git должен
иметь право на запись в этот каталог), вы можете изменить это в переменной
REPO_BASE_DIR, имя репозитория может содержать только символы латинского
алфавита, цифры, а также точку, подчёркивание и тире, имя должно начинаться
с буквы или цифры, не должно заканчиваться на точку, и быть длиной не более 64
байт, вы можете изменить эти условия в переменной REPO_NAME_REGEX.

