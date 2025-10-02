# Курсовой репозиторий — Инструкция по работе (Fork → Upstream → Ветки → PR)

Этот документ объясняет, как подключиться к проекту, создать свою ветку для задачи, регулярно подтягивать изменения из апстрима и сдавать решение через Pull Request.

---

## 1) Требования
- Git
- Python
- VS Code (расширения: Python, Jupyter, GitLens)
- Homebrew на macOS

> В репозитории запрещены изменения вне папки `solutions/<task-id>/<login>/...`.

---

## 2) Форк и клон

1. Нажмите **Fork** на (странице репозитория)[https://github.com/TEAmofey/AI-tasks].
2. Склонируйте **СВОЙ** форк:
   ```bash
   git clone <url_вашего_форка>
   cd <имя-репозитория>
   ```
3. Добавьте **upstream** (репозиторий преподавателя):
   ```bash
   git remote add upstream https://github.com/TEAmofey/AI-tasks.git
   git remote -v
   ```

---

## 3) Первичная синхронизация с upstream
Перед началом работы всегда синхронизируйтесь:
```bash
git checkout main
git fetch upstream
git pull --rebase upstream main
git push origin main
```

---

## 4) Создание рабочей ветки под задачу

Шаблон имени ветки: `task-XX/<ваш_login>` — один PR = одна задача.

```bash
git switch -c task-XX/<ваш_login>
mkdir -p solutions/task-XX/<ваш_login>
# добавьте/измените файлы в своей папке
git add solutions/task-XX/<ваш_login>
git commit -m "task-XX: initial solution"
git push -u origin task-XX/<ваш_login>
```

---

## 5) Сдача решения (Pull Request)

Откройте PR **из вашей ветки форка** → **в репозиторий преподавателя** → **в ветку `submissions`**.

Рекомендации к PR:
- Заголовок: `task-XX: Фамилия Имя`
- В описании укажите: ОС + версия Python, как запускать, что сделано
- Изменения — только в `solutions/task-XX/<ваш_login>/...`

---

## 6) Как подтягивать новые задания

Когда в апстриме появился новый коммит (новые задачи/исправления):

```bash
# обновите вашу main
git checkout main
git pull --rebase upstream main
git push origin main

# обновите свою рабочую ветку
git checkout task-XX/<ваш_login>
git rebase main
# если были конфликты — решите их, затем:
git add <исправленные_файлы>
git rebase --continue
git push --force-with-lease
```

> Используйте **`--force-with-lease`**, чтобы безопасно обновлять вашу ветку в PR после rebase.

---

## 7) Частые проблемы и решения

**Перепутаны origin/upstream**
```bash
git remote -v
git remote set-url origin   <url_вашего_форка>
git remote set-url upstream https://github.com/TEAmofey/AI-tasks.git
```

**Конфликты при rebase**
```bash
# после редактирования конфликтных файлов
git add <файлы>
git rebase --continue
# отменить текущий rebase
git rebase --abort
```

**Откат незакоммиченных правок**
```bash
git restore <файл>
```

**Откат к прошлому коммиту (осторожно!)**
```bash
git reset --hard HEAD~1
```

---

## 8) Правила

- **Менять можно только** в `solutions/<task-id>/<login>/...`.
- **Одна ветка — одна задача.** Несколько задач в одном PR не принимаются.
- Перед PR всегда делайте `git pull --rebase upstream main`.
- Коммиты должны быть осмысленными, без временных файлов.

---

## 10) Шпаргалка команд

```bash
# статус и история
git status
git log --oneline --graph --decorate

# индексация и коммиты
git add .
git commit -m "msg"

# обновление main из апстрима
git checkout main
git pull --rebase upstream main
git push origin main

# работа с веткой задачи
git switch -c task-XX/<login>   # создать
git checkout task-XX/<login>    # перейти
git rebase main                 # обновить задачную ветку
git push --force-with-lease     # обновить PR после rebase
```

Удачи и чистой истории коммитов!
