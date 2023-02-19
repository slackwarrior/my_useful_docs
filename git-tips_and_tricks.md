# Rozwiązania problemów z Git'em

## Rozwiązanie problemu rozbieżnych historii

Wspierałem się linkiem: https://komodor.com/learn/how-to-fix-fatal-refusing-to-merge-unrelated-histories-error/
https://isolution.pro/pl/q/so58270290/git-odmawia-scalania-niepowiazanych-historii-co-to-jest-niepowiazane-historie

Wykonałem następujące kroki: 
* Stworzyłem repozytorium w GitHub.
* Stworzyłem katalog, wszedłem do niego, zainicjowałem repozytorium Git
```
$ mkdir BoardTestPeripherals
$ cd BoardTestPeripherals
$ git init
Zainicjowano puste repozytorium Gita w /home/k..../STM32CubeIDE/workspace_1.11.2_nowy/BoardTestPeripherals
```
Następnie standardowo:
```
git remote add origin git@github.com:slackwarrior/L476_Discovery_Test.git
git add .
git commit
git push
```
Rezultatem tej ostatniej komendy jest:
```
$ git push --set-upstream main main
To github.com:slackwarrior/L476_Discovery_Test.git
 ! [rejected]        main -> main (fetch first)
error: nie można wypchnąć niektórych referencji do „github.com:slackwarrior/L476_Discovery_Test.git”
podpowiedź: Aktualizacje zostały odrzucone, ponieważ zdalne repozytorium zawiera
podpowiedź: pracę, której nie ma u ciebie lokalnie. Jest to zwykle spowodowane innym
podpowiedź: repozytorium wypychającym na tę samą referencję. Możesz najpierw chcieć
podpowiedź: zintegrować zdalne zmiany (np. „git pull ...”) przed kolejnym wypchnięciem.
podpowiedź: Zobacz szczegóły w „Uwadze o przewijaniu” w „git push --help”.
```
Co oznacza, że spróbujemy nadpisać historię z repozytorium zdalnego za pomocą repozytorium
lokalnego. Git broni się przed tym powyższą komendą. 
Próbujemy więc od drugiej strony, zgodnie z sugestią: 
```
$ git pull
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
Rozpakowywanie obiektów: 100% (3/3), 858 bajtów | 858.00 KiB/s, gotowe.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Z github.com:slackwarrior/L476_Discovery_Test
 * [nowa gałąź]      main       -> main/main
Bieżąca gałąź nie ma informacji o śledzeniu.
Podaj, z jaką gałęzią scalić.
Więcej szczegółów w git-pull(1).

    git pull <zdalne-repozytorium> <gałąź>

Jeśli chcesz ustawić informacje o śledzeniu w tej gałęzi, możesz to zrobić przez:

    git branch --set-upstream-to=main/<gałąź> main
```
Następnie:
```
$ git pull main main
Z github.com:slackwarrior/L476_Discovery_Test
 * branch            main       -> FETCH_HEAD
podpowiedź: You have divergent branches and need to specify how to reconcile them.
podpowiedź: You can do so by running one of the following commands sometime before
podpowiedź: your next pull:
podpowiedź: 
podpowiedź:   git config pull.rebase false  # merge
podpowiedź:   git config pull.rebase true   # rebase
podpowiedź:   git config pull.ff only       # fast-forward only
podpowiedź: 
podpowiedź: You can replace "git config" with "git config --global" to set a default
podpowiedź: preference for all repositories. You can also pass --rebase, --no-rebase,
podpowiedź: or --ff-only on the command line to override the configured default per
podpowiedź: invocation.
fatal: Należy podać, jak godzić rozbieżne gałęzie.
```
To również nie dało nam pożądanego rezultatu - dalej nie mamy scalonego repozytorium.
Sprobowałem wykonać jeszcze jedną rzecz: założyć osobną gałąź `my_main`, którą najpierw
wypchnę do repozytorium, następnie scalę z gałęzią główną.
Tu również niespodzianka: nie da rady tego dokonać, bo gałęzie są - wedle Git'a - niekompatybilne. 
Znalazłem rozwiązanie, na wyżej wspomnianej stronie. 
Po wydaniu komendy:
```
$ git merge origin/main
fatal: odmawiam scalenia niepowiązanych historii
```

Po dodaniu opcji ` --allow-unrelated-histories`
```
[karol@localhost BoardTestPeripherals]$ git pull origin main --allow-unrelated-histories
Z github.com:slackwarrior/L476_Discovery_Test
 * branch            main       -> FETCH_HEAD
Merge made by the 'ort' strategy.
 .gitignore | 52 ++++++++++++++++++++++++++++++++++++++++++++++++++++
 1 file changed, 52 insertions(+)
 create mode 100644 .gitignore
```
Następnie możemy scalić repozytorium `git merge` poprzez Merge Request na GitHub.


