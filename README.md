# Лабораторная работа №33-34. Полноценный CRUD с базой данных

## Описание проекта

**NotesApp** - приложение для управлениями заметками и категориями. Проект разработан в рамках учебного проекта лабораторных работ №33-34.

### Технологии

* ASP.NET Core
* Entity Framework Core
* Swagger
* C#

## Структура проекта 

Lab33-34_FullCRUD
├── img
│   ├── gitPushLab33-34_Morozova.png
│   ├── step6_migrationLab33-34_Morozova.png
│   ├── step7_categoriesLab33-34_Morozova.png
│   ├── step8_notesLab33-34_GET_Morozova.png
│   └── step8_notesLab33-34_POST_Morozova.png
├── NotesApp/
│   ├── Controllers/
│   │   ├── CategoriesController.cs
│   │   └── NotesController.cs
│   ├── Data/
│   │   └── AppDbContext.cs
│   ├── Helpers/
│   │   └── ApiResponse.cs 
│   ├── Models/
│   │   ├── Category.cs
│   │   ├── Note.cs
│   │   └── DTOs/
│   │       ├── CategoryDtos.cs
│   │       ├── NoteFilterDto.cs
│   │       └── NoteDtos.cs
│   ├── Repositories/
│   │   ├── ICategoryRepository.cs
│   │   ├── CategoryRepository.cs 
│   │   ├── INoteRepository.cs
│   │   └── NoteRepository.cs
│   ├── .gitignore
│   ├── appsettings.Development.json
│   ├── appsettings.json
│   ├── NotesApp.csproj
│   ├── notesapp.db
│   ├── NotesApp.http
│   └── Program.cs
├── .editorconfig
├── Lab33-34_FullCRUD.sln
└── README.md

## Описание паттерна Repository

**Repository** — это паттерн проектирования, который изолирует логику работы с данными от логики контроллера.

**Без Repository:** Контроллер → напрямую работает с DbContext → База данных

**С Repository:** Контроллер → Repository → DbContext → База данных

**Зачем это нужно:**

| Проблема без Repository                        | Решение с Repository                    |
| ---------------------------------------------- | --------------------------------------- |
| Логика запросов к БД размазана по контроллерам | Все запросы в одном месте               |
| Трудно тестировать контроллер отдельно от БД   | Можно подменить репозиторий на тестовый |
| При смене БД нужно менять все контроллеры      | Меняется только репозиторий             |
| Дублирование одинаковых запросов               | Переиспользуемые методы                 |

## Таблица маршрутов

| Метод  | URL                        | Описание                        | Коды ответа   |
| ------ | -------------------------- | ------------------------------- | ------------- |
| GET    | /api/categories            | Все категории с кол-вом заметок | 200           |
| GET    | /api/categories/{id}       | Одна категория                  | 200, 404      |
| GET    | /api/categories/{id}/notes | Категория с заметками           | 200, 404      |
| POST   | /api/categories            | Создать категорию               | 201, 400      |
| PUT    | /api/categories/{id}       | Обновить категорию              | 200, 400, 404 |
| DELETE | /api/categories/{id}       | Удалить категорию               | 204, 400, 404 |
| GET    | /api/notes                 | Все заметки с фильтрами         | 200           |
| GET    | /api/notes/{id}            | Одна заметка                    | 200, 404      |
| POST   | /api/notes                 | Создать заметку                 | 201, 400      |
| PUT    | /api/notes/{id}            | Обновить заметку                | 200, 400, 404 |
| PATCH  | /api/notes/{id}/pin        | Закрепить / открепить           | 200, 404      |
| PATCH  | /api/notes/{id}/archive    | Архивировать / восстановить     | 200, 404      |
| DELETE | /api/notes/{id}            | Удалить заметку                 | 204, 404      |

## Главные выводы

1. Паттерн Repository — не лишняя абстракция, а способ держать код управляемым при росте проекта.
2. Data Annotations решают двойную задачу: валидируют входные данные и задают ограничения в БД.
3. Единый формат ответа (ApiResponse) упрощает жизнь фронтенду — он всегда знает, что ждать от сервера.
4. DeleteBehavior.Restrict защищает данные от случайного каскадного удаления.
5. Include() и проекция в DTO через Select() — правильный способ получить связанные данные без N+1 проблемы.
