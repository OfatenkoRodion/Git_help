# Git_help

```git checkout -f``` Очистка всех незакомментированных изменений 

```git config —list —show-origin``` просмотр конфига

```git commit --amend -m "New commit message"``` обновление сообщения в коммите

```git reset HEAD~``` Отменить последний коммит, но сохранить изменения

```merge vs rebase```
Чтобы понять когда нужно использовать merge и rebase, нужно (внезапно) понять как они работают и что делают.

Merge - это средство интеграции изменений из двух (и даже более) последовательностей изменений. Когда вы хотите сказать - я беру изменения из своей ветки и из другой, и совмещаю их друг с другом - вам нужно делать merge. Слияние не изменяет имеющуюся историю изменений, а наоборот, продолжает её. Создавая мёрж-коммит вы как бы говорите всем - "я совместил изменения из этих двух веток, все кому нужны изменения из этих обоих веток ОДНОВРЕМЕННО (например, из ветки feature_buy_button и ветки master) - можете взять этот коммит". Вы говорите это в том числе и самому себе, когда подмёрживаете мастер себе в ветку разработки - после мёржа вы знаете что все ранее сделанные изменения в вашей ветке и мастер-ветке теперь живут вместе.

Rebase - это средство автоматического или полуавтоматического редактирования истории изменений. Вернее даже не редактирования, нет. Это средство СОЗДАНИЯ новой последовательности изменений на основе имеющейся. А теперь давайте подумаем, когда это бывает нужно:
вы упороли в каком-то коммите, сделанном в начале недели, оставили в правках пароль/приватный ключ и т.п., но не успели залить ветку в публичное место. Хочется пароль удалить, но все остальные коммиты (которых уже 20 штук набралось с тех пор) оставить как есть.
вас попросили убрать из ветки некоторые обособленные изменения. Вы понимаете, что вполне могли бы выбросить пару коммитов как раз с этими изменениями;
у вас в компании стандарт на формат коммит-месседжей, но вы сделали пару неудачных сообщений в вашей ветке и вам нужно их поправить;
вам нужно поправить имя/почту в ваших коммитах.

В то же время, мы стараемся не использовать и даже вовсе запрещаем ребейз, если история стала коллективной. Что это значит? Ну например:
от бранча был сделан ещё один бранч и над ним работает кто-то другой;
изменения в бранче начали проходить код-ревью (т.к. комментарии к коду обычно связаны с коммитами, формирование новой истории приводит к "отваливанию" комментариев).



```git stash```


Довольно часто при работе с git возникает ситуация, когда необходимо обновиться (сделать pull), но при этом коммитить сырой код совсем не хочется. На помощь спешит команда git stash, которая скрывает все сделанные изменения и переводит код в состояние HEAD. После чего можно сделать pull, а дальше уже накатить изменения до этого спрятанные.

Делается это следующим образом:

git stash

git pull

git stash apply

и можно продолжать работать.

Возможные варианты опций при работе с командой stash:

git stash apply - применить изменения к текущей версии

git stash list - вывести список изменений

git stash show - вывести последние изменния

git stash drop - удалить последние изменения в списке 

git stash pop - [apply] + [drop]

git stash clear - очистить список изменений

Если сравнить с SVN, то  git stash чем-то похож на svn patch, только тут мы еще код возвращаем в исходную версию, а потом уже можно применить этот патч.


```склеить последние n коммитов в 1```
 Reset the current branch to the commit just before the last 12:

git reset --hard HEAD~12

HEAD@{1} is where the branch was just before the previous command.

This command sets the state of the index to be as it would just

after a merge from that commit:

git merge --squash HEAD@{1}

Commit those squashed changes.  The commit message will be helpfully

prepopulated with the commit messages of all the squashed commits:

git commit

```--squash```
Обрабатывает рабочую область и индекс таким образом, как будто было произведено слияние (merge), но при этом не делает фактического коммита, не перемещает указатель HEAD и не записывает ничего в $GIT_DIR/MERGE_HEAD. Это позволяет сделать единый коммит к текущей ветке, содержащий все те же изменения, которые были бы применены при обычном слиянии с веткой (или несколькими, в случае сложного слияния).

Как это работает
git checkout master
git merge --squash feature123
git commit -m'merged feature #123'
Все изменения в ветке feature123 становятся одним коммитом в ветке master.

Это удобно, если ветка содержит много незначительных коммитов, которые «неинтересны» для общей истории. После такой операции история ветки останется «плоской», так же как после git pull --rebase.
