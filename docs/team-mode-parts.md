Функция «Командный режим» разбита на 3 независимые части:

1. Создание и управление (Team Service)
Капитан создает команду, система генерирует ID.
Данные: таблицы teams (id, name, captain_id) и team_members (team_id, user_id).
Ветка Git: feature/team-service-create.

2. Аутентификация по коду (Access/Auth Service)
Валидация введенного кода, привязка session_id к team_id.
Данные: таблица team_codes (code_hash, team_id, expires_at).
Ветка Git: feature/join-by-code-validation.

3. Игровой прогресс и счёт (Game/Score Service)
Прием ответов с станций, расчет общего счёта команды.
Данные: таблицы attempts и team_scores.
Ветка Git: feature/station-submit.

Зависимости:
Части 2 и 3 зависят от ID, созданного в части 1. Счёт (часть 3) не может работать без валидной сессии из части 2.

Главный риск:
Гонка данных (race condition) при одновременной отправке ответов разными участниками на одной станции. Требуется атомарное обновление счёта на бэкенде.
