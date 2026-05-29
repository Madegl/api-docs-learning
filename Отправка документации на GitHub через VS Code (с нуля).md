> **Цель**: создать репозиторий → загрузить `.md`-файл → создать ветку → внести изменения → сделать Pull Request → слить в `master`.

---

## 🔧 Шаг 0. Установи и настрой Git

1. Скачай и установи Git: https://git-scm.com/download/win  
→ При установке выбери: **«Git from the command line and also from 3rd-party software»**.
2. Перезапусти VS Code.
3. В терминале VS Code проверь:
 git --version
→ Должна отобразиться версия (например, `git version 2.43.0.windows.1`).
4. Укажи своё имя и email (используй тот же email, что в GitHub):
git config --global user.name "Твоё Имя"
git config --global user.email "твой@email.com"

---

## 🌐 Шаг 1. Создай репозиторий на GitHub

1. Зайди на [github.com](https://github.com).
2. Нажми **New repository**.
3. Заполни:
    - **Repository name**: например, `api-docs-practice`
    - **Description**: «Учебная документация по API»
    - ✅ **Public**
    - ❌ **Initialize this repository with a README** → **НЕ ставь галочку!**
4. Нажми **Create repository**.

> 💡 После создания репозитория запомни его URL:  
> `https://github.com/ВАШ-ЛОГИН/api-docs-practice.git`

---

## 📁 Шаг 2. Подготовь папку с файлами

1. Создай папку на компьютере, например:  
    `D:\ObsidianStorage\Практика\api-docs`
2. Скопируй туда один или несколько `.md`-файлов из Obsidian (например, `api-spec.md`).

---

## 💻 Шаг 3. Открой папку в VS Code

1. В VS Code: `File → Open Folder` → выбери папку `api-docs`.
2. Убедись, что видишь свои `.md`-файлы.

---

## 🔄 Шаг 4. Отправь файлы в ветку `master`

В терминале VS Code (внизу):
```powershell
# Инициализировать Git
git init

# Добавить все файлы
git add .

# Сделать коммит
git commit -m "Добавлено описание API"

# Подключить удалённый репозиторий (ЗАМЕНИ ВАШ-ЛОГИН!)
git remote add origin https://github.com/ВАШ-ЛОГИН/api-docs-practice.git

# Отправить в ветку master
git push -u origin master
```
> ✅ После этого твои файлы появятся в ветке `master` на GitHub.

---

## 🌿 Шаг 5. Создать новую ветку и внести изменения

1. В VS Code внизу слева нажми на надпись **`master`**.
2. Выбери **«Create new branch...»** → назови, например: `feature/update-api`.
3. Открой любой `.md`-файл и внеси изменения (например, добавь новый метод API).
4. Сохрани файл (`Ctrl + S`).
5. В левой панели нажми на значок **Source Control** (`⎇`).
6. Нажми `+` (Stage Changes) → введи сообщение коммита:  
    `"Добавлен POST-метод"` → нажми галочку (Commit).
7. Нажми трёхточие (`...`) → **Push** → подтверди публикацию ветки.

---

## 📤 Шаг 6. Создать Pull Request (PR)

1. Зайди на страницу своего репозитория на GitHub.
2. Нажми **"Compare & pull request"** (баннер сверху).
3. Убедись, что:
    - **base:** `master`
    - **compare:** `feature/update-api`
4. Введи заголовок: `"Добавлен POST-метод в API"`
5. Нажми **"Create pull request"** → затем **"Merge pull request"** → **"Confirm merge"**.

✅ Готово! Изменения влиты в `master`.

---

## ℹ️ Полезные примечания

- **Предупреждения о LF/CRLF** — нормальны на Windows, можно игнорировать.
- Если репозиторий уже создан с `main`, а ты хочешь `master`:
```bash
git branch -m main master
git push -u origin master
```
- Все команды работают **только после установки Git и перезапуска VS Code**.