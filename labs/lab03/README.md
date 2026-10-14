<p align="center"><img src="../../assets/course-cover.svg" alt="СКВ — лабораторные работы" width="100%" /></p>

# СКВ · Лабораторная 03

Работа с удалёнными репозиториями. Совместная разработка в GitLab.

[К сборнику](../../README.md) · [Исходный проект](https://gitlab.com/akkariq/collab-project) · [Форк](https://gitlab.com/ivannikov-skv/collab-project) · [MR !1](https://gitlab.com/akkariq/collab-project/-/merge_requests/1) · [MR !2](https://gitlab.com/akkariq/collab-project/-/merge_requests/2)

| 🍴 Проекта (источник и форк) | 🔀 Слитых MR | 💬 Контрольные вопросы |
|:---:|:---:|:---:|
| 02 | 02 | 10 |

## СКВ · Паспорт работы

| Поле | Значение |
|---|---|
| Дисциплина | Системы управления/контроля версиями (в методичке: «Системы контроля версий», СКВ) |
| Тема работы | Работа с удалёнными репозиториями. Совместная разработка в GitLab |
| Студент | Иванников Сергей Сергеевич |
| Группа | Пин-б-о-24-1(2) |
| Преподаватель | Щеголев Алексей Алексеевич |
| Методичка | 0302_lab.md |
| Выполнение | 14 октября 2026 года |
| Статус | Практика выполнена и проверена; оценка преподавателя не заявляется |

## СКВ · Оглавление

- [Цель и основания](#scope)
- [Часть 1. Источник, форк и клонирование](#part1)
- [Часть 2. MR, ревью и защита main](#part2)
- [Часть 3. Самостоятельное задание](#part3)
- [Уточнения и оговорки](#notes)
- [Итоговая проверка](#verification)
- [Контрольные вопросы](#questions)
- [Общее заключение](#conclusion)

<a id="scope"></a>
## СКВ · Цель и основания

Цель — освоить работу с удалёнными репозиториями (remote, fetch, pull, push), модель Fork + Merge Request, одновременную работу с origin и upstream, процесс Code Review и защиту веток в GitLab. Основание: методичка 0302_lab.md, выводы терминала студента и проверки состояния GitLab через API. Название дисциплины в паспорте — формулировка студента; в методичке используется «Системы контроля версий». Три части — структура отчёта, а не изменение структуры практики.

Терминал Linux с Bash, Git 2.47.3 (подтверждён в лабораторной №1), SSH-аутентификация GitLab для akkariq. Методичка называет Debian 11/12; точная версия ОС в переписке не установлена и не утверждается. Проект collab-project не связан с репозиторием первых двух работ и не заменяет его. Секреты, адрес электронной почты и скриншоты, которых не было, в отчёт не включены. Word-версия выдана отдельно и здесь не размещена.

| Роль | Проект / папка | Примечание |
|---|---|---|
| Исходный проект (upstream) | akkariq/collab-project | ID 87614612, Public, ветка main |
| Форк (origin) | ivannikov-skv/collab-project | ID 87614914, Private, создан из исходного проекта |
| Локальный клон источника | ~/tp/collab-source | использован для создания CONTRIBUTORS.md |
| Локальный клон форка | ~/tp/collab-fork | origin → форк, upstream → источник |

<a id="part1"></a>
## СКВ · Часть 1. Источник, форк и клонирование

### СКВ · Исходный проект

Исходный проект создан студентом в браузере: Create blank project, имя и slug collab-project, пространство akkariq, видимость Public, включена инициализация README. Через GitLab подтверждено: Project ID 87614612, ветка по умолчанию main, форки разрешены. Исходный коммит — b9f4025.

<details>
<summary>▸ Клонирование источника и CONTRIBUTORS.md</summary>

```text
git clone git@gitlab.com:akkariq/collab-project.git collab-source
cd collab-source
printf '# Участники проекта\n\n| Имя | Роль |\n|-----|------|\n| akkariq | Владелец исходного проекта |\n' > CONTRIBUTORS.md
git add CONTRIBUTORS.md
git commit -m "docs: add CONTRIBUTORS.md"
git push origin main
```

Пример методички использовал вымышленного участника; в начальную таблицу внесён реальный владелец источника. Коммит aa5288b подтверждён через GitLab, автор — Сергей Иванников.

</details>

### СКВ · Fork и настройка remotes

Форк создан в браузере в группе ivannikov-skv. Через GitLab подтверждено: Project ID 87614914, Private, форк связан с akkariq/collab-project, в нём одна ветка main на коммите aa5288b. Видимость группы не менялась.

<details>
<summary>▸ Клонирование форка и добавление upstream</summary>

```text
git clone git@gitlab.com:ivannikov-skv/collab-project.git collab-fork
cd collab-fork
git remote add upstream git@gitlab.com:akkariq/collab-project.git
git remote -v
```

Фактический результат: origin — git@gitlab.com:ivannikov-skv/collab-project.git, upstream — git@gitlab.com:akkariq/collab-project.git, оба с пометками fetch и push. Рабочий каталог чистый, история — b9f4025 и aa5288b.

</details>

### СКВ · Ветка feature/add_contributor

<details>
<summary>▸ Команды и итоговый CONTRIBUTORS.md</summary>

```text
git switch -c feature/add_contributor
printf '| Иванников Сергей Сергеевич | Разработчик |\n' >> CONTRIBUTORS.md
printf '# Мой вклад в проект\n\nДобавил себя в список участников.\n' > MY_CONTRIBUTION.md
git add CONTRIBUTORS.md MY_CONTRIBUTION.md
git commit -m "docs: add self to contributors"
git push -u origin feature/add_contributor
```

Итоговый файл:

```text
# Участники проекта

| Имя | Роль |
|-----|------|
| akkariq | Владелец исходного проекта |
| Иванников Сергей Сергеевич | Разработчик |
```

Коммит 19e3391 подтверждён в форке через GitLab; его родитель — aa5288b.

</details>

<a id="part2"></a>
## СКВ · Часть 2. MR, ревью и защита main

### СКВ · Merge Request !1

Первая попытка создала MR внутри форка (цель — main форка). Он закрыт без слияния, это зафиксировано как [закрытый ошибочный MR форка](https://gitlab.com/ivannikov-skv/collab-project/-/merge_requests/1). Затем студент создал MR из `ivannikov-skv/collab-project:feature/add_contributor` в `akkariq/collab-project:main`. Название — docs: add self to contributors; Closes #1 не использовалось.

[MR !1 источника](https://gitlab.com/akkariq/collab-project/-/merge_requests/1) слит студентом в браузере: статус merged, merge-коммит 1a56c1e с родителями aa5288b и 19e3391, конфликтов нет. Исходная ветка в форке сохранена, squash не применялся.

### СКВ · Code Review

Ревью другим человеком не выполнено: ревьюера нет, преподаватель комментарий не оставлял. В MR нет ревьюеров, комментариев и одобрений. Замечания и исправления по ревью не выдумывались; коммит fix: correct contributors table formatting из примера методички не создавался.

### СКВ · Синхронизация и защита main

<details>
<summary>▸ Команды синхронизации</summary>

```text
git fetch upstream
git merge --ff-only upstream/main
git push origin main
```

Фактический вывод: Fast-forward aa5288b..1a56c1e, затем push в origin прошёл. Локальная main, origin/main и upstream/main совпали.

</details>

Защита main источника настроена в Settings → Repository → Protected branches. Значения по скриншоту студента (скриншот в отчёт не встроен):

| Параметр | Значение |
|---|---|
| Ветка | main (default) |
| Allowed to merge | Maintainers |
| Allowed to push and merge | No one |
| Allowed to force push | выключено |

GitLab API подтвердил protected: true и отсутствие прав push и merge у Developers. Конкретные роли Maintainers и No one взяты со скриншота.

<a id="part3"></a>
## СКВ · Часть 3. Самостоятельное задание

Задание выполнено в том же форке и источнике; Fork и upstream повторно не создавались, так как они уже настроены.

<details>
<summary>▸ Ветка feature/calculator и calc.py</summary>

```text
git switch main
git fetch upstream
git switch -c feature/calculator
printf 'def add(a, b):\n    return a + b\n' > calc.py
python3 -c "from calc import add; print(add(2, 3))"
git add calc.py
git commit -m "feat: add calculator add function"
git push -u origin feature/calculator
```

Файл calc.py:

```python
def add(a, b):
    return a + b
```

Локальная проверка add(2, 3) вернула 5. Коммит caabe97 подтверждён в форке; его родитель — 1a56c1e.

</details>

[MR !2](https://gitlab.com/akkariq/collab-project/-/merge_requests/2) создан из форка в main источника и слит студентом: merge-коммит e5e4b45, родители 1a56c1e и caabe97. Diff содержал единственный новый файл calc.py из двух строк. В названии MR на сервере в конце стоит точка («feat: add calculator add function.»), в названии коммита её нет. Ветка feature/calculator в форке сохранена, squash не применялся. После слияния main форка совпала с источником по коммиту e5e4b45.

Задание «комментарий к MR другого студента» не выполнено: другого студента нет. Исправления по замечаниям не применялись.

<details>
<summary>▸ history.txt (локальный файл, 10 строк)</summary>

```text
git log --oneline --graph --all > history.txt
```

Содержимое файла по выводу терминала:

```text
*   e5e4b45 Merge branch 'feature/calculator' into 'main'
|\  
| * caabe97 feat: add calculator add function
|/  
*   1a56c1e Merge branch 'feature/add_contributor' into 'main'
|\  
| * 19e3391 docs: add self to contributors
|/  
* aa5288b docs: add CONTRIBUTORS.md
* b9f4025 Initial commit
```

</details>

<a id="notes"></a>
## СКВ · Уточнения и оговорки

1. Один человек выполнил обе роли: исходный проект принадлежит аккаунту akkariq, форк находится в группе ivannikov-skv. Это техническая модель source/fork. Реального взаимодействия с другим студентом не было.
2. Code Review другим человеком не выполнен: ревьюера и преподавателя для комментария на момент выполнения не было. Оба MR слиты без внешнего ревью (reviewers пусты, комментариев нет, approvals не требовались). Задание 6 самостоятельной работы («комментарий к MR другого студента») поэтому не выполнено, а задание 7 («исправить замечания») не применимо.
3. Первый MR был ошибочно создан внутри форка (форк → main форка, MR !1 форка). Он закрыт без слияния. Затем создан корректный MR из форка в main источника. Нумерация: MR !1 и !2 источника — рабочие; MR !1 форка — закрытый ошибочный.
4. В описании MR из методички есть строка Closes #1. В источнике задача №1 не создана, поэтому строка не использовалась: она могла бы закрыть неподходящую задачу.
5. Пример ревью в методичке говорит о трёх колонках таблицы, но CONTRIBUTORS.md двухколоночный. Искусственная ошибка форматирования не создавалась.
6. В примерах методички фигурируют вымышленные участники (Иванов Иван, Сидоров Сидор). В работе использованы реальные данные: akkariq как владелец и Иванников Сергей Сергеевич как разработчик.
7. Для синхронизации применён git merge --ff-only upstream/main вместо git merge upstream/main: результат тот же (fast-forward), но при расхождении история не получит случайного merge-коммита.
8. После push GitLab выводит ссылку для создания MR, которая по умолчанию ведёт в форк. Для корректного MR цель была указана вручную — проект akkariq/collab-project, ветка main.
9. Защита main настроена по методичке: слияние — Maintainers, прямой push — No one. Требование одобрения владельцев кода и успешного CI не применялось: CI в проекте не настроен. Проверка защиты практической попыткой push не выполнялась.
10. history.txt создан командой из методички и остаётся локальным файлом в ~/tp/collab-fork, не закоммичен: прямой push в защищённую main источника закрыт, а отдельный MR ради файла истории не создавался. При перенаправлении в файл git log не выводит пометки веток в скобках.
11. Папка __pycache__ появилась локально после проверки импорта calc.py; в коммиты не добавлялась.
12. Успешное прохождение CI/CD и оценка преподавателя не заявляются. VS Code опционален и отдельно не подтверждён.

<a id="verification"></a>
## СКВ · Итоговая проверка

| Коммит | Сообщение | Роль |
|---|---|---|
| b9f4025 | Initial commit | Начальный README при создании источника |
| aa5288b | docs: add CONTRIBUTORS.md | Начальный список участников в main источника |
| 19e3391 | docs: add self to contributors | Ветка feature/add_contributor: строка студента и MY_CONTRIBUTION.md |
| 1a56c1e | Merge branch 'feature/add_contributor' into 'main' | Слияние MR !1 в источнике |
| caabe97 | feat: add calculator add function | Ветка feature/calculator: calc.py с функцией add(a, b) |
| e5e4b45 | Merge branch 'feature/calculator' into 'main' | Слияние MR !2 в источнике |

Состояние на проверке: main источника и main форка указывают на e5e4b45. В форке есть ветки main, feature/add_contributor (19e3391) и feature/calculator (caabe97). Локальный git status: ветка main соответствует origin/main; неотслеживаемые файлы — history.txt и __pycache__/. В полной достижимой истории main источника шесть коммитов: начальный, CONTRIBUTORS.md, коммит ветки участника, два merge-коммита и коммит калькулятора.

<a id="questions"></a>
## СКВ · Контрольные вопросы

<details>
<summary>1. Что такое форк репозитория и зачем он нужен?</summary>

Форк — личная копия проекта в другом пространстве имён GitLab, которая сохраняет связь с исходным проектом. Он нужен, чтобы вносить изменения без прав записи в источник и затем предлагать их через Merge Request. В работе форк ivannikov-skv/collab-project создан из akkariq/collab-project.

[Документация](https://docs.gitlab.com/user/project/repository/forking_workflow/)

</details>

<details>
<summary>2. В чём разница между git fetch и git pull?</summary>

git fetch загружает новые коммиты из удалённого репозитория и обновляет удалённые ссылки (например, upstream/main), но не меняет текущую ветку и рабочий каталог. git pull выполняет fetch и затем сразу объединяет изменения с текущей веткой (merge или rebase), поэтому возможны конфликты. В работе сначала выполнялся fetch upstream, а затем контролируемое слияние --ff-only.

[Документация](https://about.gitlab.com/blog/git-pull-vs-git-fetch-whats-the-difference/)

</details>

<details>
<summary>3. Зачем добавлять upstream при работе с форком?</summary>

origin указывает на форк, куда студент может отправлять ветки. Remote upstream указывает на исходный проект, из которого нужно получать чужие изменения. Без него локальная копия не знает об обновлениях источника, и синхронизация форка из командной строки невозможна. Здесь upstream — git@gitlab.com:akkariq/collab-project.git.

[Документация](https://docs.gitlab.com/topics/git/forks/)

</details>

<details>
<summary>4. Что такое Merge Request (Pull Request) и какова его роль в совместной разработке?</summary>

Merge Request — запрос на включение изменений одной ветки в другую, в том числе из форка в исходный проект. Он показывает diff, коммиты и обсуждение, служит точкой ревью и проверок, а слияние выполняет пользователь с нужными правами. Pull Request — аналогичный термин GitHub.

[Документация](https://docs.gitlab.com/user/project/merge_requests/)

</details>

<details>
<summary>5. Какие этапы проходит MR от создания до слияния?</summary>

Ветка и коммиты → отправка ветки в форк → создание MR с указанием проектов и веток источника и цели, заголовком и описанием → обсуждение и Code Review → исправления новыми коммитами в ту же ветку, которые автоматически попадают в MR → проверки (CI, одобрения), если настроены → слияние выбранным методом → синхронизация локального репозитория и форка. В работе этапы ревью и проверок отсутствовали (см. уточнения).

[Документация](https://docs.gitlab.com/user/project/merge_requests/)

</details>

<details>
<summary>6. Что такое Code Review и зачем он нужен?</summary>

Это просмотр изменений другим участником до слияния. Ревью помогает найти ошибки и нарушения стиля, улучшить решение и распространить знания о коде. В GitLab замечания оставляют к строкам diff, автор исправляет их и отвечает в тредах. В данной работе ревью другим человеком не выполнялось.

[Документация](https://docs.gitlab.com/user/project/merge_requests/reviews/)

</details>

<details>
<summary>7. Как обновить форк, если исходный репозиторий изменился?</summary>

Если форк отстаёт, в интерфейсе GitLab можно нажать «Update fork». Из командной строки: переключиться на main, выполнить git fetch upstream, подтянуть изменения (git pull upstream main или git merge --ff-only upstream/main) и отправить результат в форк командой git push origin main. Если форк опережает источник, изменения передаются через MR. В работе использован вариант с fetch и --ff-only.

[Документация](https://docs.gitlab.com/user/project/repository/forking_workflow/)

</details>

<details>
<summary>8. Что такое защищённые ветки (Protected Branches) и зачем они нужны?</summary>

Это ветки с ограничениями: роли определяют, кто может в них пушить и сливать, а также можно запретить force-push. Такая защита не даёт случайно или намеренно переписать основную ветку и направляет изменения через Merge Request. Настраивают защиту в Settings → Repository, для этого нужна роль Maintainer или Owner. В работе main источника: слияние — Maintainers, прямой push — No one.

[Документация](https://docs.gitlab.com/user/project/repository/branches/protected/)

</details>

<details>
<summary>9. В чём разница между методами слияния: «Merge commit», «Squash commits», «Rebase and merge»?</summary>

Merge commit всегда создаёт merge-коммит и сохраняет все коммиты ветки; это метод по умолчанию. Squash объединяет коммиты ветки в один перед слиянием. Rebase при слиянии переносит коммиты ветки поверх цели и даёт линейную историю; в настройках проекта GitLab это соответствует вариантам Fast-forward merge и Merge commit with semi-linear history. В работе оба MR слиты как merge commit без squash.

[Документация](https://docs.gitlab.com/user/project/merge_requests/methods/)

</details>

<details>
<summary>10. Как добавить нескольких участников в GitLab-проект?</summary>

Пользователь с ролью Maintainer или Owner открывает проект, выбирает Manage → Members → Invite members, вводит имя пользователя GitLab (или e-mail, если аккаунта нет), выбирает роль, при необходимости срок доступа и нажимает Invite. Для нескольких участников действие повторяют или используют группу проекта. Названия меню зависят от версии GitLab. В работе участники не добавлялись.

[Документация](https://docs.gitlab.com/user/project/members/)

</details>

<a id="conclusion"></a>
## СКВ · Общее заключение

Подтверждены создание исходного проекта, форк, клонирование с настройкой origin и upstream, ветка feature/add_contributor, два MR из форка в источник (слиты как merge commit без squash и без удаления исходных веток), синхронизация форка через fetch и fast-forward, защита main источника и сохранение history.txt. Исходный проект и форк находятся на одном коммите e5e4b45. Не выполнены Code Review другим человеком и связанные с ним замечания и исправления: это зафиксировано как оговорка. Прохождение CI/CD и оценка преподавателя не заявляются.
