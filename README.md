# Курсовой репозиторий — Инструкция по работе (Fork → Upstream → Ветки → PR)

Этот документ объясняет, как подключиться к проекту, создать свою ветку для задачи, регулярно подтягивать изменения из апстрима и сдавать решение через Pull Request.

## 0) Предварительно: SSH-ключ на macOS

```bash
# есть ли ключи
ls -la ~/.ssh

# если нет — создайте
ssh-keygen -t ed25519 -C "your.email@example.com"

# добавьте в агент и свяжите с keychain
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519

# скопируйте публичный ключ и добавьте его в GitHub → Settings → SSH and GPG keys
pbcopy < ~/.ssh/id_ed25519.pub

# проверка
ssh -T git@github.com
```

Также стоит добавить имя и почту GitHub.

```bash
git config --global user.name "<first name> <last name>"
git config --global user.email "your-email@example.com"
```

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

Шаблон имени ветки: `202x-xx-xx-<task_name>/<login>` — один PR = одна задача.

```bash
git switch -c 202x-xx-xx-<task_name>/<login>
git add tasks/202x-xx-xx-<task_name>
git commit -m "202x-xx-xx-<task_name>: initial solution"
git push -u origin 202x-xx-xx-<task_name>/<login>
```

---

## 5) Сдача решения (Pull Request)

PR **внутри своего форка** → в СВОЙ `main`

Откройте Pull Request СО СВОЕЙ ветки **в ваш же `main`**:
- **base:** `main` (ваш форк)
- **compare:** `202x-xx-xx-<task_name>/<login>` (ваша ветка)

В описании PR укажите:
- кратко, что сделано;
- как запустить проверку (команда `pytest -q`);
- при необходимости — скрин/лог тестов.

---

## 6) Как подтягивать новые задания и правки преподавателя

Когда в апстриме появился новый коммит (новые задачи/исправления):

```bash
# обновите вашу main
git checkout main
git pull --rebase upstream main
git push origin main

# обновите свою рабочую ветку
git checkout 202x-xx-xx-<task_name>/<login>
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
## 8) Шпаргалка команд

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
git switch -c 202x-xx-xx-<task_name>/<login>   # создать
git checkout 202x-xx-xx-<task_name>/<login>    # перейти
git rebase main                                # обновить задачную ветку
git push --force-with-lease                    # обновить PR после rebase
```

Удачи и чистой истории коммитов!
