# Привет, просканируй содержимое документов в @Задание и перенеси их в
_Exported on 6/1/2026 at 03:18:35 GMT+3 from Cursor (3.6.21)_

---

**User**

Привет, просканируй содержимое документов в @Задание и перенеси их в MD формат.

---

**Cursor**

Ищу документы в папке «Задание» и просматриваю их содержимое.


Текст извлечён; улучшаю разметку Markdown — заголовки, списки и структура.


Готово. В папке `Задание` было 4 документа `.docx` — все перенесены в Markdown с нормальной структурой (заголовки, списки, таблицы, блоки кода).

| Исходный файл | Markdown |
|---|---|
| `Домашнее задание AI.docx` | `Домашнее задание AI.md` |
| `Домашнее задание GameMode.docx` | `Домашнее задание GameMode.md` |
| `Домашнее задание GameplayFramework.docx` | `Домашнее задание GameplayFramework.md` |
| `ТЗ Практическое занятие.docx` | `ТЗ Практическое занятие.md` |

**Краткое содержание:**

1. **Домашнее задание AI** — NPC с патрулированием и взаимодействием, противник с охраной/атакой, опционально босс-файт.
2. **Домашнее задание GameMode** — GameMode, GameState, PlayerState, респавн, контрольные точки, условия прохождения уровня.
3. **Домашнее задание GameplayFramework** — старт прототипа игры в UE 5.2: репозиторий, главное меню, уровень, герой, AnimBP, PickableActor, сдача через MR.
4. **ТЗ Практическое занятие** — мини-проект «Domino Run»: меню, ландшафт, физика шара и домино с координатами и критериями приёмки.

Оригинальные `.docx` не удалял — они остались рядом с `.md` файлами. Если нужно убрать docx или собрать всё в один общий README — напиши.

---

**User**

Проанализируй @Задание и предложи планвыполнения.

---

**Cursor**

Перечитаю все задания в папке и составлю связный план выполнения.


# План выполнения заданий

В папке `Задание` — **4 независимых, но связанных блока**. Сейчас в `d:\Unreal_Ptoject` есть только описания; **Unreal-проекта ещё нет** — старт с нуля.

Ниже — порядок работ, зависимости и одна сквозная концепция, чтобы не делать всё дважды.

---

## Общая картина

```mermaid
flowchart TD
    A[ТЗ: Domino Run] -->|отдельный мини-проект| A1[Практика: меню + физика]
    B[ДЗ: GameplayFramework] --> C[ДЗ: AI]
    B --> D[ДЗ: GameMode]
    C --> D
    D --> E[Итоговый прототип игры]
```

| Документ | Роль | Зависимости |
|----------|------|-------------|
| **ТЗ Практическое занятие** | Учебная сцена «Domino Run» | Нет — **отдельный проект** |
| **GameplayFramework** | Фундамент прототипа | Нет |
| **AI** | Поведение NPC и врагов | Нужны уровень, герой, враги (хотя бы mesh) |
| **GameMode** | Игровой цикл и прогресс | Нужны уровень, герой, pickables, враги |

**Рекомендуемый порядок:** Domino Run (параллельно или первым) → GameplayFramework → AI → GameMode.

---

## Рекомендуемая концепция игры (одна на все ДЗ)

Чтобы задания не расходились, подойдёт простой жанр — **3D action/adventure** или **лёгкий шутер**:

> Игрок проходит многоярусный уровень: собирает предметы, открывает дверь ключом, общается с NPC, сражается с охранниками и проходит финальную зону.

Это закрывает требования всех трёх домашних работ одним прототипом.

---

## Этап 0. Подготовка (1–2 дня)

1. Установить **Unreal Engine 5.2** (Blueprint).
2. Завести репозиторий на GitLab, `.gitignore` для UE (без `Binaries/`, `DerivedDataCache/`, `Intermediate/`, `Saved/`).
3. Выбрать тему игры и кратко описать в `README.md` (2–3 абзаца: жанр, цель игрока, win condition).
4. Решить: **Domino Run** — отдельный проект или в том же репо в ветке `practice/domino-run`.

---

## Этап 1. Практическое занятие — «Domino Run» (1–2 дня)

**Отдельный мини-проект**, можно сдать архивом без связи с основным прототипом.

### Шаги

| # | Задача | Критерий готовности |
|---|--------|---------------------|
| 1 | Папки `Content/Maps`, `Blueprints`, `Widgets` | Структура создана |
| 2 | Уровни `MainMenu`, `DominoRun` | Оба сохранены |
| 3 | `WBP_MainMenu`: Start / Quit | Start → `DominoRun`, Quit → выход |
| 4 | Level Blueprint `MainMenu` | UI Only + курсор |
| 5 | Project Settings | Startup Map = `MainMenu` |
| 6 | Landscape на `DominoRun` | Ровная площадка + перепады |
| 7 | Шар + домино по координатам из ТЗ | Цепочка без зависаний |
| 8 | Развилка ±250 по X, шаг 500 | Обе ветки падают |
| 9 | Архив для сдачи | `Content/`, `Config/`, `*.uproject` |

**Совет:** домино лучше дублировать в редакторе и выставлять по формулам из ТЗ; включить **CCD** на всех физических объектах.

---

## Этап 2. GameplayFramework — фундамент (1–2 недели)

База для всех домашних работ. Делать **веткой** `feature/gameplay-framework`.

### 2.1 Инфраструктура

- Blueprint-проект UE 5.2.
- Ветка, push, описание в README.

### 2.2 Главное меню

| Компонент | Что сделать |
|-----------|-------------|
| `Lvl_MainMenu` | Пустой или фоновый уровень |
| `WBP_MainMenu` | «Начать игру», «Выйти», «О игре» |
| `GM_MainMenu` | Свой GameMode для меню |
| Логика | Start → основной уровень; Quit → `Quit Game`; About → overlay с текстом |

### 2.3 Основной уровень

- Отдельный `Lvl_Game` (многоярусный: cubes/ramp/stairs).
- Несколько `PlayerStart` на разных этажах (позже привязать к чекпоинтам).
- Лёгкие Static Mesh, без тяжёлых 4K-ассетов.

### 2.4 Герой

```
BP_PlayerCharacter
├── Movement (WASD + jump)
├── Camera (Third Person)
├── HealthComponent (HP, damage, death event)
└── AnimBP (Idle / Walk / Run)
```

- Манnequin или бесплатный скелет UE.
- Набор анимаций с Marketplace / Mixamo.

### 2.5 PickableActor и взаимодействие

| Тип | Примеры | Поведение |
|-----|---------|-----------|
| Collectable | монеты, аптечка | +HP / +счётчик в GameState (позже) |
| Equipable | оружие | экипировка, смена атаки |
| Quest | ключ, дверь | ключ открывает дверь → прогресс уровня |

- Общий интерфейс `BPI_Interactable` или базовый `BP_PickableActor`.
- Ввод: `E` / raycast / overlap.

### 2.6 Противники (для max оценки)

Пока **без AI** — только заготовка:

- `BP_Enemy_Base` (Character/Pawn)
- Mesh + AnimBP (Idle)
- HealthComponent

### 2.7 Сдача

- Коммит + push.
- MR с Squash, короткое название, описание изменений.
- Ссылка в LMS.

---

## Этап 3. AI (1–1.5 недели)

Ветка `feature/ai`. Опирается на уровень и `BP_Enemy_Base` из этапа 2.

### 3.1 NPC (обязательно)

```
BP_NPC_Friendly
├── AI Controller + Behavior Tree
│   ├── Patrol (Target Points / spline)
│   ├── Detect Player (sphere overlap)
│   ├── Interact (dialog / give item)
│   └── Resume Patrol (after player leaves)
└── Widget или простой dialog BP
```

**Инструменты:** Behavior Tree + Blackboard, или State Machine в Blueprint.

### 3.2 Противник (обязательно)

```
BP_Enemy_Guard
├── Guard zone (Box Trigger)
├── States: Idle/Guard → Chase/Attack → Return
├── Attack (melee или projectile)
└── Timer before returning to guard
```

### 3.3 Звёздочка (опционально)

- `BP_Enemy_Melee` и `BP_Enemy_Ranged` — разные BT/дистанция атаки.

### 3.4 Альтернатива/бонус: босс

- Много HP, 2–3 фазы (BT Switch или фазовый BP).
- Ловушки на уровне наносят урон боссу (overlap + Apply Damage).

**Доп. баллы:** NPC + враги + босс.

---

## Этап 4. GameMode — игровой цикл (1–1.5 недели)

Ветка `feature/gamemode`. Связывает всё в играбельный уровень.

### 4.1 Архитектура

```
GM_Game
├── Spawn player after UI "Start"
├── Checkpoints (PlayerStart tags / BP_Checkpoint)
└── Win/Lose conditions

GS_Game
├── ItemsCollected / KeysFound
├── EnemiesDefeated
└── LevelComplete flag

PS_Player
├── Kills, Deaths, ItemsCollected
└── Show on Tab (WBP_Stats)
```

### 4.2 Механики

| Механика | Реализация |
|----------|------------|
| Старт игры | HUD с кнопкой Start → `RestartPlayer` / unpause |
| Смерть | HealthComponent → respawn at last checkpoint |
| Чекпоинты | Trigger обновляет `LastCheckpoint` в GameMode |
| Прогресс | Pickables → `GameState`; дверь проверяет ключ |
| Tab-статистика | `WBP_PlayerStats` читает `PlayerState` |
| Победа | Условие (ключ + дверь / N предметов / босс) → pause + `WBP_LevelComplete` |

### 4.3 Связка с AI

- Смерть врага → `GameState.EnemiesDefeated++`.
- NPC может выдать ключ/предмет → `GameState`.

---

## Сводный чеклист по приоритетам

### Must have (зачёт)

- [ ] GitLab + UE 5.2 + README
- [ ] Меню (3 кнопки) + переход на уровень
- [ ] Многоярусный уровень + герой + AnimBP
- [ ] HealthComponent + pickables + quest (ключ/дверь)
- [ ] NPC: патруль + взаимодействие + возврат
- [ ] Враг: охрана зоны + атака + возврат
- [ ] GameMode / GameState / PlayerState
- [ ] Респавн, чекпоинты, завершение уровня
- [ ] MR + LMS

### Nice to have (максимум)

- [ ] Несколько типов врагов (melee/ranged)
- [ ] Босс с фазами и ловушками
- [ ] Domino Run (практика)

---

## Примерный график (4–6 недель)

| Неделя | Фокус |
|--------|-------|
| 1 | Этап 0 + Domino Run + репо, меню, базовый уровень |
| 2 | Герой, AnimBP, pickables, quest objects |
| 3 | Enemy mesh, NPC + первый враг (AI) |
| 4 | GameMode, GameState, respawn, checkpoints |
| 5 | PlayerState UI, win condition, полировка |
| 6 | Звёздочки, босс, MR, тестирование |

---

## Практические советы

1. **Один основной проект** — Domino Run лучше держать отдельно, чтобы не смешивать с прототипом.
2. **Сначала «скелет», потом красота** — примитивы и Mannequin; полировка в конце.
3. **Компоненты переиспользовать** — `HealthComponent` на герое, NPC и врагах.
4. **Чекпоинты закладывать в GameplayFramework** — несколько `PlayerStart` с тегами `Checkpoint_01`, `Checkpoint_02`.
5. **Коммиты маленькими MR** — по этапам, как в задании (Squash при merge).

---

## С чего начать прямо сейчас

1. Выбрать концепцию игры (1 абзац в README).
2. Создать Blueprint-проект UE 5.2 и репозиторий GitLab.
3. Параллельно (если нужна практика) — быстрый проект Domino Run по ТЗ.
4. Реализовать меню + пустой основной уровень — первый коммит и MR.

Если скажешь жанр (шутер, RPG, хоррор и т.д.) и есть ли уже UE 5.2 — могу разложить **конкретные Blueprint-классы, имена ассетов и порядок создания в редакторе** под твою тему.

---

**User**

И так, у меня уже установлена нужная версия. Какой базовый проект лучше взять за основу?

---

**Cursor**

# Какой шаблон взять за основу

Для **основного прототипа** (GameplayFramework → AI → GameMode) лучший выбор:

## **Third Person — Blueprint**

При создании проекта в UE 5.2:

| Параметр | Значение |
|----------|----------|
| **Category** | Games |
| **Template** | **Third Person** |
| **Project Type** | **Blueprint** |
| **Target Platform** | Desktop |
| **Starter Content** | **With Starter Content** (рекомендуется) |
| **Raytracing** | можно выключить |

### Почему именно он

1. **В задании прямо указан Blueprint** — C++ шаблон не нужен.
2. **Нужен Character + AnimBP** — в Third Person уже есть:
   - `ThirdPersonCharacter`
   - Input (WASD, прыжок)
   - камера от третьего лица
   - базовый AnimBP и анимации (Idle / Walk / Run / Jump)
3. **Pickables, враги, NPC** — удобнее на `Character`, чем с нуля на `Pawn`.
4. **AI** — Behavior Tree и Nav Mesh обычно настраивают под Character; шаблон к этому уже близок.
5. **Starter Content** — примитивы и простые меши для многоярусного уровня без тяжёлых 4K-ассетов (как просят в ДЗ).

---

## Что не брать для основного проекта

| Шаблон | Почему не подходит |
|--------|-------------------|
| **Blank** | Слишком много ручной работы: герой, камера, input, анимации — всё с нуля |
| **First Person** | Можно, если задуман именно FPS, но для NPC, диалогов и «ключ + дверь» Third Person проще |
| **Top Down** | Другая камера и логика; не совпадает с типичным 3D action/adventure из заданий |
| **Third Person — C++** | Лишняя сложность; задания не требуют C++ |

---

## Отдельно: «Domino Run» (практика)

Для практического ТЗ лучше **второй, лёгкий проект**:

| Параметр | Значение |
|----------|----------|
| **Template** | **Blank** или **Basic** |
| **Project Type** | **Blueprint** |
| **Starter Content** | **No Starter Content** (не обязательно) |

Там нужны только меню, Landscape, шар и кубики — Third Person Character не нужен.

---

## После создания Third Person — что переименовать/настроить

Сразу после создания проекта:

1. **Project Settings → Maps & Modes**
   - позже заменить дефолтную карту на свой `MainMenu`
   - свой `GM_MainMenu` и `GM_Game`

2. **Не удалять** `ThirdPersonCharacter` — использовать как базу для `BP_PlayerCharacter`.

3. **Content Browser** — завести структуру из задания:
   ```
   Content/
   ├── Maps/
   ├── Blueprints/
   │   ├── Character/
   │   ├── AI/
   │   ├── Pickables/
   │   └── Game/
   └── Widgets/
   ```

4. **Дефолтный уровень** шаблона (`ThirdPersonMap`) — можно временно использовать как основной, потом сделать свой `Lvl_Game`.

---

## Краткий итог

- **Один главный проект:** `Third Person` + `Blueprint` + `With Starter Content`
- **Domino Run (если отдельно):** `Blank` + `Blueprint`

Если скажешь, какой жанр хочешь (шутер, RPG, приключение), подскажу, оставлять ли Third Person как есть или сразу переключать на First Person.

---

**User**

С чем ты можешь мне помочь при выполнении данного задания?

---

**Cursor**

# Чем могу помочь с заданием

Я не могу нажимать кнопки в Unreal Editor за тебя, но могу сопровождать работу почти на всех остальных этапах — от идеи до сдачи.

---

## Планирование и структура

- Разбить задание на шаги и чеклисты под **GameplayFramework**, **AI**, **GameMode**, **Domino Run**
- Предложить концепцию игры, win condition, связку «ключ → дверь → чекпоинт → финал»
- Спроектировать имена Blueprint-классов, папок в `Content/`, веток Git и порядок MR

---

## Unreal Engine (логика и настройка)

- Пошаговые инструкции: что создать в редакторе и в каком порядке
- **Blueprint**: Event Graph, компоненты, интерфейсы (`BPI_Interactable`), наследование
- **UI**: `WBP_MainMenu`, HUD, экран статистики по Tab, экран победы
- **GameMode / GameState / PlayerState**: что где хранить и как связать
- **AI**: Behavior Tree, Blackboard, состояния (патруль / атака / возврат), зоны охраны
- **Pickables**: collectables, equipables, quest-объекты (ключ, дверь)
- **Domino Run**: координаты, физика, Landscape, настройки CCD/collision
- Разбор ошибок: если пришлёшь текст из Output Log или скрин — помогу найти причину

---

## Файлы проекта (то, что могу делать напрямую)

- `README.md`, описание игры для GitLab
- `.gitignore` для Unreal
- П правки в `Config/*.ini` (карты по умолчанию, input и т.д.)
- **C++**, если решишь расширять Blueprint-кодом (компоненты, интерфейсы)
- Документация, чеклисты, текст для MR и LMS

---

## Git и сдача

- Структура репозитория, что коммитить, а что нет
- Тексты для merge request (название, описание «что сделано»)
- Список файлов для архива Domino Run

---

## Когда помощь будет максимально полезной

| Ты присылаешь | Я делаю |
|---------------|---------|
| «Делаю меню, застрял на шаге X» | Конкретные ноды и настройки |
| Скрин Blueprint / ошибка из лога | Диагностика и исправление |
| «Как связать GameState и pickables?» | Схема + пошаговая реализация |
| «Проверь, всё ли по ТЗ» | Чеклист по критериям из `Задание/` |

---

## Чего я не заменю

- Работу внутри редактора (расставить домино, нарисовать ландшафт, настроить AnimBP вручную)
- Импорт ассетов с Marketplace / Mixamo (но подскажу, что выбрать и как импортировать)
- Запуск UE на твоём ПК — только команды и инструкции

---

## С чего логично начать вместе

1. **Создание проекта** Third Person Blueprint — проверим настройки после создания  
2. **Структура `Content/`** и первый MR  
3. **Главное меню** — самый быстрый видимый результат  

Напиши, на каком ты этапе сейчас: проект уже создан или только собираешься создавать — и с чего хочешь начать (меню, уровень, герой, Domino Run). Пойдём по шагам.

---

**User**

И так, я создал базовый проект, создал репозиторий в Git, что следует сделать дальше?

---

**Cursor**

Проверю текущее состояние проекта и репозитория — от этого зависит следующий шаг.


Проект **Homework** (UE 5.2, Third Person) и первый коммит уже есть — это хорошая база. Дальше идём по **ДЗ GameplayFramework** в логичном порядке.

---

## Сейчас у тебя

| Готово | Ещё нет |
|--------|---------|
| Проект `Homework/Homework.uproject` | `.gitignore` для Unreal |
| Third Person + Starter Content | `README` с описанием игры |
| Initial commit | Ветка для текущего ДЗ |
| | Remote + push (если ещё не делал) |
| | Структура `Content/` и главное меню |

**Важно:** в репозиторий попали `Homework/Saved/` и `Homework/Intermediate/` — их обычно **не коммитят**. Лучше поправить до первого push.

---

## Шаг 1. Git (15–20 мин)

### 1.1 Добавь `.gitignore` в корень репозитория

Минимум для UE:

```gitignore
# Unreal
**/Binaries/
**/DerivedDataCache/
**/Intermediate/
**/Saved/
**/.vs/
**/.idea/

# OS
Thumbs.db
Desktop.ini
```

### 1.2 Убери лишнее из Git (файлы останутся на диске)

```powershell
cd d:\Unreal_Ptoject
git rm -r --cached Homework/Saved Homework/Intermediate
git add .gitignore
git commit -m "Add Unreal gitignore and stop tracking generated folders"
```

### 1.3 Подключи remote и запушь

```powershell
git remote add origin <URL твоего репозитория>
git push -u origin main
```

### 1.4 Создай ветку для ДЗ

```powershell
git checkout -b feature/gameplay-framework
```

По заданию работа сдаётся через MR — ветка нужна сразу.

---

## Шаг 2. README и концепция игры (30 мин)

В задании прямо требуется **описание проекта**. Создай `README.md` в корне репозитория:

- название игры;
- жанр (шутер / приключение / RPG);
- цель игрока;
- как пройти уровень (например: собрать ключ → открыть дверь → дойти до выхода).

Пример структуры:

```markdown
# Название игры

Краткое описание (2–3 абзаца).

## Стек
- Unreal Engine 5.2 (Blueprint)
- Third Person template

## Структура Content
- Maps — уровни
- Widgets — UI
- Blueprints — логика
```

После этого — коммит: `Add project README`.

---

## Шаг 3. Структура Content (10 мин, в редакторе)

В **Content Browser** создай папки:

```
Content/
├── Maps/
├── Widgets/
└── Blueprints/
    ├── Character/
    ├── Game/
    ├── Pickables/
    └── AI/          (пока пустая, пригодится позже)
```

Шаблонные `ThirdPerson/` и `StarterContent/` можно не трогать — новые ассеты клади в свои папки.

---

## Шаг 4. Главное меню — первый результат (1–2 часа)

Это первый блок из задания, который сразу видно в игре.

| # | Что создать | Имя |
|---|-------------|-----|
| 1 | Уровень | `Content/Maps/Lvl_MainMenu` |
| 2 | Widget Blueprint | `Content/Widgets/WBP_MainMenu` |
| 3 | GameMode Blueprint | `Content/Blueprints/Game/GM_MainMenu` |

**WBP_MainMenu** — кнопки:
- «Начать игру» → Open Level → `Lvl_Game` (пока можно `ThirdPersonMap`)
- «Выйти из игры» → Quit Game
- «О игре» → показать текст (overlay или второй виджет)

**Lvl_MainMenu → Level Blueprint:**
1. Event BeginPlay  
2. Create Widget (`WBP_MainMenu`)  
3. Add to Viewport  
4. Set Input Mode UI Only  
5. Show Mouse Cursor = True  

**Project Settings → Maps & Modes:**
- Editor Startup Map = `Lvl_MainMenu`
- Game Default Map = `Lvl_MainMenu`

**GM_MainMenu:** на уровне меню в World Settings → GameMode Override = `GM_MainMenu`.

Коммит: `Add main menu level and widget`.

---

## Шаг 5. Основной уровень (следующий MR-блок)

После меню:

1. `Content/Maps/Lvl_Game` — многоярусный уровень (кубы, лестницы, платформы из Starter Content).
2. Несколько **PlayerStart** на разных ярусах.
3. Кнопка «Начать игру» ведёт на `Lvl_Game`.
4. Свой **GameMode** для игрового уровня (пока можно оставить `BP_ThirdPersonGameMode`).

---

## Шаг 6. Герой и компоненты

На базе `BP_ThirdPersonCharacter`:

- Duplicate → `BP_PlayerCharacter`
- Actor Component → `AC_HealthComponent` (HP, урон, смерть)
- Позже — AnimBP (в шаблоне уже есть базовые анимации)

---

## Шаг 7. Pickables

- `BP_Pickable_Base`
- Collectable (аптечка, монета)
- Quest (ключ + дверь)

---

## Порядок на ближайшие 2–3 дня

```
День 1:  .gitignore + README + ветка + push
День 2:  Главное меню (уровень + виджет + GameMode)
День 3:  Lvl_Game + PlayerStart + переход из меню
Далее:   герой → HealthComponent → pickables → MR
```

---

## Критерий «можно двигаться дальше»

После шагов 1–4 у тебя должно быть:

- [ ] Репозиторий на GitLab без `Saved/` и `Intermediate/`
- [ ] README с описанием игры
- [ ] Ветка `feature/gameplay-framework`
- [ ] При запуске игры открывается меню с 3 кнопками
- [ ] «Начать игру» загружает уровень
- [ ] «Выйти» закрывает игру

---

Могу помочь прямо сейчас с любым из пунктов: например, подготовить `.gitignore` и `README.md` в репозитории или пошагово расписать Blueprint главного меню (какие ноды соединять). Напиши, с чего хочешь начать.

---

**User**

Выполни шаги 1, 2 и 3, но пока не делай никаких пушев и коммитов.

---

**Cursor**

Выполняю шаги 1–3: добавлю `.gitignore`, `README.md` и структуру папок в `Content/` — без коммитов и пушей.


Готово. Коммитов и пушей не было.

---

## Шаг 1 — Git

Создан `.gitignore` в корне репозитория. Исключаются:

- `Binaries/`, `Intermediate/`, `Saved/`, `DerivedDataCache/`
- файлы IDE, логи, OS-мусор

**Важно:** `Homework/Saved/` и `Homework/Intermediate/` всё ещё в истории Git из initial commit. Перед следующим коммитом выполни:

```powershell
cd d:\Unreal_Ptoject
git rm -r --cached Homework/Saved Homework/Intermediate
```

Это уберёт их из индекса; `.gitignore` не даст закоммитить их снова.

---

## Шаг 2 — README

Создан `README.md` с концепцией игры **Ruin Explorer**:

- 3D action-adventure от третьего лица
- цель: ключ → дверь → выход
- описание стека, структуры `Content`, порядка сдачи

Если хочешь другую тему (шутер, хоррор, RPG) — напиши, поправлю README.

---

## Шаг 3 — Структура Content

Созданы папки (с `.gitkeep`, чтобы Git их видел):

```
Homework/Content/
├── Maps/
├── Widgets/
└── Blueprints/
    ├── Character/
    ├── Game/
    ├── Pickables/
    └── AI/
```

В Unreal: открой проект → **Content Browser → правый клик → Refresh**, папки должны появиться. Файлы `.gitkeep` в редакторе не видны — это нормально.

---

## Текущий статус Git (не закоммичено)

| Файл | Статус |
|------|--------|
| `.gitignore` | новый |
| `README.md` | новый |
| `Homework/Content/Maps/`, `Widgets/`, `Blueprints/` | новые |
| `Homework/Saved/Logs/Homework.log` | изменён (после `.gitignore` не попадёт в новые коммиты) |

---

**Следующий шаг — шаг 4:** главное меню (`Lvl_MainMenu`, `WBP_MainMenu`, `GM_MainMenu`). Могу расписать пошагово в редакторе или помочь с коммитом, когда скажешь.

---

**User**

Шаг 4, распиши последовательно, что надо сделать.

---

**Cursor**

# Шаг 4: Главное меню — пошаговая инструкция

Цель: при запуске игры открывается меню с тремя кнопками; «Начать игру» загружает уровень, «Выйти» закрывает игру, «О игре» показывает описание.

Используй папки, которые уже созданы: `Content/Maps/`, `Content/Widgets/`, `Content/Blueprints/Game/`.

---

## Часть A. Уровень главного меню

### 1. Создай уровень

1. Открой проект `Homework.uproject`.
2. **File → New Level → Empty Level** (или Basic — без лишней геометрии).
3. **File → Save Current Level As…**
4. Путь: `Content/Maps/Lvl_MainMenu`.
5. Сохрани.

### 2. Минимальное оформление (по желанию)

- Добавь **Directional Light** и **Sky Atmosphere** (или **Sky Light**), чтобы уровень не был чёрным.
- Можно поставить камеру/декоративный mesh из Starter Content — необязательно для зачёта.

### 3. GameMode для меню

1. **Content Browser → Blueprints/Game → правый клик → Blueprint Class**.
2. В поиске выбери **Game Mode Base** → назови `GM_MainMenu`.
3. Открой `GM_MainMenu`.
4. В **Class Defaults**:
   - **Default Pawn Class** → `None` (на меню персонаж не нужен).
   - **Player Controller Class** → оставь `PlayerController` (дефолт).
   - **HUD Class** → `None` (если есть лишний HUD).
5. **Compile → Save**.

### 4. Привяжи GameMode к уровню

1. Открой `Lvl_MainMenu`.
2. **Window → World Settings** (или кнопка в toolbar).
3. **GameMode Override** → `GM_MainMenu`.
4. Сохрани уровень (**Ctrl+S**).

---

## Часть B. Виджет главного меню

### 5. Создай Widget Blueprint

1. **Content/Widgets → правый клик → User Interface → Widget Blueprint**.
2. Имя: `WBP_MainMenu`.
3. Открой в **Designer**.

### 6. Разметка (Designer)

Добавь на Canvas (иерархия примерная):

```
Canvas Panel
├── Vertical Box (центр экрана, Anchors: middle-center)
│   ├── Text Block          → текст: "RUIN EXPLORER" (заголовок)
│   ├── Button "Btn_Start"  → текст: "Начать игру"
│   ├── Button "Btn_About"  → текст: "О игре"
│   └── Button "Btn_Quit"   → текст: "Выйти из игры"
└── Overlay "Overlay_About" (на весь экран, Visibility: Collapsed)
    └── Border / Background
        ├── Text Block      → описание игры
        └── Button "Btn_CloseAbout" → "Закрыть"
```

**Настройки:**

- У кнопок включи **Is Variable** (галочка в Details) — имена: `Btn_Start`, `Btn_About`, `Btn_Quit`, `Btn_CloseAbout`.
- У `Overlay_About` тоже **Is Variable**.
- Текст «О игре» (можно скопировать из README):

  > Добро пожаловать в Ruin Explorer!  
  > Исследуй руины, собирай предметы, общайся с NPC и найди выход с уровня.

### 7. Логика кнопок (Graph)

Перейди на вкладку **Graph**.

#### Кнопка «Начать игру»

1. Выбери `Btn_Start` → **+ On Clicked**.
2. Цепочка:

```
Btn_Start → OnClicked
    → Open Level (by Name)
        Level Name: ThirdPersonMap
```

> Пока основной уровень ещё не готов — используй `ThirdPersonMap` из шаблона.  
> Когда создашь `Lvl_Game`, замени имя на `Lvl_Game`.

**Альтернатива:** **Open Level by Object Reference** — если хранишь soft reference на уровень.

#### Кнопка «Выйти из игры»

```
Btn_Quit → OnClicked
    → Quit Game
```

#### Кнопка «О игре»

```
Btn_About → OnClicked
    → Set Visibility (Overlay_About)
        Visibility: Visible
```

#### Кнопка «Закрыть»

```
Btn_CloseAbout → OnClicked
    → Set Visibility (Overlay_About)
        Visibility: Collapsed
```

3. **Compile → Save**.

---

## Часть C. Подключение меню на уровне

### 8. Level Blueprint для `Lvl_MainMenu`

1. Открой `Lvl_MainMenu`.
2. **Blueprints → Open Level Blueprint**.
3. Собери цепочку:

```
Event BeginPlay
    → Create Widget
        Class: WBP_MainMenu
    → Add to Viewport
        Target: Return Value (widget)
    → Get Player Controller (Index 0)
    → Set Input Mode UI Only
        Widget to Focus: Return Value (widget)
        Lock Mouse to Viewport: false
    → Set Show Mouse Cursor (на Player Controller)
        Show Mouse Cursor: true
```

**Порядок важен:** сначала Create Widget, потом Add to Viewport, затем Input Mode и курсор.

4. **Compile → Save Level**.

---

## Часть D. Стартовая карта проекта

### 9. Project Settings

1. **Edit → Project Settings → Maps & Modes**.
2. **Editor Startup Map** → `Lvl_MainMenu`.
3. **Game Default Map** → `Lvl_MainMenu`.
4. Закрой настройки.

### 10. Проверка игрового уровня (временно)

На `ThirdPersonMap` (куда ведёт Start):

1. Открой `ThirdPersonMap`.
2. **World Settings → GameMode Override** → `BP_ThirdPersonGameMode` (дефолт шаблона).
3. Сохрани.

---

## Часть E. Тестирование

### 11. Play

1. Открой `Lvl_MainMenu`.
2. Нажми **Play (Alt+P)** или **Standalone Game** (лучше для проверки смены уровней).

**Проверь:**

| Действие | Ожидание |
|----------|----------|
| Запуск игры | Открывается меню, виден курсор |
| «Начать игру» | Загружается `ThirdPersonMap`, персонаж управляется |
| «О игре» | Появляется текст, «Закрыть» скрывает |
| «Выйти» | Игра закрывается (в Standalone / packaged build) |

> В **Play in Editor** кнопка Quit иногда не закрывает редактор — это нормально. Для проверки используй **Standalone**.

---

## Часть F. Частые проблемы

| Проблема | Решение |
|----------|---------|
| Нет курсора | `Set Show Mouse Cursor = true` + `Set Input Mode UI Only` |
| Кнопки не нажимаются | Widget добавлен в Viewport? Input Mode UI Only? |
| После Start чёрный экран | Проверь имя уровня в Open Level (без пути, только `ThirdPersonMap`) |
| На меню спавнится персонаж | В `GM_MainMenu` → Default Pawn Class = `None` |
| Меню не при старте | Project Settings → Game Default Map = `Lvl_MainMenu` |

---

## Чеклист готовности шага 4

- [ ] `Content/Maps/Lvl_MainMenu`
- [ ] `Content/Widgets/WBP_MainMenu` (3 кнопки + overlay «О игре»)
- [ ] `Content/Blueprints/Game/GM_MainMenu` (Pawn = None)
- [ ] Level Blueprint: Create Widget → Viewport → UI Input → Cursor
- [ ] Project Settings: стартовая карта = `Lvl_MainMenu`
- [ ] Start ведёт на игровой уровень, Quit и About работают

---

Когда сделаешь — напиши, если что-то не сработает (скрин Blueprint или текст ошибки), разберём. Дальше по плану **шаг 5**: свой `Lvl_Game` вместо `ThirdPersonMap`.

---

**User**

Быстро распиши часть C8, ибо я не вижу в явном виде Get Player Controller (UE 5.2.1 если важно).

---

**Cursor**

# Часть C.8 — Level Blueprint (кратко, UE 5.2.1)

Открой **`Lvl_MainMenu` → Blueprints → Open Level Blueprint → вкладка Event Graph** (не Designer).

---

## Цепочка нод

```
Event BeginPlay
  → Create Widget (Class = WBP_MainMenu)
  → Add to Viewport
  → [получить Player Controller]
  → Set Input Mode UI Only
  → Set Show Mouse Cursor
```

---

## Как найти Player Controller

**Способ 1 (основной):**

1. ПКМ на графе → в поиске сними галочку **Context Sensitive** (если нода не находится).
2. Введи: `Get Player Controller`
3. Нода: **Get Player Controller** (не "From Platform User", если путает).
4. **Player Index** = `0`.

**Способ 2 (если всё равно не видно):**

1. ПКМ → `player controller` или `controller`
2. Или: **Get Player Controller from Platform User** → **Platform User Index** = `0` (UE 5.2 тоже ок).

**Способ 3 (через виджет):**

1. От **Return Value** у `Create Widget` → потянуть провод → в поиске: `Get Player Controller` или **Get Owning Player** → **Get Player Controller**.

---

## Подключения

| От | К |
|----|---|
| `Create Widget` → Return Value | `Add to Viewport` → **Target** (сам виджет) |
| `Create Widget` → Return Value | `Set Input Mode UI Only` → **In Widget to Focus** |
| `Get Player Controller` → Return Value | `Set Input Mode UI Only` → **Target** (сверху у ноды) |
| `Get Player Controller` → Return Value | `Set Show Mouse Cursor` → **Target** |
| `Set Show Mouse Cursor` → **Show Mouse Cursor** | ✅ `true` |

`Set Input Mode UI Only`:
- **Lock Mouse to Viewport** = `false`

---

## Порядок exec (белые стрелки)

```
BeginPlay → Create Widget → Add to Viewport → Set Input Mode UI Only → Set Show Mouse Cursor
```

Player Controller подключай **до** Set Input Mode (синие пины), exec-цепочка — как выше.

---

## Если нода не ищется

- Ты точно в **Level Blueprint**, не в `WBP_MainMenu`?
- Вкладка **Event Graph**, не Designer?
- **Context Sensitive** выключен?
- Попробуй английские названия: `Get Player Controller`, `Set Input Mode UI Only`, `Set Show Mouse Cursor`.

---

## Мини-схема

```
[Event BeginPlay]
       |
[Create Widget: WBP_MainMenu]---(widget)---→ [Add to Viewport]
       |                         |
       |                         +---→ [Set Input Mode UI Only] ←---(PC)--- [Get Player Controller (0)]
       |                                    |
       +------------------------------------→ [Set Show Mouse Cursor] ←---(PC)---
```

Compile → Save Level → Play на `Lvl_MainMenu`.

Если застрянешь — скинь скрин графа, подскажу по месту.

---

**User**

У меня открывается сцена при начале игры, но персонажем я не управляю. 

---

**Cursor**

**«Сцена есть, управления нет»** — чаще всего одна из трёх причин. Разберём по порядку.

---

## 1. Ты на главном меню — это нормально

На `Lvl_MainMenu` в `GM_MainMenu` стоит **Default Pawn Class = None** → персонаж **не спавнится**, WASD не работают.

**Проверка:** есть ли на экране кнопки меню?

- **Да** → нажми **«Начать игру»**. Управление должно быть уже на игровом уровне.
- **Нет** → виджет не добавился (Level Blueprint `Lvl_MainMenu`).

---

## 2. После «Начать игру» — залип Input Mode (самое частое)

На меню ты ставил **Set Input Mode UI Only** и **Show Mouse Cursor = true** на **Player Controller**.

Controller **переживает** `Open Level`, поэтому на `ThirdPersonMap` может остаться режим UI → персонаж есть, но не управляется.

### Исправление — Level Blueprint игрового уровня

1. Открой **`ThirdPersonMap`** (или свой `Lvl_Game`).
2. **Blueprints → Open Level Blueprint**.
3. На **Event BeginPlay**:

```
Event BeginPlay
  → Get Player Controller (0)
  → Set Input Mode Game Only
  → Set Show Mouse Cursor (false)
```

4. **World Settings** на этом уровне:
   - **GameMode Override** = `BP_ThirdPersonGameMode` (не `GM_MainMenu`!)

5. Compile → Save → Play с меню: Start → проверь WASD.

---

## 3. На игровом уровне нет персонажа

Видишь уровень, но **нет модели** игрока.

**World Settings** на `ThirdPersonMap`:

| Параметр | Значение |
|----------|----------|
| GameMode Override | `BP_ThirdPersonGameMode` |
| *(в GameMode)* Default Pawn Class | `BP_ThirdPersonCharacter` |

Если там `GM_MainMenu` или Pawn = None — персонаж не появится.

---

## Быстрая диагностика

| Что видишь | Причина | Что сделать |
|------------|---------|-------------|
| Меню + 3D фон, без героя | Меню, pawn не нужен | Жми «Начать игру» |
| Уровень + герой, не двигается | UI Input после меню | BeginPlay на игровом уровне → Game Only |
| Уровень без героя | Неверный GameMode | `BP_ThirdPersonGameMode` |
| Play с открытого `Lvl_MainMenu` | Ожидаемо нет управления | Тест: Play → Start, или Play с `ThirdPersonMap` |

---

## Как тестировать

1. **Project Settings → Game Default Map** = `Lvl_MainMenu`.
2. **Play** → **Start**.
3. Должен загрузиться `ThirdPersonMap`, персонаж двигается.

**Standalone Game** (не PIE на меню) — надёжнее для проверки смены уровней.

---

Напиши, что именно у тебя:

1. **Меню с кнопками** или сразу игровой уровень?  
2. **Персонаж на экране есть**, но стоит на месте?

Если есть — почти наверняка пункт **2** (Input Mode на `ThirdPersonMap`).

---

**User**

И так, теперь всё работает. Какую часть задания мы уже сделали?

---

**Cursor**

# Прогресс по заданию

Речь про **«Домашнее задание GameplayFramework»** — первое ДЗ цепочки. Остальные (AI, GameMode, Domino Run) **ещё не начинались**.

---

## Общий прогресс: ~35–40%

```
[████████░░░░░░░░░░░░]  GameplayFramework
[░░░░░░░░░░░░░░░░░░░░]  AI
[░░░░░░░░░░░░░░░░░░░░]  GameMode
[░░░░░░░░░░░░░░░░░░░░]  Domino Run (практика)
```

---

## ✅ Уже сделано

### Инфраструктура
| Пункт | Статус |
|-------|--------|
| Проект UE 5.2 Blueprint (Third Person) | ✅ |
| Репозиторий Git | ✅ (initial commit) |
| `.gitignore` | ✅ создан (в коммит ещё не входил) |
| `README.md` с описанием игры | ✅ |
| Структура папок `Content/` | ✅ |

### Блок «Для зачёта предыдущих ДЗ» — **полностью**
| Пункт | Статус |
|-------|--------|
| Уровень главного меню | ✅ `Lvl_MainMenu` |
| Виджет с 3 кнопками | ✅ `WBP_MainMenu` |
| GameMode меню | ✅ `GM_MainMenu` |
| «Начать игру» → игровой уровень | ✅ (пока `ThirdPersonMap`) |
| «Выйти из игры» | ✅ |
| «О игре» | ✅ |
| Стартовая карта = меню | ✅ |
| Управление после перехода | ✅ (исправлен Input Mode) |

---

## ⚠️ Частично / от шаблона

| Пункт | Сейчас | Что нужно по ТЗ |
|-------|--------|-----------------|
| Игровой уровень | `ThirdPersonMap` из шаблона | Свой **`Lvl_Game`**, многоярусный |
| Герой | `BP_ThirdPersonCharacter` | Свой **`BP_PlayerCharacter`** + компоненты |
| AnimBP | Есть в шаблоне (Mannequin) | Свой AnimBP или импорт анимаций |
| GitLab push / ветка / MR | Не сделано | Ветка + push + MR |

---

## ❌ Ещё не сделано (GameplayFramework)

### Основной уровень
- [ ] Отдельный `Lvl_Game`
- [ ] Static Mesh / примитивы, многоярусность
- [ ] Несколько `PlayerStart` на разных ярусах

### Герой
- [ ] Свой Character (на базе шаблона)
- [ ] `HealthComponent` (Actor Component)
- [ ] AnimBP под своего героя (или явно свой на базе импорта)

### PickableActor
- [ ] Взаимодействие с объектами (E / raycast)
- [ ] Подбираемые предметы (аптечка, collectable)
- [ ] Квестовые объекты (ключ + дверь)

### Максимальная оценка
- [ ] Заготовка противника: Pawn/Character + Mesh + AnimBP (без AI)

### Сдача
- [ ] Коммиты, push
- [ ] Ветка `feature/gameplay-framework`
- [ ] MR в GitLab (Squash) + ссылка в LMS
- [ ] Убрать `Saved/` и `Intermediate/` из Git перед push

---

## Что логично делать дальше

**Шаг 5** — свой игровой уровень `Lvl_Game` и переключение кнопки Start с `ThirdPersonMap` на него.

Потом по порядку:
1. `BP_PlayerCharacter` + `HealthComponent`
2. Pickables (ключ, дверь, аптечка)
3. Заготовка врага (mesh + AnimBP)
4. Git: коммит, ветка, push, MR

---

**Итог:** закрыт блок **«цели задания»** (кроме ветки/push) и **полностью блок «главное меню»**. Основная работа — **уровень, герой, pickables, сдача** — впереди.

Если хочешь — распишу **шаг 5** (`Lvl_Game`) так же пошагово, как меню.

