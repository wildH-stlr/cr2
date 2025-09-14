1.Чем отличаются git commit и git commit --amend? Когда --amend опасен?

 без --amend создается новый коммит с текущими проиндексированными изменениями, а с --amend заменяется самый последний коммит новым коммитом с всякиси изменениями
 опастность --amend в том что если последний коммит уже есть в удаленном репозитории, то использование --amend повлечет за собой конфликты

2.Что делает git branch -d и чем отличается от -D?
 -d удаляет ветку безопасно, если она уже слита в удаленный репозиторий и смержена
 -D прост делитает ее без предупреждений и прочего

3.Для чего используют git checkout <branch> и git checkout <commit>? Что такое «detached HEAD»?

 1 переход на ветку, 2 переход на определенный коммит
 состояние в котором голова(HEAD) указывает именно на коммит 

4.В чём разница между reset --soft, --mixed (по умолчанию) и --hard?

 --soft сбрасывает HEAD на указанный коммит сохраняя все изменения(тут всё понятно)
 --mixed сбрасывает на указанный коммит, обновляет индекс, но не трогает файлы в рабочем каталоге(изменения остаюстся, но они неиндексированные)
 --hard сбрасывет на указанный коммит, обновляет индекс и -все изменения(тут прост гг, считай откат. делает типочек чет, изменяет и  пока не закоммитил, а ты ему вот это вводишь и сидишь на +морали)

5.Зачем нужен git revert, и чем он принципиально отличается от git reset?

 1 +коммит который отменяет изменения предыдущего и не меняет историю. 2 менят всё, от истории до файлов в каталоге перемещая ветку на другой коммит.

6.Что делает git merge --no-ff? Когда уместен --squash?

 1 сохраняет историю веток и добавляет коммит слияния. уместен для объединения всех изменений из ветки в один коммит без создания коммита слияния(кароч если история не важна и видеть отдельные ветки не надо можешь юзать)

7.Что происходит при git rebase <base>? Как безопасно прервать ребейз?

 переписывает коммиты текущей ветки и применяет их на основании <base>
 чтобы прервать ребейз надо сделать аборт(--abort) он отменяет процесс ребейза и восстанавливает первоначальное состояние ветки

8.Как перенести 3 конкретных коммита на текущую ветку с помощью cherry-pick? Что дают флаги -n, -x, -e?

 для переноса коммитов на текущую ветку с помощью cherry-pick надо прописать их хеши. флан -n применяет изменения в рабочий каталог и индекс, но не создает коммит. -x добавляет в сообщение нового коммита строку с указанием хеша исходного коммита
 -e открывает редактор для редактирования сообщения коммита перед его созданием

9.Как отменить конфликтный cherry-pick/merge/rebase?
 git то что мы хотим отменить --abort

10.Что такое «родитель» для merge-коммита и зачем нужен git revert -m?

 родители коммита это ветки которые он объединяет
 для корректного отката коммита с выбором какого родителя брать за основу для отмены изменений

ЧАСТЬ B:

git checkout -b feature/header
git branch -m feature/header feat/header
git branch -d feature/header
git branch -vv
echo "header section" >> app.txt 
git add app.txt 
git commit -m "h1: add header" 
git checkout main 
echo "footer section" >> app.txt
git commit -m "c3: add footer"
git add app.txt
git add answers.md
git log --oneline --decorate --graph --all -- app.txt
merge --no-ff feat/header -m "merge: bring header"
git add answers.md app.txt
git commit -m "Save local changes before merge"
git merge --no-ff feat/header
git log --oneline --graph --decorate 
git revert HEAD -m "revert: remove footer"
git reflog
git reset --hard a8a35eb 
c98a639 (HEAD -> main) HEAD@{0}: reset: moving to HEAD^
a8a35eb HEAD@{1}: commit (merge): Save local changes before merge
c98a639 (HEAD -> main) HEAD@{2}: commit: Save local changes before merge
c4ac381 (tag: c2) HEAD@{3}: merge feat/header: updating HEAD
c4ac381 (tag: c2) HEAD@{4}: merge feat/header: updating HEAD
c4ac381 (tag: c2) HEAD@{5}: checkout: moving from feat/header to main
c8b835c (feat/header) HEAD@{6}: commit: h1: add header
fc16e96 (tag: c1) HEAD@{7}: checkout: moving from feat/header to feat/header
fc16e96 (tag: c1) HEAD@{8}: Branch: renamed refs/heads/feature/header/promo to refs/heads/feat/header
fc16e96 (tag: c1) HEAD@{10}: checkout: moving from main to feature/header/promo
c4ac381 (tag: c2) HEAD@{11}: checkout: moving from feature/promo to main
1e9af35 (tag: p1, feature/promo) HEAD@{12}: commit: p1: add promo banner
fc16e96 (tag: c1) HEAD@{13}: checkout: moving from feature/login to feature/promo
8eafd5e (tag: f2, feature/login) HEAD@{14}: commit: f2: login validation
1b91ebb HEAD@{15}: commit: f1: add login block
c4ac381 (tag: c2) HEAD@{16}: checkout: moving from feature/menu to feature/login
229e497 (tag: m1, feature/menu) HEAD@{17}: commit: m1: menu greeting
fc16e96 (tag: c1) HEAD@{18}: checkout: moving from main to feature/menu
c4ac381 (tag: c2) HEAD@{19}: commit: c2: tweak greeting on main
fc16e96 (tag: c1) HEAD@{20}: commit (amend): c1:initial app
a109178 HEAD@{21}: commit (initial): c1:initial app
git merge feature/menu
git status
git commit -m "merge: accept menu greeting" 
cat app.txt            
Hello from main
menu greeting accepted

