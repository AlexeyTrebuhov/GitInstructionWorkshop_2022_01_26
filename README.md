## Соответствие групп и тем на практикум.

1. Что такое система контроля версий
2. Для чего нужна система контроля версий
3. Установка git на ваш ПК (в зависимости от системы)
4. Установка VSCode на ваш ПК
> заходим по адресу https://code.visualstudio.com/download
запускаем инсталлятор, после установки регистрируемся.

5. Что такое репозиторий и инструкция по созданию локальных репозиториев.
6. Базовая работа с локальным репозиторием

Базовые команды
git init — создание репозитория
Команда git init создает в директории пустой репозиторий в виде директории .git, где и будет в дальнейшем храниться вся информация об истории коммитов, тегах — о ходе разработки проекта:

mkdir project-dir
cd project-dir
git init
git add и git rm — индексация изменений
Следующее, что нужно знать — команда git add. Она позволяет внести в индекс — временное хранилище — изменения, которые затем войдут в коммит.

Индексирует измененный файл, либо оповещение о создании нового:

git add EDITEDFILE
Вносит в индекс все изменения, включая новые файлы:

git add .
Из индекса и дерева проекта одновременно файл можно удалить командой git rm.

Удаляет из индекса и дерева проекта отдельные файлы:

git rm FILE1 FILE2
Хороший пример удаления из документации к git, удаляются сразу все файлы txt из папки:

git rm Documentation/\*.txt
Вносит в индекс все удаленные файлы:

git rm -r --cached .
Сбросить весь индекс или удалить из него изменения определенного файла можно командой git reset:

git reset
Удаляет из индекса конкретный файл:

git reset — EDITEDFILE
Команда git reset используется не только для сбрасывания индекса, поэтому дальше ей будет уделено гораздо больше внимания.

git status — состояние проекта, измененные и не добавленные файлы, индексированные файлы
Команду git status, пожалуй, можно считать самой часто используемой наряду с командами коммита и индексации. Она выводит информацию обо всех изменениях, внесенных в дерево директорий проекта по сравнению с последним коммитом рабочей ветки; отдельно выводятся внесенные в индекс и неиндексированные файлы. Использовать ее крайне просто:

git status
Кроме того, git status указывает на файлы с неразрешенными конфликтами слияния и файлы, игнорируемые git.

git commit — совершение коммита
Коммит — базовое понятие во всех системах контроля версий, поэтому совершаться он должен легко и по возможности быстро. В простейшем случае достаточно после индексации набрать:

git commit
Если индекс не пустой, то на его основе будет совершен коммит, после чего пользователя попросят прокомментировать вносимые изменения вызовом команды edit. Сохраняемся, и вуаля! Коммит готов. Есть несколько ключей, упрощающих работу с git commit.

Совершает коммит, автоматически индексируя изменения в файлах проекта. Новые файлы при этом индексироваться не будут! Удаление же файлов будет учтено:

git commit -a
Комментирует коммит прямо из командной строки вместо текстового редактора:

git commit -m «commit comment»
Вносит в индекс и создаёт коммит на основе изменений единственного файла:

git commit FILENAME
Пример написания хорошего сообщения коммита:

Описывает изменение (до 50 символов)

Более детальное объяснение, если необходимо. Перенос на 72 символе
или около того. В некоторых контекстах первая строка рассматривается
как тема письма, а остальное как тело. Пустая строка, отделяющая сводку
от тела, важна (если вы не опустили тело целиком); если вы оставите их
вместе, инструменты, такие как rebase, могут воспринять это неправильно.

Дальнейшие параграфы идут после пустых строк

 - также можно применять маркеры списков

 - обычно в качестве маркера списка используется дефис или звёздочка
   с одним пробелом перед ним и пустыми строками между пунктами,
   хотя соглашения в этом аспекте могут разниться

Если вы используете систему отслеживания задач, поставьте ссылки на нее:

Resolves: #123
See also: #456, #789
git reset — возврат к определенному коммиту, откат изменений, «жесткий» или «мягкий»
Помимо работы с индексом (см. выше), git reset позволяет сбросить состояние проекта до какого-либо коммита в истории. В git данное действие может быть двух видов: «мягкого»(soft reset) и «жесткого» (hard reset).

«Мягкий» (с ключом --soft) резет оставит нетронутыми ваши индекс и все дерево файлов и директорий проекта, вернется к работе с указанным коммитом. Иными словами, если вы обнаруживаете ошибку в только что совершенном коммите или комментарии к нему, то легко можно исправить ситуацию:

Некорректный коммит
git commit
Переход к работе над уже совершенным коммитом, сохраняя все состояние проекта и проиндексированные файлы
git reset --soft HEAD^
Редактирование файла или файлов
Добавление файлов в индекс
git add .
Возврат к последнему коммиту, будет предложено отредактировать его сообщение
git commit -c ORIG_HEAD
Если сообщение оставить прежним, то достаточно изменить регистр ключа -с
git commit -C ORIG_HEAD
Обратите внимание на обозначение HEAD^, оно означает «обратиться к предку последнего коммита». Подробней описан синтаксис такой относительной адресации будет ниже, в разделе «Хэши, тэги, относительная адресация». Соответственно, HEAD — ссылка на последний коммит. Ссылка ORIG_HEAD после «мягкого» резета указывает на оригинальный коммит.

Естественно, можно вернуться и на большую глубину коммитов,

«Жесткий» резет (ключ --hard) — команда, которую следует использовать с осторожностью. git reset --hard вернет дерево проекта и индекс в состояние, соответствующее указанному коммиту, удалив изменения последующих коммитов:

git add .
git commit -m «destined to death»
git reset --hard HEAD~1 — больше никто и никогда не увидит этот позорный коммит...
git reset --hard HEAD~3 — ...вернее, три последних коммита. Никто. Никогда!
Если команда достигнет точки ветвления, удаления коммита не произойдет.

Для команд слияния или выкачивания последних изменений с удаленного репозитория примеры резета будут приведены в соответствующих разделах.

git revert — отмена изменений, произведенных в прошлом отдельным коммитом
Возможна ситуация, в которой требуется отменить изменения, внесенные отдельным коммитом. git revert создает новый коммит, накладывающий обратные изменения.

Отменяет коммит, помеченный тегом:

git revert config-modify-tag
Отменяет коммит, используя его хэш:

git revert cgsjd2h
Для отмены коммита слияния (коммита у которого несколько родителей), необходимо указать хэш и номер одного из родителей коммита:

git revert cgsjd2h -m 1
Для использования команды необходимо, чтобы состояние проекта не отличалось от состояния, зафиксированного последним коммитом.

git log — разнообразная информация о коммитах в целом
Иногда требуется получить информацию об истории коммитов; коммитах, изменивших отдельный файл; коммитах за определенный отрезок времени и так далее. Для этих целей используется команда git log.

Простейший пример использования, в котором приводится короткая справка по всем коммитам, коснувшимся активной в настоящий момент ветки (о ветках и ветвлении подробно узнать можно ниже, в разделе «Ветвления и слияния»):

git log
Получает подробную информацию о каждом в виде патчей по файлам из коммитов можно, добавив ключ -p (или -u):

git log -p
Статистика изменения файлов, вроде числа измененных файлов, внесенных в них строк, удаленных файлов вызывается ключом --stat:

git log --stat
За информацию по созданиям, переименованиям и правам доступа файлов отвечает ключ --summary:

git log --summary
Чтобы просмотреть историю отдельного файла, достаточно указать в виде параметра его имя (кстати, в моей старой версии git этот способ не срабатывает, обязательно добавлять " — " перед «README»):

git log README
или, если версия git не совсем свежая:

git log — README
Далее приводится только более современный вариант синтаксиса. Возможно указывать время, начиная в определенного момента («weeks», «days», «hours», «s» и так далее):

git log --since=«1 day 2 hours» README
git log --since=«2 hours» README
изменения, касающиеся отдельной папки:

git log --since=«2 hours» dir/
Можно отталкиваться от тегов.

Все коммиты, начиная с тега v1:

git log v1...
Все коммиты, включающие изменения файла README, начиная с тега v1:

git log v1... README
Все коммиты, включающие изменения файла README, начиная с тега v1 и заканчивая тегом v2:

git log v1..v2 README
Интересные возможности по формату вывода команды предоставляет ключ --pretty.

Выводит на каждый из коммитов по строчке, состоящей из хэша (здесь — уникального идентификатора каждого коммита, подробней — дальше):

git log --pretty=oneline
Лаконичная информация о коммитах, приводятся только автор и комментарий:

git log --pretty=short
Более полная информация о коммитах, с именем автора, комментарием, датой создания и внесения коммита:

git log --pretty=full/fuller
В принципе, формат вывода можно определить самостоятельно:

git log --pretty=format:'FORMAT'
Определение формата можно поискать в разделе по git log из Git Community Book или справке. Красивый ASCII-граф коммитов выводится с использованием ключа --graph.

git diff — отличия между деревьями проекта, коммитами и т.д.
Своего рода подмножеством команды git log можно считать команду git diff, определяющую изменения между объектами в проекте - деревьями (файлов и директорий).

Показывает изменения, не внесенные в индекс:

git diff
Изменения, внесенные в индекс:

git diff --cached
Изменения в проекте по сравнению с последним коммитом:

git diff HEAD
Предпоследним коммитом:

git diff HEAD^
Можно сравнивать «головы» веток:

git diff master..experimental
или активную ветку с какой-либо:

git diff experimental
git show — показать изменения, внесенные отдельным коммитом
Посмотреть изменения, внесенные любым коммитом в истории, можно командой git show:

git show COMMIT_TAG
git blame и git annotate — команды, помогающие отслеживать изменения файлов
При работе в команде часто требуется выяснить, кто именно написал конкретный код. Удобно использовать команду git blame, выводящую построчную информацию о последнем коммите, коснувшемся строки, имя автора и хэш коммита:

git blame README
Можно указать и конкретные строки для отображения:

git blame -L 2,+3 README — выведет информацию по трем строкам, начиная со второй.
Аналогично работает команда git annotate, выводящая и строки, и информацию о коммитах, их коснувшихся:

git annotate README
git grep — поиск слов по проекту, состоянию проекта в прошлом
git grep, в целом, просто дублирует функционал знаменитой юниксовой команды. Однако он позволяет слова и их сочетания искать в прошлом проекта, что бывает очень полезно.

Ищет слова tst в проекте:

git grep tst
Подсчитывает число упоминаний tst в проекте:

git grep -с tst
Ищет в старой версии проекта:

git grep tst v1
Команда позволяет использовать логическое И и ИЛИ.

Ищет строки, где упоминаются и первое слово, и второе:

git grep -e 'first' --and -e 'another'
Ищет строки, где встречается хотя бы одно из слов:

git grep --all-match -e 'first' -e 'second'
Ветвление
git branch — создание, перечисление и удаление веток
Работа с ветками — очень легкая процедура в git, все необходимые механизмы сконцентрированы в одной команде.

Просто перечисляет существующие ветки, отметив активную:

git branch
Создаёт новую ветку new-branch:

git branch new-branch
Удаляет ветку, если та была залита (merged) с разрешением возможных конфликтов в текущую:

git branch -d new-branch
Удаляет ветку в любом случае:

git branch -D new-branch
Переименовывает ветку:

git branch -m new-name-branch
Показывывает те ветки, среди предков которых есть определенный коммит:

git branch --contains v1.2
Показывает коммит ответвления ветки new-name-branch от ветки master:

git merge-base master new-name-branch
git checkout — переключение между ветками, извлечение файлов
Команда git checkout позволяет переключаться между последними коммитами (если упрощенно) веток:

checkout some-other-branch
Создаёт ветку, в которую и произойдет переключение:

git checkout -b some-other-new-branch
Если в текущей ветке были какие-то изменения по сравнению с последним коммитом в ветке(HEAD), то команда откажется производить переключение, дабы не потерять произведенную работу. Проигнорировать этот факт позволяет ключ -f:

git checkout -f some-other-branch
В случае, когда изменения надо все же сохранить, следует использовать ключ -m. Тогда команда перед переключением попробует залить изменения в текущую ветку и, после разрешения возможных конфликтов переключиться в новую:

git checkout -m some-other-branch
Вернуть файл (или просто вытащить из прошлого коммита) позволяет команда вида:

git checkout somefile
Возвращает somefile к состоянию последнего коммита:

git checkout somefile
Возвращает somefile к состоянию на два коммита назад по ветке:

git checkout HEAD~2 somefile
git merge — слияние веток, разрешение возможных конфликтов
Слияние веток, в отличие от обычной практики централизованных систем, в git происходит практически каждый день. Естественно, что имеется удобный интерфейс к популярной операции.

Пытается объединить текующую ветку и ветку new-feature:

git merge new-feature
В случае возникновения конфликтов коммита не происходит, а по проблемным файлам расставляются специальные метки а-ля svn; сами же файлы отмечаются в индексе как «не соединенные» (unmerged). До тех пор пока проблемы не будут решены, коммит совершить будет нельзя.

Например, конфликт возник в файле TROUBLE, что можно увидеть в git status.

Произошла неудачная попытка слияния:

git merge experiment
Смотрим на проблемные места:

git status
Разрешаем проблемы:

edit TROUBLE
Индексируем наши изменения, тем самым снимая метки:

git add .
Совершаем коммит слияния:

git commit
Вот и все, ничего сложного. Если в процессе разрешения вы передумали разрешать конфликт, достаточно набрать (это вернёт обе ветки в исходные состояния):

git reset --hard HEAD
Если же коммит слияния был совершен, используем команду:

git reset --hard ORIG_HEAD
git rebase — построение ровной линии коммитов
Предположим, разработчик завел дополнительную ветку для разработки отдельной возможности и совершил в ней несколько коммитов. Одновременно по какой-либо причине в основной ветке также были совершены коммиты: например, в нее были залиты изменения с удаленного сервера, либо сам разработчик совершал в ней коммиты.

В принципе, можно обойтись обычным git merge. Но тогда усложняется сама линия разработки, что бывает нежелательно в слишком больших проектах, где участвует множество разработчиков.

Предположим, имеется две ветки, master и топик, в каждой из которых было совершенно несколько коммитов начиная с момента ветвления. Команда git rebase берет коммиты из ветки topic и накладывает их на последний коммит ветки master.

Вариант, в котором явно указывается, что и куда накладывается:

git-rebase master topic
на master накладывается активная в настоящий момент ветка:

git-rebase master
После использования команды история становится линейной. При возникновении конфликтов при поочередном накладывании коммитов работа команды будет останавливаться, а в проблемные местах файлов появятся соответствующие метки. После редактирования, разрешения конфликтов, файлы следует внести в индекс командой:

git add
и продолжить наложение следующих коммитов командой:

git rebase --continue
Альтернативными выходами будут команды пропустить наложение коммита и перейти к следующему:

git rebase --skip 
и отмена работы команды и всех внесенных изменений:

git rebase --abort 
С ключом -i (--interactive) команда будет работать в интерактивном режиме. Пользователю будет предоставлена возможность определить порядок внесения изменений, автоматически будет вызывать редактор для разрешения конфликтов и так далее.

...... И много чего еще)

7. Что такое ветки и для чего они нужны при работе с системой контроля версий.
8. Базовая работа с ветками в git.
9. Что такое удаленный репозиторий и для чего он нужен
10. Базовая работа с удаленными репозиториями GitHub
11. Как строится и для чего нужна совместная работа в системах контроля версий
12. Инструкция по созданию pull request
13. Книги и полезные ссылки по изучению git.
14. Альтернативные системы контроля версий.