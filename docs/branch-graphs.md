# СКВ · Графы ветвления

[← К сборнику](../README.md) · [Отчёт №2](../labs/lab02/README.md) · [Отчёт №3](../labs/lab03/README.md)

Схемы построены вручную по подтверждённым хешам из отчётов. Они показывают упрощённую логику истории, а не точный вывод `git log`: порядок и расположение коммитов приближённые, ветки с названием из примера Mermaid не заменяют реальный граф на GitLab.

## СКВ · Лабораторная №2

Ветки оплаты и уведомлений слиты в main, затем ветка базы данных (в состоянии после rebase поверх `bcde007`) слита через MR !1, а релизная ветка — через MR !2. Дальше на main выполнены учебные фичи, два revert, исправление и fast-forward третьей фичи.

```mermaid
gitGraph
   commit id: "8052a3a"
   commit id: "c1f4071"
   commit id: "e9986a8"
   commit id: "c2de54e"
   branch feature/payment
   checkout feature/payment
   commit id: "b07f340"
   checkout main
   branch feature/notifications
   checkout feature/notifications
   commit id: "41f6c15"
   checkout main
   merge feature/payment id: "e4ac89c"
   merge feature/notifications id: "e60ad88"
   commit id: "bcde007"
   branch feature/database
   checkout feature/database
   commit id: "535861c"
   checkout main
   merge feature/database id: "a2c482c"
   branch release/v1.0
   checkout release/v1.0
   commit id: "a138c69"
   checkout main
   merge release/v1.0 id: "f23815f"
   commit id: "1264ae1"
   commit id: "7a5e255"
   commit id: "5e10e6e"
   commit id: "8122edd"
   commit id: "638508a"
   commit id: "c25db92"
   commit id: "e7ed679"
   commit id: "440b925"
```

Коммит `dbc966c` до rebase и `ef57499` до второго rebase в схему не включены: они заменены `535861c` и `e7ed679`. Ветка `release/lab2-final` указывает на `440b925`.

## СКВ · Лабораторная №3

История основной ветки исходного проекта `akkariq/collab-project`. Feature-ветки лежали в форке `ivannikov-skv/collab-project` и вливались в источник через два Merge Request; Code Review другим человеком не выполнялся.

```mermaid
gitGraph
   commit id: "b9f4025"
   commit id: "aa5288b"
   branch feature/add_contributor
   checkout feature/add_contributor
   commit id: "19e3391"
   checkout main
   merge feature/add_contributor id: "1a56c1e"
   branch feature/calculator
   checkout feature/calculator
   commit id: "caabe97"
   checkout main
   merge feature/calculator id: "e5e4b45"
```

| Этап | Где происходит | Результат |
|---|---|---|
| Fork | GitLab, группа ivannikov-skv | Форк ID 87614914 |
| Ветка и push | Форк, origin | `19e3391`, затем `caabe97` |
| MR !1 и !2 | Из форка в `main` источника | Merge-коммиты `1a56c1e` и `e5e4b45` |
| Синхронизация | `fetch upstream`, `merge --ff-only`, `push origin main` | Источник и форк на `e5e4b45` |
