# MDiSUBD

# Лебецкий Дмитрий, 153501

# Тема разрабатываемого проекта - Дневник настроения

# Функциональные требования

1. Авторизация пользователя
2. Реализовать систему ролей, включая две роли: участник (Member) и консультант
    1. Участник (Member):
        1. Могут создавать и управлять дневниками (CRUD)
        2. Могут создавать и управлять своими записями о эмоциях (CRUD).
        3. Управление тегами к записям (CRUD)
        4. Могут периодически (ежедневно/еженедельно/ежемесячно) проходить тесты (measurements) для оценки своего состояния.
        5. Возможность фильтровать и искать записи по дате, типу эмоции, тегу и другим параметрам.
    2. Консультант:
        1. Могут добавлять комментарии к записям об эмоциях других пользователей.
        2. Добавление ресурсов по самопомощи (CRUD).
3. Журналирование действий пользователя


# Сущности

1. Дневник
    - Таблица: diary
    - Поля:
        - diary_id: PRIMARY_KEY
        - member_id: FOREIGN_KEY
        - name: VARCHAR
    - Связи:
        - один-ко-многим с emotion_record
        - многие-к-одному с member

2. Пользователь
    - Таблица: user
    - Поля:
        - user_id: PRIMARY_KEY
        - username: VARCHAR
        - password: VARCHAR
        - first_name: VARCHAR
        - last_name: VARCHAR
    - Связи:
        - один-к-одному с member
        - один-к-одному с counselor
        - один-ко-многим с activity_log

3. Участник
    - Таблица: member
    - Поля:
        - member_id: PRIMARY_KEY
        - user_id: FOREIGN_KEY
    - Связи:
        - один-к-одному с user
        - один-ко-многим с diary
        - один-ко-многим с tag
        - один-ко-многим с measurement

4. Консультант:
    - Таблица: counselor
    - Поля:
        - counselor_id: PRIMARY_KEY
        - user_id: FOREIGN_KEY        
    - Связи:
        - один-к-одному с user
        - один-ко-многим с advice_resource
        - один-ко-многим с comment

5. Запись об эмоции
    - Таблица: emotion_record
    - Поля:
        - record_id: PRIMARY_KEY
        - diary_id: FOREIGN_KEY
        - category_id: FOREIGN_KEY
        - date: DATETIME
        - emotion: VARCHAR
        - content: TEXT
    - Связи:
        - многие-к-одному с diary
        - многие-ко-многим с tag
        - один-ко-многим с comment
        - многие-к-одному с emotion_category

6. Тег
    - Таблица: tag
    - Поля:
        - tag_id: PRIMARY_KEY
        - member_id: FOREIGN_KEY
        - name: VARCHAR
    - Связи:
        - многие-ко-многим с emotion_record
        - многие-к-одному с member

7. Совет или ресурс
    - Таблица: advice_resource
    - Поля:
        - resource_id: PRIMARY_KEY
        - counselor_id: FOREIGN_KEY
        - title: VARCHAR
    - Связи:
        - один-к-одному с video_resource
        - один-к-одному с article_resource
        - многие-к-одному с counselor

8. Видео ресурс
    - Таблица: video_resource
    - Поля:
        - video_resource_id: PRIMARY_KEY
        - resourse_id: FOREIGN_KEY
        - url: VARCHAR 
    - Связи:
        - один-к-одному с advice_resource

9. Статья (ресурс)
    - Таблица: article_resource
    - Поля:
        - article_resource_id: PRIMARY_KEY
        - resourse_id: FOREIGN_KEY
        - content: TEXT
    - Связи:
        - один-к-одному с advice_resource

10. Комментарий
    - Таблица: comment
    - Поля:
        - comment_id: PRIMARY_KEY
        - record_id: FOREIGN_KEY
        - counselor_id: FOREIGN_KEY
        - content: TEXT
    - Связи:
        - многие-к-одному с counselor
        - многие-к-одному с emotion_record

11. Категория эмоции (их пять базовых)
    - Таблица: emotion_category
    - Поля:
        - category_id: PRIMARY_KEY
        - name: VARCHAR
    - Связи:
        - один-ко-многим с emotion_record

12. Измерение
    - Таблица: measurement
    - Поля:
        - measurement_id: PRIMARY_KEY
        - member_id: FOREIGN_KEY
        - value: DECIMAL
        - date: DATETIME
    - Связи:
        - многие-к-одному с member

13. Журнал действий
    - Таблица: activity_log
    - Поля:
        - log_id: PRIMARY_KEY
        - user_id: FOREIGN_KEY
        - action_type: VARCHAR
        - date: DATETIME
    - Связи:
        - многие-к-одному с user

# diagram - https://drawsql.app/teams/demetron-oficial/diagrams/diagramma-lab1
