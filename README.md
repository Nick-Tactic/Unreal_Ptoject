# Ruin Explorer

**Ruin Explorer** — прототип 3D-приключения от третьего лица на Unreal Engine 5.2 (Blueprint).

Игрок исследует многоярусные руины: собирает предметы, общается с NPC, сражается с охранниками и ищет выход с уровня. Прототип создаётся в рамках курса по геймдизайну и последовательно расширяется домашними заданиями (Gameplay Framework → AI → GameMode).

## Геймплей (концепция)

- **Жанр:** action-adventure, third person.
- **Цель игрока:** найти ключ, открыть запечатанную дверь и добраться до финальной зоны.
- **На пути:** аптечки и collectables, дружелюбный NPC (может выдать подсказку или предмет), враги, охраняющие ключевые зоны.
- **Победа:** игрок достигает выхода с уровня после выполнения условий (ключ, дверь, финальная точка).

## Стек

- Unreal Engine **5.2**
- Шаблон **Third Person (Blueprint)**
- Starter Content + Mannequin

## Структура Content

| Папка | Назначение |
|-------|------------|
| `Content/Maps/` | Уровни (`Lvl_MainMenu`, `Lvl_Game`, …) |
| `Content/Widgets/` | UI (главное меню, HUD, экраны) |
| `Content/Blueprints/Character/` | Игрок, компоненты (Health и т.д.) |
| `Content/Blueprints/Game/` | GameMode, GameState, PlayerState |
| `Content/Blueprints/Pickables/` | Подбираемые и квестовые объекты |
| `Content/Blueprints/AI/` | NPC и враги (Behavior Tree, AI Controller) |

Шаблонные ассеты (`ThirdPerson/`, `StarterContent/`, `Characters/`) остаются без изменений — новая логика и контент создаются в папках выше.

## Запуск проекта

1. Открыть `Homework/Homework.uproject` в Unreal Engine 5.2.
2. Дождаться компиляции шейдеров при первом запуске.
3. Play — стартовый уровень задаётся в **Project Settings → Maps & Modes**.

## Репозиторий

- Ветка для Gameplay Framework: `feature/gameplay-framework`
- Сдача работы: Merge Request в GitLab (Squash commits) + ссылка в LMS
